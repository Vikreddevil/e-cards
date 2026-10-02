# 06 — Logging, Monitoring, Alerting and Analytics

**Status:** Draft v1 · **As of:** 2 October 2026 · **Owner:** Engineering + Product Analytics
Labels: **[V]** sourced · **[E]** estimate · **[A]** assumption.

---

## 1. Log tables (database-backed, for analysis and debugging)

**Two layers:**
1. **Structured application logs** (JSON to stdout → Grafana Loki). These cover everything, are kept 30 days, and are used for debugging.
2. **Business log tables in Postgres** (below). These are queryable by support, finance and product, are kept hot for 90 days, then archived to S3 Parquet and queried with Athena.

**Shared conventions**
- Every request gets a `trace_id` (W3C traceparent) and a `request_id`. Both are propagated through queues (SQS message attributes) and returned in the `X-Request-Id` response header. Support can paste the request ID into an admin search.
- **PII masking:**
  - Phone numbers are stored as an **HMAC hash** plus the last 2 digits.
  - Emails are hashed.
  - Never logged: OTP codes, tokens, gateway secrets, full webhook signatures, full card or VPA details. VPAs are masked to `ab***@okbank`.
  - Photos and message text are **never** logged. Only their IDs are.
- All tables below are **partitioned monthly** (pg_partman) and indexed by time + key.

```sql
-- 1) API request log (sampled: 100% of errors & payment/auth routes, 10% of other 2xx)
CREATE TABLE api_request_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  request_id    text NOT NULL,
  trace_id      text,
  method        text NOT NULL,
  route         text NOT NULL,          -- templated, e.g. /v1/orders/:id
  status        int  NOT NULL,
  latency_ms    int  NOT NULL,
  user_id       uuid,
  device_id     text,
  ip_hash       bytea,                  -- HMAC(ip) — raw IP kept only in edge logs (7 days)
  country       char(2),
  state_code    char(2),
  locale        text,
  user_agent_family text,
  error_code    text,                   -- domain error code if any
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
CREATE INDEX ON api_request_logs (route, ts DESC);
CREATE INDEX ON api_request_logs (request_id);

-- 2) Webhook log (100%; raw payload kept for disputes/debugging)
CREATE TABLE webhook_logs (
  id            bigserial,
  received_at   timestamptz NOT NULL DEFAULT now(),
  gateway       text NOT NULL,
  event_id      text NOT NULL,
  event_type    text NOT NULL,
  signature_valid boolean NOT NULL,
  payload       jsonb NOT NULL,         -- gateway payload (contains no card data)
  status        text NOT NULL CHECK (status IN ('received','processed','ignored_duplicate','ignored_out_of_order','rejected_signature','failed')),
  processing_ms int,
  error         text,
  processed_at  timestamptz,
  PRIMARY KEY (id, received_at),
  UNIQUE (gateway, event_id, received_at)  -- de-dupe also enforced via a separate unpartitioned key table
);

-- 3) Payment event log (append-only audit of every payment/order state change)
CREATE TABLE payment_event_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  order_id      uuid NOT NULL,
  payment_id    uuid,
  gateway       text,
  source        text NOT NULL CHECK (source IN ('api','webhook','sweeper','recon','admin')),
  from_status   text,
  to_status     text NOT NULL,
  amount_paise  bigint,
  method        text,
  gateway_error_code   text,
  gateway_error_reason text,            -- e.g. BAD_REQUEST_ERROR / payment_failed: bank declined
  request_id    text,
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
CREATE INDEX ON payment_event_logs (order_id, ts);
CREATE INDEX ON payment_event_logs (gateway_error_code, ts DESC) WHERE gateway_error_code IS NOT NULL;

-- 4) Render job log (one row per attempt)
CREATE TABLE render_job_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  render_job_id uuid NOT NULL,
  card_id       uuid NOT NULL,
  template_id   uuid NOT NULL,
  variant_locale text NOT NULL,
  kind          text NOT NULL,          -- image | video
  priority      text NOT NULL,
  attempt       int  NOT NULL,
  outcome       text NOT NULL CHECK (outcome IN ('succeeded','failed','timeout','cancelled')),
  queue_wait_ms int,
  render_ms     int,
  upload_ms     int,
  output_bytes  bigint,
  worker_id     text,
  instance_type text,
  spot          boolean,
  error_class   text,                   -- FONT_MISSING | ASSET_404 | FFMPEG_EXIT | OOM | TIMEOUT ...
  error_detail  text,
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
CREATE INDEX ON render_job_logs (template_id, ts DESC);
CREATE INDEX ON render_job_logs (outcome, ts DESC) WHERE outcome <> 'succeeded';

-- 5) Refund log (every refund action and gateway response)
CREATE TABLE refund_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  refund_id     uuid NOT NULL,
  order_id      uuid NOT NULL,
  action        text NOT NULL CHECK (action IN ('requested','approved','sent_to_gateway','gateway_ack','processed','failed','retry','manual_review')),
  actor         text NOT NULL,          -- system | support:<id> | finance:<id>
  amount_paise  bigint,
  speed         text,
  gateway_refund_id text,
  gateway_status text,
  error         text,
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
CREATE INDEX ON refund_logs (order_id, ts);

-- 6) Audit log (every admin/support/finance action; immutable)
CREATE TABLE audit_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  actor_id      uuid NOT NULL,
  actor_role    text NOT NULL,
  action        text NOT NULL,          -- template.publish, price.update, refund.approve, user.block, badge.override ...
  entity_type   text NOT NULL,
  entity_id     text NOT NULL,
  before        jsonb,
  after         jsonb,
  reason        text,
  ip_hash       bytea,
  request_id    text,
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
-- Immutability: app role has INSERT only; no UPDATE/DELETE grants. Monthly export to S3 with Object Lock (WORM).

-- 7) Error log (unhandled/business errors, linked to Sentry)
CREATE TABLE error_logs (
  id            bigserial,
  ts            timestamptz NOT NULL DEFAULT now(),
  service       text NOT NULL,          -- web | api | payments | render | jobs
  severity      text NOT NULL CHECK (severity IN ('warning','error','critical')),
  error_code    text NOT NULL,
  message       text NOT NULL,
  fingerprint   text NOT NULL,          -- groups identical errors
  sentry_event_id text,
  request_id    text,
  trace_id      text,
  user_id       uuid,
  context       jsonb,                  -- masked
  PRIMARY KEY (id, ts)
) PARTITION BY RANGE (ts);
CREATE INDEX ON error_logs (fingerprint, ts DESC);

-- 8) Alert events (what fired, who was told, de-dup state)
CREATE TABLE alert_events (
  id            bigserial PRIMARY KEY,
  alert_key     text NOT NULL,          -- e.g. payment_success_drop:upi
  severity      text NOT NULL CHECK (severity IN ('P1','P2','P3')),
  status        text NOT NULL CHECK (status IN ('firing','resolved','suppressed')),
  first_fired_at timestamptz NOT NULL,
  last_fired_at  timestamptz NOT NULL,
  occurrences   int NOT NULL DEFAULT 1,
  summary       text NOT NULL,
  details       jsonb,
  notified      jsonb NOT NULL DEFAULT '[]',  -- [{channel:'email', to:'oncall@', at:'…'}]
  resolved_at   timestamptz,
  runbook_url   text
);
CREATE UNIQUE INDEX ON alert_events (alert_key) WHERE status = 'firing';
```

**Retention**

| Data | Hot (Postgres) | Cold (S3 Parquet / Athena) | Notes |
|---|---|---|---|
| `api_request_logs` | 30 days | 1 year | Sampled |
| `webhook_logs`, `payment_event_logs`, `refund_logs` | 180 days | **8 years** | Financial audit trail **[A: confirm with CA]** |
| `render_job_logs` | 90 days | 1 year | |
| `audit_logs` | 1 year | 8 years, S3 Object Lock (WORM) | |
| `error_logs` | 90 days | 1 year | |
| App logs (Loki) | 30 days | — | |
| Edge logs (Cloudflare, raw IP) | 7 days | — | Security investigations |

---

## 2. Observability stack

| Concern | Tool | Notes |
|---|---|---|
| Instrumentation | **OpenTelemetry SDKs** (Node/NestJS, Next.js) | Traces, metrics, logs; vendor-neutral |
| Metrics | Grafana Cloud (Prometheus/Mimir) | RED metrics per route; business metrics (payments by method/status, queue age, render p95, refund backlog) |
| Logs | Grafana Loki | JSON logs with `trace_id`; masked fields |
| Traces | Grafana Tempo | API → SQS → render worker → R2, joined by trace context |
| Errors | **Sentry** (web + API + workers) | Release tracking, source maps, user impact |
| Synthetic checks | Grafana Synthetic Monitoring (probes from Mumbai/Chennai/Delhi) | Home, a recipient page, a full checkout in sandbox every 5 min |
| Real-user monitoring | Web Vitals → PostHog / Grafana Faro | LCP/INP/CLS by device class, network and locale |
| Dashboards | Grafana | **Festival war-room dashboard**; payments; render; SEO pages; cost |
| Status page | Public status page (e.g. Better Stack / Instatus) | Linked from help pages |

**Core dashboards:** (1) Golden signals per service. (2) Payments: attempts, success rate by method/bank/app, failure reasons, webhook lag, pending count. (3) Render: queue age per priority, throughput, p50/p95, failure classes, Spot interruptions. (4) Refunds: requested/processed/failed, backlog age. (5) Business live: shares/min, orders/min, revenue today vs. the same festival last year.

---

## 3. Alerting

### 3.1 Delivery
- **Two independent alert paths**, so payment alerts still go out even if the observability vendor is down:
  1. **Grafana Alerting** (metrics/log-based) → email (SES SMTP) + Slack + PagerDuty/Opsgenie (P1 only, from V1).
  2. **In-app alert service** (the payments service and jobs write `alert_events`) → **Amazon SES email** directly, + Slack. This path covers business-critical conditions: payment failures, refund failures, reconciliation mismatches.
- Recipients: `oncall@` (rotating engineer), `payments-alerts@` (engineering + finance), `founders@` for P1.
- **Email format:** the subject carries the severity, key and a short summary: `[P1] payment_success_drop:upi — 71% (baseline 89%) last 10 min`. The body has the dashboard link, recent example `request_id`s, the runbook link and the time it first fired.

### 3.2 Critical alerts

| # | Alert key | Condition (trigger) | Threshold / window | Severity | Channels | Runbook |
|---|---|---|---|---|---|---|
| A1 | `payment_success_drop:{method}` | Success rate (captured ÷ attempts) by method vs. 7-day same-hour baseline | Drop > 10 pts over 10 min, with ≥ 50 attempts | **P1** | Email + Slack + PagerDuty | RB-PAY-01: check gateway status; switch to secondary (V1) |
| A2 | `payment_failures_spike` | Count of `payment.failed` | > 3x baseline over 5 min, ≥ 30 failures | P2 | Email + Slack | RB-PAY-02 |
| A3 | `gateway_api_errors` | 5xx/timeouts calling the gateway API | > 5% over 5 min, or 3 consecutive timeouts on order create | **P1** | Email + Slack + PagerDuty | RB-PAY-03 |
| A4 | `webhook_lag` | Time from gateway event to processing; or no webhooks in a busy period | p95 > 2 min over 10 min; **zero webhooks for 15 min while attempts > 20** | **P1** | Email + Slack | RB-PAY-04: verify endpoint, signature secret, WAF |
| A5 | `webhook_signature_failures` | Invalid signatures | > 5 in 10 min | P2 (security) | Email + Slack | RB-SEC-02 |
| A6 | `paid_not_fulfilled` | Orders `paid`/`fulfilling` older than 15 min | ≥ 5 orders | **P1** | Email + Slack | RB-RND-02 |
| A7 | `refund_failed` | Any refund in `failed` after automatic retries | ≥ 1 | **P1** | Email (payments-alerts) + Slack | RB-PAY-05 |
| A8 | `refund_backlog` | `refund_pending` older than 24h | ≥ 1 | P2 | Email | RB-PAY-06 |
| A9 | `recon_mismatch` | Daily reconciliation mismatches | Any `captured_not_paid` (after auto-heal) or `paid_not_captured` or amount mismatch | **P1** (paid_not_captured, amount) / P2 (others) | Email (finance + eng) | RB-FIN-01 |
| A10 | `render_queue_age:{priority}` | Age of the oldest SQS message | paid > 60s for 5 min (P1); free > 10 min (P2) | P1/P2 | Email + Slack | RB-RND-01: scale-out check, Spot capacity |
| A11 | `render_failure_rate` | Failed ÷ total renders, overall or per template | > 2% over 15 min (≥ 50 jobs); per template > 10% (≥ 20 jobs) | P2 (P1 if paid) | Email + Slack | RB-RND-03: disable template |
| A12 | `api_error_rate` | 5xx ÷ requests | > 1% over 5 min | P2; **P1 on checkout/auth routes** | Email + Slack | RB-API-01 |
| A13 | `slo_burn_rate` | Error-budget burn (99.9% / 99.95%) | Fast burn 14.4x over 1h; slow burn 6x over 6h | P1 / P2 | Email + Slack (+PagerDuty) | RB-SLO-01 |
| A14 | `latency_p95` | API p95 vs. NFR | > 2x NFR for 10 min | P2 | Slack + email | RB-API-02 |
| A15 | `otp_delivery_failure` | OTP send failures or verify ratio drop | Send failures > 10% over 10 min; verify rate < 50% of baseline | P2 (P1 on festival day) | Email + Slack | RB-AUTH-01: switch channel/provider |
| A16 | `otp_abuse` | OTP requests per number range/IP/ASN | > 5x baseline over 5 min | P2 (security) | Email + Slack | RB-SEC-03 |
| A17 | `synthetic_checkout_failed` | Synthetic sandbox checkout | 2 consecutive failures | **P1** | Email + Slack | RB-PAY-07 |
| A18 | `db_health` | CPU > 80%, connections > 80%, replica lag > 30s, storage < 15% | 10 min | P2 (P1 if replica lag affects reads) | Email + Slack | RB-DB-01 |
| A19 | `cost_anomaly` | Daily infra cost vs. 7-day average | > 50% higher (non-festival days) | P3 | Email | RB-COST-01 |

### 3.3 De-duplication and throttling (no alert storms)
- **One open alert per `alert_key`** (unique partial index). New occurrences increment `occurrences` and `last_fired_at`; they don't send new emails.
- **Re-notify** only on (a) a severity escalation, or (b) a reminder every 30 min (P1) / 2h (P2) while still firing.
- **Grouping:** alerts that share a root cause (e.g. A1, A2 and A3 during a gateway outage) are grouped by `group_key = gateway` into **one email thread**.
- **Inhibition:** if `gateway_api_errors` is firing, `payment_success_drop` emails are sent as "related" within the same thread, not as separate pages.
- **Resolve:** an automatic "RESOLVED" email when the condition has been clear for 10 min.
- **Maintenance windows:** silence rules are created in the admin panel, with an expiry and an audit log entry.
- **Per-channel rate limits:** at most 20 alert emails per hour per recipient. Anything beyond that is rolled into a digest.

---

## 4. Product analytics

### 4.1 Tool choice
- **PostHog Cloud** for the MVP: event analytics, funnels, retention, cohorts, feature flags, A/B experiments and session replay (**replay off by default**; only with consent and masked inputs). The free tier covers early volume **[V: vendor pricing, verify]**.
- **Server-side events** (orders, payments, renders, refunds) are sent from the backend so revenue analytics don't depend on client tracking.
- **Warehouse:**
  - MVP: nightly export of PostHog events + Postgres facts to S3 Parquet → Athena.
  - V1: BigQuery in `asia-south1` (Mumbai) or ClickHouse once volume and analyst needs grow.
- **Alternatives:**
  - Mixpanel: excellent, but costly at scale, and it has no feature flags.
  - GA4 + BigQuery: free, but weak for product funnels and sampling-prone. We use GA4 **only** for SEO/acquisition reporting, if at all.
- **Consent (DPDP):** before analytics consent, we send only **essential, non-identifying** events (cookieless, no person profiles). After consent, events link to a pseudonymous `distinct_id`. Phone and email are **never** sent to analytics; only `user_id` (UUID) is.

### 4.2 Event taxonomy (MVP)

Naming: `object_action`, snake_case. Every event carries these common properties: `locale`, `ui_locale`, `state_code`, `device_class` (low/mid/high), `network_type`, `platform` (web/pwa/twa), `app_version`, `session_id`, `utm_*`, `entry_surface` (seo/recipient/ad/direct/push), `ref_card_id` (when it came from a recipient page).

| Event | Trigger | Key properties |
|---|---|---|
| `page_viewed` | Any page view | `page_type` (home/collection/template/editor/checkout/recipient/event/seo_text), `path` |
| `language_selected` | User picks a language | `from`, `to`, `method` (picker/switcher) |
| `festival_hero_clicked` | Home hero CTA | `festival_key`, `days_to_festival` |
| `collection_viewed` | Collection grid | `collection_slug`, `filters`, `result_count` |
| `template_impression` | A template card ≥ 50% visible for 1s (batched) | `template_id`, `position`, `badges[]`, `price`, `ref_price_shown` |
| `template_viewed` | Template detail opened | `template_id`, `kind`, `tier`, `price`, `badges[]`, `variant_locale` |
| `search_performed` | Search submitted | `query_raw` (truncated), `query_script` (latin/deva/taml), `result_count` |
| `search_no_results` | Zero results | `query_raw`, `locale` |
| `editor_started` | Draft created | `template_id`, `variant_locale`, `format`, `from_recipient` |
| `slot_filled` | A slot is completed (debounced) | `slot_type` (photo/name/message/date/venue), `used_transliteration`, `used_ai` |
| `photo_uploaded` | Upload done | `bytes`, `upload_ms`, `bg_removal_used`, `retries` |
| `ai_wish_requested` / `ai_wish_used` | AI writer | `tone`, `relation`, `latency_ms`, `suggestion_index` |
| `preview_generated` | Preview ready | `kind`, `latency_ms`, `watermarked` |
| `paywall_viewed` | Premium options shown | `options[]`, `default_option`, `ref_price_shown` |
| `paywall_option_selected` | User picks an option | `option` (free/hd/pack/tier), `price` |
| `otp_requested` / `otp_verified` | Auth | `channel`, `attempt`, `latency_ms` |
| `checkout_started` | Checkout page | `order_id`, `total`, `coupon_auto_applied`, `items_count` |
| `payment_method_selected` | Method chosen | `method`, `upi_app` |
| `payment_succeeded` (server) | Capture verified | `order_id`, `amount`, `method`, `gateway`, `time_to_pay_ms` |
| `payment_failed` (server) | Failure | `order_id`, `method`, `error_code`, `error_reason` |
| `render_completed` (server) | Render done | `card_id`, `kind`, `priority`, `queue_wait_ms`, `render_ms` |
| `render_failed` (server) | Render failed | `card_id`, `error_class`, `attempt` |
| `card_shared` | Share action | `card_id`, `channel`, `is_paid` |
| `card_downloaded` | Download | `card_id`, `format` |
| `recipient_page_viewed` | Recipient opens `/c/` | `card_id`, `template_id`, `lcp_ms` |
| `recipient_make_own_clicked` | Viral CTA | `card_id`, `template_id` |
| `wishes_sent_back` | Reply from recipient | `card_id` |
| `rsvp_submitted` | RSVP | `event_id`, `response`, `guests` |
| `refund_initiated` / `refund_completed` (server) | Refund lifecycle | `order_id`, `reason_code`, `amount`, `speed` |
| `reminder_opt_in` / `notification_clicked` | Retention | `channel`, `campaign` |
| `rating_submitted` | Template rating | `template_id`, `stars` |

### 4.3 Key funnels and reports
1. **Core greeting funnel:** `page_viewed(home/collection)` → `template_viewed` → `editor_started` → `preview_generated` → `card_shared` (free) **or** `paywall_viewed` → `checkout_started` → `payment_succeeded` → `render_completed` → `card_shared`. Broken down by locale, device class, entry surface and festival.
2. **Invitation funnel:** `template_viewed(invitation)` → `editor_started` → `preview_generated` (watermarked) → `card_shared` (preview to family) → `paywall_option_selected(tier)` → `payment_succeeded` → `rsvp_submitted` (guests).
3. **Viral loop:** `card_shared` → `recipient_page_viewed` → `recipient_make_own_clicked` → `editor_started (from_recipient)` → `card_shared`. **K = new creators from recipient pages ÷ active creators**, by template and language.
4. **Payment health:** attempts → success by method, UPI app and bank (from gateway data); time to pay; pending → resolved.
5. **Retention:** festival-cohort retention (users first active in festival window N, returning in N+1, N+2), payer repeat rate, D7/D30 for invitation hosts (RSVP checking).
6. **Template performance:** views → editor starts → completions → orders, revenue, **template ROI** (revenue ÷ production cost), rating. This feeds the badges and the content roadmap.
7. **Search quality:** no-result rate per language, top no-result queries (fed into the alias table every week).

### 4.4 Experimentation framework
- **PostHog feature flags + experiments.** Assignment by `distinct_id` (sticky).
- **Process:** a hypothesis and a primary metric are written down before launch. A minimum detectable effect and sample size are calculated in advance. Guardrails: payment success, refund rate, CSAT. Run for at least 1 full week, and **avoid starting tests within 3 days of a major festival** (traffic mix shifts).
- **Seed backlog:**
  1. Strike-through vs. plain price (eligible items only).
  2. "Made with [brand]" text on free cards: on/off.
  3. ₹19 vs. ₹29 for premium images (in the pilot).
  4. Invitation 3-tier vs. 2-tier ladder.
  5. Recipient CTA copy variants.
  6. Watermark style.
  7. Trust strip on/off.

### 4.5 Analytics → product loops
- Nightly aggregates (`template_metrics_daily`) feed **badges** (Bestseller, Trending in state), **ranking** (collection sort by a blend of conversion, shares and freshness) and **recommendations** (V1).
- No-result queries feed the **search alias table** (weekly review by the content team).
- Template ROI feeds the **content production plan** for the next festival.

---

## 5. Key decisions (observability and analytics)
- **Business log tables in Postgres** (payments, webhooks, renders, refunds, audit, errors, alerts) with masking, monthly partitions and S3 archival. App logs live in Loki.
- **Two independent alert paths.** Payment, refund and reconciliation alerts are emailed directly via SES, so they don't depend on the observability vendor. Alerts are de-duplicated, grouped and throttled.
- **OpenTelemetry + Grafana Cloud + Sentry** for observability; **PostHog** for product analytics, flags and experiments; consent-aware, with no PII.
- **Analytics feed the product directly:** badges, ranking, search aliases and the content plan.

## 6. Open questions for the founder
1. Who receives **P1 alerts at night** during festival weeks? We need at least 2 people on the on-call rota.
2. Is **session replay** acceptable, with consent and masking? It's useful for UX debugging. Recommended: off for the MVP; enable for 5% of consented sessions in V1.

# E-Cards for India: Market Study, PRD and Technical Design

**As of:** 2 October 2026 · **Status:** Draft v1 for founder review
**Produced from:** `prompts/ecards-prd-and-system-design.md` (all 8 phases, run end-to-end without the phase stops)

## Documents

| # | Document | What's inside |
|---|---|---|
| 01 | [Market and competitor research](01-market-research.md) | India 2026 context, competitor profiles (Crafto, business poster apps, invitation platforms, legacy e-card sites, Canva, global benchmarks), comparison table, willingness to pay, TAM/SAM/SOM, 7 personas, white space, sources |
| 02 | [Product Requirements Document](02-prd.md) | Problem, vision, goals, north-star and input metrics, **riskiest assumptions + validation plan**, pricing and unit economics, MoSCoW feature list by phase, 14 user stories with acceptance criteria, localisation, edge cases, legal risks, platform decision |
| 03 | [UX and conversion design](03-ux-and-conversion.md) | Sitemap, wireframes for 10 screens, a 17-tactic conversion playbook with data rules, CCPA dark-pattern compliance mapping, performance UX, accessibility, design system |
| 04 | [System design](04-system-design.md) | NFRs, architecture diagram, tech stack with reasons, modules, template scene schema + render pipeline, ER diagram + DDL, festival-spike scaling, load testing, "Diwali readiness" runbook, REST API contracts |
| 05 | [Payments, refunds and reconciliation](05-payments-and-refunds.md) | Gateway comparison, checkout/webhook/refund sequences, order/payment/refund state machines, idempotency, automatic refunds, reconciliation, RBI/PCI/GST compliance |
| 06 | [Logging, monitoring, alerting and analytics](06-observability-and-analytics.md) | Log table DDL, observability stack, 19 critical alerts with email routing and de-duplication, PostHog event taxonomy, funnels, experimentation |
| 07 | [Security and compliance](07-security-and-compliance.md) | STRIDE threat model, OWASP Top 10 + API Top 10 controls, upload pipeline, abuse prevention, content moderation (IT Rules 2026), DPDP Act compliance |
| 08 | [Roadmap, team, cost, risks and GTM](08-roadmap-team-cost-gtm.md) | Festival calendar, Gantt roadmap, team and runway, infra cost at 10K/100K/1M MAU, top 10 risks, go-to-market plan with ₹ budgets |
| 09 | [**Lean launch plan** (solo + Claude, go live ~16 Oct 2026)](09-lean-launch-plan.md) | **Start here for the first launch.** Reduced scope, user flow, ₹6–7k/month stack, launch pricing, 2-week schedule, go-live checklist, brand name |

## Executive summary

**1. The market is proven, and Bharat pays.**
- Crafto (personalised greetings, by Kutumb/Primetrace) made **₹128.6 crore revenue in FY25 (+173%)**, mainly from subscriptions.
- Its parent reported a **₹550 crore run-rate** in February 2026.
- Wedding invitation videos already sell for **₹149 to ₹4,999**.
- Business festival-poster apps charge **₹99/year to ₹199/month**.

**2. Our wedge: free greetings for reach, paid invitations and premium cards for revenue.**
- Festival greetings stay free and viral.
- Revenue comes from **pay-once premium cards (₹19–₹49)** and **video invitations (₹149 / ₹399 / ₹799)** with RSVP.
- A Pass and a Business Plan arrive in V1.
- **No ads, ever, on the page the recipient sees.**

**3. How we stand out:**
- **Language-native quality** in Hindi, Marathi, Tamil and English.
- **Your photo and name in 3 taps.**
- A **recipient page with "Make your own"** as the growth loop.
- **Honest commerce:** real discounts, real bestsellers, no autopay traps, and **automatic refunds when something fails**. This is the trust gap competitors leave open, and it's what India's CCPA dark-pattern rules require anyway.

**4. Validate before building.**
- A **Diwali 2026 concierge pilot** (starting 10 Oct) tests ₹19/₹29/₹49 pricing and virality.
- A **wedding-season pilot** (Nov–Dec 2026) tests invitations.
- **Go/no-go: 15 Dec 2026.**

**5. Launch plan.**
- Build the MVP from Oct 2026 to Jan 2027.
- **Closed beta at Pongal** (Jan 2027).
- Public soft launch on 1 Feb at regular prices.
- **Public launch across Holi (22 Mar) → Gudi Padwa (7 Apr) → Puthandu (14 Apr) 2027**, one hero festival per launch language.
- Scale for **Diwali 2027 (29 Oct)**.

**6. Tech.**
- Next.js PWA + NestJS modular monolith on **AWS Mumbai**, with **Cloudflare** at the edge (and R2 for zero-egress media).
- PostgreSQL, SQS, Redis.
- **Chromium-shaped text + FFmpeg** render workers on Spot instances.
- **Razorpay** for payments (Cashfree as V1 failover), with webhook-verified state machines, automatic refunds and daily reconciliation.
- Two independent alert paths with **email alerts** for payment, refund and reconciliation failures.
- PostHog analytics; DPDP-compliant from day 1.

**7. Money.**
- **≈ ₹2.5–3.5 crore runway** to Diwali 2027 for a team of ~7 FTE plus freelancers **[E]**.
- Infra costs about **₹40–50k/month at 10K MAU** and **₹12–15 lakh/month at 1M MAU** **[E]**.
- **≈ 93% contribution margin** on net revenue per order **[E]**.

## Decisions waiting on the founder
Each document ends with its own open questions. The ones that block work:
1. Approve the **Diwali 2026 pilot** (~₹1–1.5 lakh) and the **invitations-led revenue model**.
2. **Entity, GST and gateway KYC** status. Payments can't go live without them.
3. A **brand name** shortlist (domains + trademark search).
4. **Budget** confirmation (₹2.5–3.5 crore to Diwali 2027), or a scope cut.
5. **Critical hires:** lead engineer and motion-design lead.

## Labels used throughout
**[V]** sourced from a cited public report · **[E]** our estimate (logic shown) · **[A]** assumption to validate. Research used web search. Some figures come from search summaries of the cited pages, so re-check them on the live page before relying on them for money decisions.

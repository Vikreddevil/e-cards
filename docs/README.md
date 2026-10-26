# CardReel: Market Study, PRD and Technical Design

*Wishes, cards and video invitations for every occasion, in Hindi, English, Marathi and Tamil.*

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
| 09 | [**Lean launch plan** (solo + Claude; free launch first)](09-lean-launch-plan.md) | **Start here for the first launch.** Source of truth for launch scope and pricing: Phase 0 free launch, Phase 1 payments switch-on rule, user flow, ₹6–7k/month stack, pricing, relative schedule, go-live checklist |
| 10 | [Independent PRD review](10-prd-review.md) | Neutral review against Evite, Paperless Post, Punchbowl, Kudoboard, InvitiApp, NIMNTRN, Crafty Art and Indian invite makers: scorecard, 24 gaps with priorities, inconsistencies, launch re-baseline |
| 11 | [Analytics tracking plan](11-analytics-tracking-plan.md) | Vendor-neutral event spec for user engagement: common envelope, 39 generic events, engagement metric definitions, launch KPIs, implementation guide, QA and governance |
| 12 | [Business setup: entity, Razorpay, GST](12-business-setup-razorpay-gst.md) | Start as an individual, then a proprietorship; Razorpay from signup to live payments (who does what, integration, fees, timelines); GST not needed under ₹20 lakh; CA checklist |
| 13 | [Brand identity: CardReel](13-brand-identity.md) | Name, `cardreel.in`, social handles, deferred trademark plan, logo (`brand/` SVGs), colours with contrast, fonts for four scripts, SEO principle, voice. Visual brand board: https://claude.ai/artifact/4D4BYhFGvwSdy1DGkvaLWB |
| 14 | [Senior PM review: launch readiness](14-senior-pm-review.md) | Scorecard, 11 ranked findings (one Launch PRD, real festival and wedding calendar, sell one thing first, design partners, Phase 0 go-to-market, template quality bar, metrics), founder decisions, re-baselined calendar |
| 15 | [**Release plan**: staging → free launch → payments](15-release-plan.md) | Founder's timeline: staging by Day 3, testing week, free production week (Day 11), payments (Day 18). Environments, gates, founder checklist, test plan, bug severity, operations, risks |
| 16 | [**Stack, accounts and email** (decided)](16-stack-accounts-email.md) | Lean AWS stack (S3 + CloudFront, Lambda, DynamoDB, SQS render, SES) at ≈ ₹100–600/month; DynamoDB vs PostgreSQL; accounts kept and dropped; AWS setup; **Zoho Mail Free** with a 0–10 user cost comparison against Google Workspace |
| 17 | [**High-Level Design (HLD)**](17-hld.md) | How CardReel is built on the lean AWS stack: components, DynamoDB tables, create/render/share flows, layer-based video rendering, **payment security and 16 concurrent-payment scenarios**, refunds and reconciliation, scalability limits and festival playbook, security and Indian compliance (CERT-In 180-day logs), speed and output-quality targets, failure modes, CI/CD, tests, cost by component |

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

> **Update:** points 4 and 5 describe the original full-team plan. The founder chose a lean, solo launch under the brand **CardReel** (`cardreel.in`). The release plan (`15`) is: **staging by Day 3** after the PRD go-ahead, a **testing week**, **production live free on Day 11**, and **payments on Day 18** once Razorpay KYC is approved. See `09-lean-launch-plan.md`, `12-business-setup-razorpay-gst.md` and `13-brand-identity.md`.

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

> **Launch stack (decided):** lean AWS in Mumbai (S3 + CloudFront, API Gateway + Lambda, DynamoDB, SQS render Lambda, SES) at about **₹100–600/month**; email on **Zoho Mail Free**. See `16`. **How it's built** (architecture, payments under concurrency, scalability, security, quality): `17-hld.md`. Points 6 and 7 below describe the full-scale design and team plan for later.

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
Each document ends with its own open questions. For the lean launch, these are the ones that block work:
1. **Go-ahead on the PRD** to start the build (Day 1 of the schedule in `09` §6).
2. **Buy `cardreel.in`**, reserve the social handles, and run the free trademark search (`13` §1–3).
3. ~~Stack and email~~ **Decided:** lean AWS stack with DynamoDB; Zoho Mail Free (`16`).
4. ~~Colour direction~~ **Decided: Purple & Marigold** (violet `#6D28D9`, marigold `#FFB627`, mint `#5EEAD4`); see `13` §5.
5. **Native reviewers** for Marathi and Tamil, and **licensed music** for video templates (`09` §7).
6. For Phase 1 payments: **Razorpay KYC** once the site is live, and a **30-minute CA consultation** (`12`).
7. Small HLD choices, none blocking (`17` §16): email required at checkout, separate staging and production AWS accounts, and the refund safety brake (all recommended: yes).

## Labels used throughout
**[V]** sourced from a cited public report · **[E]** our estimate (logic shown) · **[A]** assumption to validate. Research used web search. Some figures come from search summaries of the cited pages, so re-check them on the live page before relying on them for money decisions.

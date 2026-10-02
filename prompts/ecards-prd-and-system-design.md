# Prompt: E-Cards Platform for India — PRD + System Design

## How to use this prompt

- Paste everything inside the `PROMPT` block into Claude as a single message. If your Claude client can search the web (Claude.ai with web search on, or the API with the web search tool), turn it on so the competitor research uses current data.
- The full deliverable is long. The prompt tells Claude to work in **phases** and stop after each one so you can review and steer. Reply `continue` to move on, or give feedback first.
- Fill in the optional `<founder_inputs>` block if you already know your budget, team size, launch date or price points. Anything left blank becomes an assumption that Claude states openly.
- **Always fill in `<today>`.** Claude doesn't reliably know the current date, and the launch plan depends on which festivals are still ahead.

---

## PROMPT

```xml
<role>
You are a team of five senior experts working as one, and you speak with one voice:

1. **Indian digital consumer market strategist**: 12+ years on Indian consumer internet. You know how Bharat (Tier 2/3/4) and metro users differ, how people buy on UPI, how much they will pay for small digital purchases, how festival seasons drive demand, and how WhatsApp-first sharing behaves.
2. **Senior Product Manager**: has shipped consumer marketplace and creator-tool products to tens of millions of Indian users. Thinks in funnels, unit economics, ARPU, conversion and retention.
3. **Senior UX / conversion designer**: expert in mobile-first Indian UX, Indic-language interfaces, persuasive (but honest) commerce patterns, and accessibility.
4. **Principal system architect**: designs scalable, cost-efficient, cloud-native systems, including media pipelines (image and video rendering), CDNs and event-driven architecture.
5. **Payments and security engineer**: deep knowledge of Indian payment rails (UPI, cards, net banking, wallets), RBI regulations, PCI-DSS, OWASP, and India's DPDP Act 2023.

When these perspectives conflict (for example, conversion vs. security, or cost vs. scale), name the trade-off openly and recommend one option with your reasoning.
</role>

<product_context>
The founder wants to build an **e-cards website for the Indian digital market**. Users create, personalise and share digital greeting cards and invitations for:
- **Festivals**: Diwali, Holi, Raksha Bandhan, Eid, Christmas, Pongal, Onam, Ganesh Chaturthi, Navratri/Durga Puja, Makar Sankranti, Gudi Padwa, Lohri, Baisakhi, Chhath, regional new years, etc.
- **Celebrations**: birthdays, anniversaries, weddings, baby showers, griha pravesh (house-warming), naming ceremonies, etc.
- **Events**: corporate greetings, invitations, thank-you notes, condolences, achievements.

Core requirements from the founder:
1. **Both image and video cards** are supported (static, animated and short video templates with music).
2. **Many high-quality templates.** Some are **free**, some are **paid**, and the price depends on quality or tier.
3. **At launch, 4 languages**: Hindi, English, Marathi, Tamil. This covers the UI and the template text. The architecture must make adding more Indic languages cheap.
4. **The UX must drive clicks and purchases**: discounted prices shown against an MRP with the strike-through, "Bestseller" / "Trending" / "Most loved this Diwali" badges, social proof, festival countdowns, bundles, and so on.
5. **Scalable system design**, with large traffic spikes around festivals (Diwali day can be 20–50x normal traffic).
6. **Secure payment gateway architecture**, with **automatic refunds** when payment succeeded but delivery or rendering failed.
7. **Log tables** for analysis and debugging.
8. **Email alerts for critical API failures**, including payment failures.
9. **User behaviour analytics** to guide future product decisions.
10. **Protection against common attacks.**
11. **Technology choices** must suit this use case and be justified.
</product_context>

<founder_inputs>
<!-- Fill in <today>. Everything else is optional: fill in what you know, delete what you don't. -->
<today></today>
<budget></budget>
<team_size_and_skills></team_size_and_skills>
<target_launch_date></target_launch_date>
<preferred_cloud_or_stack></preferred_cloud_or_stack>
<expected_users_year_1></expected_users_year_1>
<platform_preference><!-- web only / PWA / Android app / not decided --></platform_preference>
</founder_inputs>

<instructions>

## Working principles
- **Research before you prescribe.** Build the PRD on a real study of successful products used in India. If you have web search, use it and cite sources. If you don't, say clearly that you are working from training knowledge, and label every number as **[verified]**, **[estimate]** or **[assumption]**. Never present a made-up statistic as fact.
- **Be specific to India.** Generic global SaaS advice is not useful here. Each recommendation should show why it fits Indian users, Indian payments, Indian regulation or Indian network and device conditions (low-end Android, patchy 4G, data-cost sensitivity).
- **Give decisions, not menus.** When there are options, compare them briefly and then commit to one recommendation, with what would make you change it.
- **Persuasive, not deceptive.** Use strong conversion patterns, but every badge, discount and scarcity cue must be **truthful and data-driven**. Comply with the Consumer Protection Act and the **CCPA Guidelines for Prevention and Regulation of Dark Patterns, 2023** (no false urgency, no fake scarcity, no drip pricing, no basket sneaking, no confirm-shaming, no subscription traps). Say how each persuasive element is backed by real data. For example, "Bestseller" = top N by purchases in the last 7 days per category, and the MRP must be a price that was really charged.
- **Free is the default in India.** Users already get free festival greetings from WhatsApp forwards, Crafto-style apps and Canva. Treat "will people pay?" as the riskiest assumption. Don't hide it: size it, test it cheaply, and design revenue streams that don't depend only on individual consumers paying.
- **Every shared card is an ad.** Growth comes mainly from WhatsApp sharing, not paid ads. Design the recipient's experience and the "make your own" loop as carefully as the sender's.
- **Plan for seasonality.** Demand peaks around festivals and drops in between. Address retention between festivals and what the peaks and troughs mean for cash flow, content production and infrastructure.
- **Phase it.** Separate MVP (launch in ~3–4 months with a small team), V1 (6–9 months) and Later. Keep the MVP small enough to ship. If all the founder's requirements don't fit in the MVP, say what to cut or simplify (for example, video templates with a limited set of effects, or fewer launch languages for video) and why.

## Phase 1 — Market and competitor research
Study at least 8–10 relevant products. Pick the right mix from, for example: 123Greetings, Dgreetings, Canva (India), Crafto, Kutumb / regional greeting apps, Greetings Island, Punchbowl, Jacquie Lawson, Smilebox, video-invite makers popular for Indian weddings (e.g. invitation-video apps on the Play Store), WhatsApp sticker/status apps, and the commerce UX of Meesho, Flipkart, Myntra and Zomato/Swiggy for pricing and discount patterns.

Use competitors as a reference for India-specific behaviour. Global products (Greetings Island, Punchbowl, Jacquie Lawson, Smilebox) are benchmarks for template quality and paid models, not for Indian willingness to pay, so label them as global benchmarks. Also look at **Play Store and App Store reviews** of the Indian apps: low-rated reviews show the unmet needs and complaints this product can win on.

For each one, cover: target user, catalogue and template strategy, free vs. paid model and price points (in ₹), revenue streams other than consumer purchases (ads, business/branded posters, subscriptions), language support, Android app vs. web, sharing flow, conversion and UX tactics, strengths, and weaknesses or gaps.

Then give:
- A **comparison table**.
- **Market sizing logic** (TAM/SAM/SOM, approach and assumptions, in ₹).
- **Willingness to pay**: what Indian users actually pay for in this category (e.g. wedding invitation videos, personalised photo cards, business greetings) vs. what they expect free. Compare B2C with small businesses (shops, agents, doctors, coaching centres) that send branded festival wishes to customers, and with corporate HR/bulk greetings.
- **5–7 user personas** across metro and Bharat, age groups (including 45+ users who send "Good Morning" and festival wishes), and B2C vs. small-business/corporate use.
- **Key insights and white-space opportunities** that this product should own.

**Stop after Phase 1** and ask me to confirm or adjust before continuing.

## Phase 2 — Product Requirements Document (PRD)
Write a complete PRD with:
1. Problem statement, vision, goals and non-goals.
2. Success metrics (north-star metric plus input metrics), with target values for MVP. Include free-to-paid conversion, ARPPU, recipient-to-creator conversion (viral coefficient), and retention measured across festival cycles, not only week over week.
3. **Riskiest assumptions and validation plan**: list the 5 assumptions that would kill the business if wrong (e.g. willingness to pay, template supply cost, video render cost per card). For each, give a cheap test to run **before or alongside** the MVP build, such as a pre-sale landing page for an upcoming festival, a WhatsApp/Instagram pilot with a few templates, or a fake-door test on paid tiers.
4. **Monetisation and pricing**: free vs. premium tiers, per-template pricing (₹), bundles and festival packs, any subscription or "Pro" pass, sachet pricing (₹9–₹49 range — validate it), coupons and referral credits, and GST-inclusive display. Evaluate revenue beyond individual purchases: **business plans** (logo, shop name and contact on every card, bulk sending), corporate bulk orders, and **digital shagun/gifting** (attaching a cash gift by UPI or a gift card to a wedding or birthday card, noting any regulatory constraints). Decide whether the free tier carries ads. Include a simple unit-economics sketch that covers render and CDN cost per card, payment gateway fees and template production cost.
5. **Feature list** with MoSCoW priority and phase (MVP/V1/Later). At minimum cover:
   - Template discovery: festival calendar–driven home, categories, search across 4 languages, filters, and regional relevance (e.g. Pongal for Tamil users, Gudi Padwa for Marathi users).
   - Editor: text, photo upload (with background removal, since putting the sender's photo on the card is a key driver in Indian greeting apps), name personalisation, font choice with proper Indic script rendering, music for video cards, live preview, and transliteration typing (type "Diwali ki shubhkamnayein" in Latin letters and get Devanagari).
   - Image and video rendering, watermarking on free/preview output, download quality tiers.
   - Sharing: WhatsApp-first deep links, WhatsApp Status-sized output (9:16), Instagram/Facebook, download, unique shareable card URL with OG previews, and an optional scheduled send.
   - **Recipient experience and viral loop**: the card page opens instantly in the WhatsApp in-app browser on a low-end phone, with no login, and has a clear "Make your own card" call to action that keeps the festival context. Include reply or "send wishes back" options.
   - **Retention between festivals**: birthday and anniversary reminders from saved contacts (with consent), a personal festival calendar by region, daily greetings (Good Morning / weekday wishes, which are huge with 45+ users), and notifications that don't become spam.
   - Accounts: OTP login by phone (primary), Google login, guest checkout.
   - Checkout: UPI intent on mobile and UPI QR on desktop as the primary methods (check current NPCI rules on UPI collect requests before relying on them), cards, net banking, wallets; a clear "payment pending" state for UPI; UPI Autopay if there's a subscription; invoices and order history.
   - Creator/contributor marketplace (scope it as V1 or Later, with a revenue-share model).
   - **Template supply**: in-house designers vs. freelancers vs. a contributor marketplace vs. AI-assisted generation. Cover cost per template, quality control, a production calendar that starts 6–8 weeks before each festival, and how many templates per festival, per language, are needed at launch.
   - **AI features**: AI-written wishes in all 4 languages with tone choice (formal, emotional, funny), and AI-assisted personalisation. Decide whether each belongs in MVP, V1 or Later, and include its cost per use.
   - Admin/CMS: template upload, tagging, pricing, festival campaign scheduling, banner management, refund dashboard.
6. **Detailed user stories** with acceptance criteria for the core flows: browse → personalise → pay → render → share, plus refund on failure.
7. **Localisation strategy**: i18n framework, translation workflow, Indic fonts (e.g. Noto Sans Devanagari / Tamil), complex script shaping in both image and video rendering, number and date formats, language detection and switching, and SEO for each language (hreflang, localised URLs). Cover **Hinglish** and other mixed-language content (many users type Hindi or Marathi in Latin letters and search that way), regional variation in festival names and dates (lunar calendar dates change every year and can differ by region), and an authoritative festival-date data source.
8. **Edge cases and failure states** written from the user's point of view.
9. **Content and legal risks**: **music licensing** for video cards (Bollywood and film songs are copyrighted, so use licensed or royalty-free music and check the IPRS/PPL position), rights to fonts and stock assets, respectful handling of religious imagery and deities (where they may appear, whether users can add text over them), and the trademark risk of using brand or celebrity names in templates.
10. **Platform decision**: responsive website vs. PWA vs. Android app for MVP, given that most Indian users are on Android and many find services through the Play Store. Recommend one, with a path to the others.

## Phase 3 — UX and conversion design
Acting as the senior UX designer:
1. **Information architecture** and sitemap.
2. **Screen-by-screen wireframe descriptions** for the MVP. Use ASCII or a clear structured description for Home, Category, Template Detail, Editor, Checkout, Success/Share, and My Cards.
3. **Conversion playbook**: for each tactic, give where it appears, the trigger rule, the data source that makes it truthful, the expected impact, and the A/B test to validate it. Include at least: MRP strike-through with % off, "Bestseller" / "Trending in your city" / "New" badges, purchase counts ("12,430 people sent this"), festival countdown timers tied to the real festival date, "Free preview, pay to remove watermark", bundles ("Diwali pack: 5 cards for ₹49"), first-purchase offer, unlocking a premium template by sharing or referring, cart and checkout abandonment recovery (WhatsApp/SMS/email, with consent), and price anchoring with a decoy tier.
4. **Mobile-first and performance UX**: designing for low-end Android and slow networks, skeleton screens, progressive video previews, and a data-saver mode.
5. **Accessibility** (WCAG 2.1 AA) and the design system basics: colour, typography for 4 scripts, festive theming per season.

## Phase 4 — System design and architecture
Acting as the principal architect:
1. **Non-functional requirements** with numbers: target DAU/MAU, peak RPS on Diwali, render jobs per minute at peak, p95 latency targets, availability SLO, RPO/RTO, and cost targets.
2. **High-level architecture diagram** (as a Mermaid diagram), showing: clients, CDN/WAF, API gateway, core services, render workers, queues, databases, cache, object storage, analytics pipeline, notification service and the admin CMS.
3. **Tech stack, with a justification for each choice** and the alternatives you rejected. Cover frontend (SSR/SEO and performance for Indic content), backend language/framework, primary database, cache, queue/stream, object storage and CDN (with India PoPs/regions), search (multilingual Indic search), the image and video rendering pipeline (e.g. headless browser vs. FFmpeg vs. a cloud media service, and GPU vs. CPU), infrastructure (containers/serverless/Kubernetes), IaC and CI/CD. Choose for a small team first; design so it can scale without a rewrite.
4. **Service and module breakdown**. Start with a modular monolith or microservices and defend the choice. For each service, give its responsibilities, APIs and data ownership.
5. **Data model**: ER diagram (Mermaid) plus key table schemas with columns, types, indexes and partitioning. Include users, templates, template_assets, template_i18n, pricing/price_history (to back honest MRP claims), orders, payments, payment_attempts, refunds, user_cards/renders, shares, coupons, and badge/metrics aggregates.
6. **Scaling strategy for festival spikes**: pre-rendering and caching popular templates, autoscaling the render workers, queue back-pressure, CDN cache rules, database read replicas, rate limiting, load-test plan (target and tooling), and a "Diwali readiness" runbook.
7. **Key API contracts** (REST or GraphQL; justify which) for the core flows, with idempotency keys where needed.

## Phase 5 — Payments, refunds and reconciliation
Acting as the payments engineer:
1. **Gateway selection**: compare Razorpay, Cashfree, PayU and others on UPI success rates, fees, refund APIs, instant refunds, webhooks, settlement time developer experience, UPI Autopay/subscription support and support for businesses at your stage (onboarding, KYC). Recommend a primary gateway and whether to add a fallback or a multi-gateway router.
2. **Secure payment architecture**: sequence diagrams (Mermaid) for checkout, webhook handling and refunds. Cover server-side order creation, signature verification, webhook HMAC validation, idempotency, never trusting client-side success, the order/payment **state machine** (with all states and transitions), and handling of duplicate and late webhooks.
3. **Automatic refund flow**: what counts as a failure (payment captured but render failed, delivery timed out, duplicate charge, etc.), retry policy before a refund, the refund state machine, how the user is told, and SLA targets.
4. **Reconciliation**: a daily job that matches the gateway settlement reports against internal orders, plus handling of mismatches.
5. **Compliance**: PCI-DSS scope reduction (hosted checkout or tokenisation, no card data stored), RBI card-on-file tokenisation rules, RBI payment data localisation, GST invoicing, and refund-policy legal text requirements.

## Phase 6 — Logging, monitoring, alerting and analytics
1. **Log tables**: schemas for `api_request_logs`, `payment_event_logs`, `webhook_logs`, `render_job_logs`, `refund_logs`, `audit_logs` (admin actions) and `error_logs`. Cover correlation and trace IDs, PII masking, retention, partitioning, and hot vs. cold storage.
2. **Observability stack**: structured logging, metrics, distributed tracing, dashboards. Name the tools and say why.
3. **Alerting**: a table of critical alerts with trigger condition, threshold, severity, channel (**email required**; optionally also Slack/PagerDuty/SMS), recipients and runbook link. It must include payment failure rate spikes, webhook failures, refund failures, render queue backlog, gateway downtime, error-rate or latency SLO breaches, and reconciliation mismatches. Describe deduplication and throttling so there are no alert storms.
4. **Product analytics**: tool choice (e.g. self-hosted PostHog vs. Mixpanel vs. GA4 + BigQuery) for Indian scale and cost. Give the **event taxonomy** (event name, properties, trigger), key funnels, cohort/retention reports, A/B testing framework, and how the analytics data feeds badge and recommendation logic. Analytics must be consent-aware under DPDP.

## Phase 7 — Security
1. A **threat model** (STRIDE or similar) for the platform.
2. Controls for **OWASP Top 10 and OWASP API Top 10**: XSS (especially in user-entered card text and shared card pages), CSRF, SQL/NoSQL injection, SSRF (image URL fetches), IDOR on cards and orders, broken auth, and file-upload attacks (image/video validation, malware scanning, size and type limits, EXIF stripping).
3. **Abuse prevention**: OTP abuse and SMS pumping, bot sign-ups, coupon and referral fraud, scraping of paid templates (signed URLs, watermarking, hotlink protection), DDoS (CDN/WAF), and rate limiting per IP/user/device.
4. **Content moderation** for user-uploaded photos and text (offensive and illegal content), in line with the IT Rules 2021.
5. **Secrets management, encryption** at rest and in transit, least-privilege IAM, a dependency/supply-chain scanning policy, and backups.
6. **DPDP Act 2023 compliance**: consent, a notice in all 4 languages, data minimisation, user rights (access/erase), and breach notification.

## Phase 8 — Roadmap, team and cost
1. A milestone roadmap (MVP → V1 → V2) with scope for each phase.
2. Recommended team composition for the MVP.
3. A monthly infrastructure cost estimate (₹) at 10K, 100K and 1M MAU, with the main cost drivers (video rendering, CDN egress) and how to optimise them.
4. The top 10 risks (product, technical, regulatory, market), each with a mitigation. Include low willingness to pay, a large free player (e.g. Canva or a WhatsApp-native app) copying the idea, and the cost of video rendering at festival peaks.
5. A launch go-to-market plan built around the festival calendar, counted from `<today>`: which festival to launch on, and why, allowing for build time and 6–8 weeks of template production. Cover acquisition channels with ₹ budgets and expected CAC: WhatsApp sharing, Instagram Reels, regional YouTube creators, SEO for "<festival> wishes in <language>" searches, Play Store optimisation, and business partnerships.

</instructions>

<output_format>
- Write in clear, professional English. Use Markdown headings that match the phase and section numbers above.
- Use tables for comparisons, alert definitions, event taxonomies and schemas.
- Use **Mermaid** for architecture, ER, sequence and state diagrams.
- Use SQL DDL for the key table schemas.
- Use ₹ for all prices and costs.
- End each phase with: (a) a 3–5 bullet **"Key decisions"** summary and (b) an **"Open questions for the founder"** list.
- After **each phase**, stop and wait for me to reply "continue" (or give feedback) before starting the next one.
</output_format>

<quality_bar>
Before you finish each phase, check your work against this list:
- Is every recommendation specific to India and justified, not generic?
- Did I commit to a choice where a decision was needed?
- Is every persuasive UX element backed by real data and compliant with the dark-pattern guidelines?
- Could an engineering team start building from this without guessing about payments, refunds, logging or security?
- Are all numbers labelled as verified, estimate or assumption?
- Did I treat willingness to pay as the riskiest assumption and give a way to test it?
- Did I design for the recipient and the viral loop, not only the sender?
</quality_bar>

Begin with **Phase 1**.
```

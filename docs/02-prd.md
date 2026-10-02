# 02 — Product Requirements Document (PRD)

**Product:** E-Cards for India (working name; see the naming question in §12)
**Status:** Draft v1 for founder review · **As of:** 2 October 2026 · **Owner:** Product
**Related:** `01-market-research.md` · `03-ux-and-conversion.md` · `04-system-design.md` · `05-payments-and-refunds.md` · `06-observability-and-analytics.md` · `07-security-and-compliance.md` · `08-roadmap-team-cost-gtm.md`

Labels: **[V]** sourced (see `01-market-research.md`) · **[E]** estimate · **[A]** assumption to validate.

---

## 1. Problem, vision, goals and non-goals

### 1.1 Problem
Indians send billions of greetings and invitations through WhatsApp every festival and life event. Today they choose between:
- **Free forwards and generic images:** impersonal, often low quality, not in their language.
- **App-only subscription products** (e.g. Crafto): good personalisation, but they need an install, push autopay subscriptions, and users report refunds being refused **[V]**.
- **Invitation designers and platforms:** expensive (₹500–₹4,999 **[V]**), often slow, and weak in Marathi and Tamil at the premium end **[A]**.
- **Canva:** powerful but too complex for a 50-year-old who wants a Diwali card in 60 seconds.

The result: people who want a **beautiful, personal card in their own language, quickly, at a fair one-time price, delivered to WhatsApp** have no product built for them.

### 1.2 Vision
> **Make every Indian celebration feel personal, in every language, in under a minute.**

### 1.3 Product principles
1. **Language-native, not translated.** Copy is written by native speakers, with typography that respects each script.
2. **Personal in 3 taps.** Guided slots (photo, names, message) instead of a blank canvas.
3. **WhatsApp is the home screen.** Every output is optimised for WhatsApp chats and Status.
4. **The recipient is our next user.** The card page is fast, ad-free and beautiful, with a gentle "make your own".
5. **Honest commerce.** Real discounts, real bestsellers, pay once, automatic refund if anything fails.
6. **Light enough for Bharat.** Fast on a ₹8,000 Android phone over patchy 4G.

### 1.4 Goals: MVP (first 6 months after public launch)

| # | Goal | Measure | Target |
|---|---|---|---|
| G1 | Prove people will pay | Monthly net revenue (after GST) | ≥ ₹4 lakh by month 3; ≥ ₹10 lakh by month 6 **[A]** |
| G2 | Prove the viral loop | Viral coefficient K (new creators per creator through recipient pages) | ≥ 0.15 **[A]** |
| G3 | Earn trust | Payment success ≥ 88%; render success ≥ 99.5%; 99% of failures auto-refunded within 30 min; CSAT ≥ 4.3/5 | as stated |
| G4 | Be truly vernacular | Share of sessions in a non-English UI | ≥ 50% **[A]** |
| G5 | Survive the season | Zero checkout downtime on festival days; p95 video render ≤ 60s at peak | as stated |

### 1.5 Non-goals (MVP)
- Native iOS app or native Android app. The PWA comes first; an Android wrapper follows in V1.
- A free-form design editor (Canva-style). Guided slots only.
- Physical or printed cards.
- A creator/contributor marketplace (Later).
- Corporate bulk sending and CSV personalisation (V2).
- Scheduled sending via the WhatsApp Business API (V1, because of message costs).
- Ads anywhere in the product, and **never** on recipient pages.
- International pricing and NRI-specific flows (Later).
- Photo-realistic AI imagery of people (regulatory and trust risk; see §9).

---

## 2. Success metrics

### 2.1 North-star metric
**Weekly Shared Cards (WSC):** the number of finished cards (free and paid) that are shared or downloaded in a week.
*Why:* it captures real value (a personal card sent), drives both the viral loop and revenue, and can be measured for file shares as well as link shares.

**Guardrail business metric:** Weekly Net Revenue (₹, after GST and refunds).

### 2.2 Input metrics and MVP targets

| Stage | Metric | Definition | MVP target |
|---|---|---|---|
| Acquire | Unique visitors | Monthly unique devices | 300k/month at the Holi–Gudi Padwa peak **[A]** |
| Acquire | Organic share | Share of sessions from SEO + recipient pages + direct | ≥ 60% **[A]** |
| Activate | Template view → editor start | Sessions that open the editor ÷ sessions that view a template | ≥ 25% **[A]** |
| Activate | Editor start → card completed | Previews generated with every required slot filled ÷ editor starts | ≥ 60% **[A]** |
| Activate | Completed → shared/downloaded | ≥ 80% **[A]** |
| Monetise | Greeting conversion | Paying greeting users ÷ monthly creators | ≥ 3% **[A]** |
| Monetise | Invitation conversion | Paid invitations ÷ invitation editor starts | ≥ 15% **[A]** |
| Monetise | Payment success rate | Captured ÷ checkout attempts (UPI and overall) | UPI ≥ 85%, overall ≥ 88% **[A]** |
| Monetise | ARPPU | Revenue per paying user per month | ₹70 greetings, ₹350 invitations **[A]** |
| Viral | Recipient → "make your own" click | Clicks ÷ recipient page views | ≥ 8% **[A]** |
| Viral | Recipient → creator | Recipients who complete a card within 7 days ÷ recipient page views | ≥ 3% **[A]** |
| Retain | Festival-to-festival return | MAU in festival window N who return in window N+1 | ≥ 25% **[A]** |
| Retain | Repeat purchase | Payers who buy again within 90 days | ≥ 30% **[A]** |
| Quality | Render success | Successful renders ÷ render jobs | ≥ 99.5% |
| Quality | Render time p95 | Image ≤ 5s; video ≤ 60s (≤ 90s on festival day) | as stated |
| Trust | Refund rate | Refunds ÷ paid orders | ≤ 2% (automatic + manual) |
| Trust | Auto-refund latency | Time from fulfilment failure to refund initiated | 99% ≤ 30 min |
| Trust | Support load | Tickets per 100 orders | ≤ 3 |
| Performance | Recipient page LCP p75 (mobile 4G) | ≤ 1.5s |

---

## 3. Riskiest assumptions and validation plan

**Today is 2 October 2026. Diwali is 8 November 2026** (Dussehra 20 Oct; Dhanteras 6 Nov; Bhai Dooj ~10–11 Nov) **[V]**. The Nov–Dec wedding season follows. That gives two cheap, real-world test windows **before** the MVP is built.

| # | Riskiest assumption | Why it could kill us | Cheap test (before or alongside the build) | Pass bar |
|---|---|---|---|---|
| R1 | Users will **pay ₹19–₹49 per premium greeting** on the web, without a subscription | Free alternatives are everywhere; Crafto monetises through subscriptions **[V]** | **Diwali 2026 concierge pilot (10 Oct – 11 Nov).** A landing page in Hindi and Marathi with 20 premium templates (made in Canva/After Effects). Users fill a form with name and photo, pay via Razorpay Payment Links at **₹19 / ₹29 / ₹49** (randomised split), and a designer delivers within 30 min. Drive ₹50k of Meta + Instagram ads and creator posts. | ≥ 2% of landing visitors pay; ≥ 300 paid orders; CAC ≤ ₹60 |
| R2 | Families will buy **video invitations from a new brand** at ₹149–₹799 | Trust in an unknown brand for a once-in-a-lifetime event | **Wedding-season concierge (Nov–Dec 2026).** 10 Marathi + 10 Hindi + 5 Tamil video invite templates. Free watermarked preview sent on WhatsApp; pay to unlock. Instagram/Meta ads on "wedding invitation video in Marathi/Tamil". | Preview → paid ≥ 20%; ≥ 100 paid invites; NPS ≥ 40 |
| R3 | **Recipients become creators** (K ≥ 0.15) | Without virality, CAC is too high (Crafto spends 62% of costs on ads **[V]**) | Pilot share links point to a simple card page with "Make your own". Track recipient → form-start → paid. | Recipient → creator ≥ 2% in the pilot |
| R4 | We can supply **enough high-quality templates in 4 languages** at a sustainable cost | The catalogue is the product; Marathi and Tamil typography skills are scarce | Commission 10 image + 3 video templates from 3 freelancers per language. Measure cost, time, rework and the native-speaker QA pass rate. | Image ≤ ₹2,000, video ≤ ₹10,000, ≤ 5 working days, ≥ 80% pass QA first time |
| R5 | **Video render cost and time** are acceptable at festival peak | A slow or expensive render pipeline kills margins and the experience | 2-week technical spike: FFmpeg compositing + Chromium-rendered text layers on 3 templates; load test at 50 renders/min. | p95 ≤ 45s for 30s/720p; cost ≤ ₹0.50 per render |
| R6 | **SEO** can bring regional-language festival traffic | Organic share is key to a low CAC | Publish 60 pages before Diwali ("Diwali wishes in Marathi/Tamil/Hindi", "Bhai Dooj wishes in Marathi", etc.) with text wishes + pilot template links. | ≥ 50k impressions and ≥ 2k clicks in Search Console by 15 Nov |

**Decision rule:** if R1 fails but R2 passes, move MVP scope further toward invitations (invitations-first; greetings free only). If R2 fails, re-examine positioning before building video invitations.

---

## 4. Monetisation and pricing

### 4.1 Model summary
- **Free:** the large majority of festival and wish templates, as **image** cards with name and photo personalisation, link sharing and 1080px download. A small corner brand mark acts as attribution and marketing; it never covers content.
- **Pay per item ("sachet"):** premium greetings and video greetings.
- **Pay per event:** invitations, in tiers.
- **Packs:** festival bundles.
- **V1:** an optional **Utsav Pass** for heavy senders, and a **Business Plan** for shopkeepers.
- **Later:** a creator marketplace and **digital shagun/gifting**.
- **No ads** in the MVP. Ads to Indian audiences earn little, and the 123Greetings lesson is that ads spoil the moment **[V]**.

All prices are **GST-inclusive** (18%) **[A: confirm SAC code and rate with a CA]**.

### 4.2 Price list (launch hypothesis; validate in R1/R2)

| Product | What you get | Price (₹, incl. GST) | Notes |
|---|---|---|---|
| Free greeting (image) | Name + photo, 1080px, link + download, small corner mark | **0** | Acquisition engine |
| Premium greeting (image) | Premium art, no brand mark, 2160px, 3 free re-edits within 7 days | **19** | Impulse price point |
| Premium greeting (video) | 15–30s animated card with music, 720p/1080p, no mark | **29–49** | By template tier |
| Festival pack | e.g. *Diwali 5-day pack*: Dhanteras, Choti Diwali, Diwali, Govardhan/Padwa, Bhai Dooj; premium image + 1 video | **79** (vs ₹145 bought separately) | Savings shown = sum of the individual prices (truthful) |
| Image invitation | Classic ₹49 / Premium ₹99; event page with map + add-to-calendar | **49 / 99** | Birthdays, pujas, griha pravesh |
| Video invitation: Essential | 30–45s, 1–2 photos, 1 language, event page | **149** | |
| Video invitation: Premium (**Most popular**) | 45–75s, up to 6 photos, music choice, RSVP, unlimited edits for 30 days | **399** | The target choice in the price ladder |
| Video invitation: Royal | 60–120s, up to 12 photos, multiple events (haldi/mehendi/sangeet), illustrated or caricature-style art, RSVP + guest list export | **799** | Anchor tier |
| Wedding bundle | Royal video + 3 ceremony image cards + RSVP + 2 language versions | **1,199** | Saves vs. buying separately |
| Add-on: second language version | Same invite, translated by our template team | **+99** | High perceived value for mixed families |
| *V1* Utsav Pass | Unlimited premium greetings (image + video), 20% off invitations | **99/quarter or 299/year** | Opt-in autopay; one-tap cancel |
| *V1* Business Plan | Logo, shop name, phone and address on every card; a daily "today's post"; GST invoice | **149/month or 999/year** | Priced between the ₹99/yr and ₹199/mo competitors |

**Price-honesty policy (binding):**
1. A strike-through "MRP"/"regular price" is shown **only** if the item was sold at that price for **at least 30 of the previous 90 days**. The data comes from `price_history`.
2. At public launch, items are sold at the regular price first (soft launch Jan–Feb 2027). That makes the festive discounts at Holi 2027 genuine.
3. Festive discounts end exactly when stated. Countdown timers point to a real festival date or a real, logged campaign end time.
4. No hidden fees. The price on the card page is the price at checkout.

### 4.3 Coupons, referrals and credits
- **First purchase:** 50% off up to ₹50 (one per verified phone number).
- **Referral:** the referrer gets ₹20 credit when a referred friend makes their first paid purchase; the friend gets ₹20 off. Credits expire after 180 days, and the expiry is shown on every surface where credits appear.
- **Unlock by sharing** (greetings only): share 3 free cards to unlock one premium image card per festival. We verify the share action, not the recipient (no spam incentive).
- **Auto-apply:** the checkout automatically applies the best eligible coupon and shows the saving.

### 4.4 Revenue beyond individual purchases (evaluated)

| Stream | Decision | Rationale |
|---|---|---|
| Business Plan (micro-businesses) | **V1** | Proven demand; competitors price at ₹99/yr–₹199/mo **[V]**. We add premium design + 4 languages. Low engineering cost: a branding layer on existing templates. |
| Corporate/HR bulk | **V2** | Needs CSV personalisation, brand kits, invoices and sales effort. |
| Digital shagun / cash gift with a card | **Later** (design now) | We **never hold the money**. The invitation or card page shows a "Send shagun" button that opens a **UPI intent link to the host's own UPI ID**, so money goes straight from guest to host. No payment-aggregator or wallet licence needed. Only P2P *collect* requests were stopped by NPCI from 1 Oct 2025; **intent/QR** payments stay allowed **[V]**. Gift cards would come through a partner (Later). Confirm with counsel. |
| Advertising | **No** | It spoils the moment and earns little. Revisit only for a separate, non-recipient surface. |
| Creator marketplace | **Later** | 30–50% revenue share to independent designers once demand is proven. |

### 4.5 Unit economics (per paid order) **[E]**

Assumptions: gateway fee ~2% + 18% GST on the fee **[V: published standard rates; negotiate]**; video render ~₹0.10–₹0.30 per render on spot CPU **[E]**; media delivery through a zero-egress object store and CDN **[E]**; ₹88/US$.

| Line (₹) | Premium video greeting @ ₹49 | Premium video invite @ ₹399 |
|---|---|---|
| Price paid (incl. GST) | 49.00 | 399.00 |
| − GST (18% inclusive) | −7.47 | −60.86 |
| **Net revenue** | **41.53** | **338.14** |
| − Gateway fee (2% + GST on fee) | −1.16 | −9.42 |
| − Render (incl. re-edits) | −0.30 | −1.50 (≤ 10 re-renders) |
| − Storage + delivery (incl. RSVP page views) | −0.10 | −2.00 |
| − AI text and background removal | −0.20 | −0.50 |
| − Refund/failure provision (1.5%) | −0.62 | −5.07 |
| − Support (allocated) | −0.50 | −5.00 |
| **Contribution per order** | **≈ 38.65 (93% of net)** | **≈ 314.65 (93% of net)** |

**Template economics:** an image template costs ₹800–₹2,500 and a video template ₹4,000–₹15,000 **[E]**. A ₹10,000 video-invite template pays back after **~32 Premium sales**. Track **template ROI** in the admin panel and retire templates that don't sell.

**CAC guardrails:**
- Blended CAC per first-time payer: **≤ ₹60** for greetings, **≤ ₹250** for invitations.
- 12-month LTV targets: ₹150 for a greetings payer, ₹450 for an invitation buyer (about 1.3 family events a year plus greetings) **[A]**.
- Keep LTV:CAC ≥ 2.5 on a 12-month basis.

---

## 5. Feature list (MoSCoW × phase)

**M** = Must, **S** = Should, **C** = Could, **W** = Won't (this phase). Phase: **MVP** (launch), **V1** (+6–9 months), **Later**.

### 5.1 Discovery

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Festival-calendar home: a hero for the next festival with a countdown to its **real date**, then "Coming up" and "Celebrations" rails | M | MVP | Dates come from the festival table (by region and year) |
| Category tree: Festivals · Wishes (Birthday, Anniversary, Congratulations, Thank you, Condolence) · Invitations (Wedding, Engagement, Haldi/Mehendi/Sangeet, Griha Pravesh, Birthday party, Puja/Katha, Naming ceremony, Baby shower) | M | MVP | Business category in V1 |
| Language-aware catalogue: each template has native **variants** per language | M | MVP | Not machine-translated |
| Search across 4 languages with aliases (Diwali / Deepavali / दिवाली / தீபாவளி / "diwali ki shubhkamnayein") | M | MVP | Postgres trigram + alias table; dedicated engine in V1 |
| Filters: Free/Premium, Image/Video, price band, format (Status 9:16 / Square / Landscape), community style (Marathi, Tamil, Punjabi, Bengali, Gujarati, Muslim, Christian…) | S | MVP | |
| Regional boost: Gudi Padwa for Maharashtra users, Pongal for Tamil Nadu (geo-IP state + chosen language) | S | MVP | |
| Badges: Bestseller, Trending in your state, New, Most loved this festival | M | MVP | Rules in `03-ux-and-conversion.md` §3 |
| Personalised recommendations ("Because you made…") | C | V1 | |
| "Good Morning" / daily-wish collection | S | V1 | For daily habit and retention |

### 5.2 Editor and personalisation

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Guided slots per template: names (sender/family), relation, photo(s), message, date/time/venue (invitations), city | M | MVP | Slot schema in `04-system-design.md` §5 |
| Live preview that updates as the user types | M | MVP | Client-side for image cards; overlay preview for video |
| Photo upload with crop, rotate and auto-fit to a frame | M | MVP | Resumable upload; client-side compression |
| Background removal for the subject photo | S | MVP | A key driver in Indian greeting apps; server model with a cost cap |
| Native-script typing via the device keyboard | M | MVP | |
| Transliteration suggestions (type "Sunita Sharma" → सुनीता शर्मा / சுனிதா ஷர்மா) | S | MVP | Open-source IndicXlit (AI4Bharat), self-hosted |
| Curated fonts per script (3–5) and colour themes per template | M | MVP | Licensed for embedding (see §9) |
| **AI wish writer**: a 2–3 line wish in the chosen language and tone (Traditional / Emotional / Funny / Formal), with relation and festival context | S | MVP | Small LLM; rate-limited; moderation filter |
| Music picker for video (3–5 licensed tracks per template) | M | MVP | No film songs (see §9) |
| Output formats: Status 9:16, Square 1:1, Landscape 16:9 (invites) | M | MVP | Per-template support |
| Free/preview brand mark; premium unlock removes it | M | MVP | |
| Post-purchase edits: greetings 3 re-edits within 7 days; invitations unlimited within 30 days | M | MVP | Fixes typos without a refund |
| Save drafts, restore on return | M | MVP | Local draft without an account; server draft when logged in |
| AI-stylised photo effects (e.g. watercolour) | C | V1 | Clearly labelled; no photo-realistic people |

### 5.3 Rendering and output

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Server-side final render: image ≤ 5s p95; video ≤ 60s p95 | M | MVP | Queue-based; progress UI |
| "We'll notify you when it's ready" (web push / WhatsApp / email) if render is slow | M | MVP | Peak-day protection |
| Formats: JPG/WebP image; MP4 H.264 + AAC video (WhatsApp-compatible, ≤ 16MB) | M | MVP | |
| Pre-render cache for popular template + empty-slot combinations | S | MVP | Festival-day speed |
| Auto-retry render, then automatic refund on failure | M | MVP | See `05-payments-and-refunds.md` |

### 5.4 Sharing, recipient experience and virality

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| **Share to WhatsApp as a file** (Web Share API with files on Android Chrome) plus a link fallback (wa.me with the card URL) | M | MVP | Files play in WhatsApp chat and Status |
| Unique card link with an unguessable slug, rich OG preview image, title in the card's language | M | MVP | |
| Download (image/video) with a long-press hint for in-app browsers | M | MVP | |
| Instagram Story / Facebook share through the system share sheet | S | MVP | |
| **Recipient page**: LCP ≤ 1.5s on 4G, no login, **no ads**, autoplay muted video with a tap for sound, "Send wishes back" reply, **"Make your own [festival] card →"** that opens the same template | M | MVP | Main viral surface |
| Invitation event page: details, Google Maps link, add-to-calendar (ICS), countdown, gallery | M | MVP | |
| RSVP (Attending / Maybe / Not attending + number of guests + message) with a host dashboard and CSV export | S | MVP | Included in Premium/Royal invites |
| "Send shagun" UPI intent button to the host's UPI ID | C | Later | Host opt-in; we never hold funds |
| Scheduled send through the WhatsApp Business API | C | V1 | Per-message cost; needs consent |

### 5.5 Retention

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Festival reminders (opt-in): "Pongal is in 3 days — your card is ready to personalise" via web push / WhatsApp / email | S | MVP | Frequency cap: 2 a week, 1 a day around festivals |
| Personal calendar: birthdays and anniversaries the user saves (with consent) | S | V1 | |
| "My people" saved recipients (name + relation, no phone needed) | C | V1 | Speeds up personalisation |
| Daily Good Morning collection + optional daily reminder | S | V1 | |

### 5.6 Accounts and identity

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Create and share free cards **without an account** (guest) | M | MVP | Lowest friction |
| Phone OTP login: WhatsApp OTP first, SMS fallback | M | MVP | SMS needs TRAI DLT template registration |
| Google sign-in | M | MVP | |
| Phone verification required at checkout ("guest checkout" = verified phone, no profile) | M | MVP | Needed for receipts, recovery and refunds |
| My Cards: drafts, purchased, re-download, re-edit window | M | MVP | |
| Orders and GST invoices (PDF) | M | MVP | |
| Account deletion and data export (DPDP) | M | MVP | |
| Truecaller one-tap verification (Android) | C | V1 | |

### 5.7 Checkout and payments

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Hosted checkout (Razorpay Standard Checkout): **UPI intent** (mobile), **UPI QR** (desktop), cards, net banking, wallets | M | MVP | No card data touches our servers |
| GST-inclusive single price; coupon field + auto-apply best coupon | M | MVP | |
| Clear "payment pending" state with automatic status polling and recovery | M | MVP | Common with UPI |
| Automatic refund on fulfilment failure; refund status in My Orders | M | MVP | |
| Festival packs and bundles | S | MVP | |
| Utsav Pass with opt-in UPI Autopay, pre-debit notice, one-tap cancel | S | V1 | |
| Business Plan subscription | S | V1 | |
| Second gateway + routing for failover | S | V1 | Before Diwali 2027 |
| International cards / multi-currency | C | Later | NRIs |

### 5.8 Template supply

| Feature/process | Priority | Phase | Notes |
|---|---|---|---|
| Template spec: slots, safe zones, **per-script line-length limits**, long-name behaviour (auto-shrink, then wrap) | M | MVP | |
| Production calendar: brief **8 weeks** before each festival; publish **2 weeks** before | M | MVP | |
| Launch catalogue: ≥ 400 image + 60 video greetings; ≥ 80 invitations (30 video), across 4 languages | M | MVP | See the breakdown in `08-roadmap-team-cost-gtm.md` |
| Native-speaker QA checklist (shaping, matras, conjuncts, honorifics, festival names) | M | MVP | |
| AI-assisted background/illustration generation with human curation; no photo-realistic people | S | MVP | Speeds up supply; labelled where needed |
| Template ROI analytics (cost vs. revenue, views → buys) | S | MVP | Admin |
| Contributor marketplace (upload, review, revenue share) | C | Later | |

### 5.9 Admin and CMS

| Feature | Priority | Phase | Notes |
|---|---|---|---|
| Template upload: assets, slot JSON, per-language variants, preview, publish/unpublish, scheduling | M | MVP | |
| Pricing with automatic `price_history` and discount-eligibility check | M | MVP | Enforces the honesty policy |
| Festival calendar management (date per year per region, collection mapping) | M | MVP | |
| Banners and campaigns (start/end, audience by language/state) | M | MVP | |
| Orders, payments and refunds dashboard; manual refund with reason; **maker-checker approval for refunds > ₹500** | M | MVP | |
| Moderation queue for flagged uploads and text | M | MVP | |
| Coupon management, badge rule configuration | S | MVP | |
| Role-based access (admin, content, support, finance) and an audit log of every admin action | M | MVP | |

---

## 6. User stories and acceptance criteria (core flows)

### US-01 Discover the next festival (Sunita, Hindi)
*As a Hindi-speaking user, I want to see the upcoming festival's cards first, so I can send wishes quickly.*
- **Given** my language is Hindi and my state is Madhya Pradesh, **when** I open the home page 5 days before Diwali, **then** the hero shows "दिवाली — 5 दिन बाकी" with Diwali cards, and the countdown matches the stored Diwali date.
- **Given** the next festival is regional (e.g. Gudi Padwa) and my state is Maharashtra or my language is Marathi, **then** it is ranked above festivals not celebrated in my region.
- The home page renders a usable first view (LCP) in ≤ 2.5s at p75 on a mid-range Android over 4G.

### US-02 Search in Hinglish
*As Anjali, I want to type "bday wish for bestie" and find relevant cards.*
- **Given** the query "bday wish for bestie", **then** results include birthday templates tagged *friend*, with Hinglish-styled templates ranked first.
- **Given** "deepavali" or "தீபாவளி", **then** Diwali templates appear, with Tamil variants first when the UI is Tamil.
- No-result queries are logged (`search_no_results`) and show popular festival collections.

### US-03 Personalise a greeting with name and photo
*As Sunita, I want my photo and name on a Diwali card in 3 taps.*
- **Given** I open a template, **when** I tap "Add your photo", **then** I can pick from the gallery or camera. The photo is compressed on the device to ≤ 1.5MB and uploaded with a progress bar. If the network drops, the upload resumes.
- **When** background removal is on for this template, **then** my cut-out appears in the frame within 5s (p95), with an "Undo" option.
- **When** I type my name in Latin letters, **then** native-script suggestions appear (Should), and the preview updates within 200ms.
- **Given** a name longer than the slot limit, **then** the text auto-shrinks to its minimum size, then wraps to 2 lines. If it still doesn't fit, I see "Name is too long for this design — try a shorter version" and can't continue with clipped text.

### US-04 AI wish writer
*As Karthik, I want a well-written Tamil Pongal wish for my in-laws.*
- **Given** language Tamil, festival Pongal, relation "In-laws", tone "Traditional", **when** I tap "Write for me", **then** 3 suggestions appear within 4s, each within the slot's character limit.
- Suggestions pass a safety filter (no offensive or political content). Offensive input is rejected with a polite message.
- Usage limits: 10 generations per device per day for free users; 50 for paying users.
- I can edit any suggestion before using it.

### US-05 Share a free card on WhatsApp
*As Sunita, I want to send my card to my family group.*
- **Given** my card is complete, **when** I tap "WhatsApp पर भेजें" on Android Chrome, **then** the system share sheet opens with the **image/video file** attached.
- **Given** file sharing is unsupported (iOS in-app browser, some WebViews), **then** a wa.me link opens with the card URL and a pre-filled message in my language.
- The share is logged (`card_shared`, channel = whatsapp_file | whatsapp_link | download | other).

### US-06 Buy a premium video greeting with UPI
*As Karthik, I want to pay ₹49 by GPay and get the HD video without a watermark.*
- **Given** I tap "₹49 में HD पाएं" / "Get HD for ₹49", **when** I'm not logged in, **then** I verify my phone with OTP (WhatsApp first, SMS fallback) without leaving the flow.
- The checkout shows a single **GST-inclusive** price, the auto-applied best coupon, and the payable amount. **No new fees appear after this point.**
- **When** I choose UPI on mobile, **then** my UPI app opens through intent; on desktop, a QR is shown.
- **When** payment is captured (confirmed by verified webhook or server-side status check, never by the client callback alone), **then** the order moves to PAID and the render starts automatically.
- The HD video is ready in ≤ 60s p95, and I get an on-screen notification plus a WhatsApp/email receipt with a GST invoice link.

### US-07 Payment pending or failed
- **Given** I return from the UPI app without a confirmed result, **then** I see "Checking your payment… (this can take up to 2 minutes)". The page polls the server every 3s for up to 2 minutes, then every 30s for up to 15 minutes (polling stops if I leave the page).
- **Given** the payment fails, **then** I see the reason in plain language ("Payment declined by bank — no money was deducted") and one tap to retry with another method. My card and personalisation are preserved.
- **Given** money was debited but the order isn't confirmed within 15 minutes, **then** the system reconciles with the gateway and either fulfils or refunds automatically. I'm told which, within 30 minutes, by WhatsApp/SMS/email.

### US-08 Automatic refund on render failure
- **Given** my order is PAID and the render fails 3 times (with automatic retries over ≤ 10 minutes), **then** a full refund is initiated automatically. The order shows "Refunded — ₹49 will reach your account in 1–5 working days" (or "instantly" when an instant UPI refund is used), and I'm notified.
- An email alert goes to the on-call engineer if the failure rate for that template is > 2% within 15 minutes (see `06-observability-and-analytics.md`).
- I'm offered a free premium card credit (₹19) as an apology, shown clearly with its expiry.

### US-09 Create a wedding video invitation with RSVP
*As Rahul & Priya, we want a Marathi + English video invite with RSVP.*
- **Given** I choose a Premium video invite template, **then** I see the slots: bride and groom names, parents' names (in an order I can change), date/time, venue + map pin, up to 6 photos, music.
- **When** I tap "Preview", **then** a **watermarked** low-resolution preview is generated (≤ 90s), and I can share it with family **before paying**.
- **When** I pay ₹399 (or ₹1,199 for the Wedding bundle), **then** I get the HD video, an event page with RSVP, and unlimited re-edits for 30 days. Each re-edit regenerates the video and **keeps the same event link**.
- **Given** the second-language add-on, **then** the Marathi and English versions share one event page with a language toggle.
- The host dashboard shows RSVP counts and supports CSV export. Guests aren't asked to log in and only give a name and number of guests (a phone number is optional, with stated consent).

### US-10 Recipient becomes creator
- **Given** I open a card link in the WhatsApp in-app browser, **then** the card shows in ≤ 1.5s LCP (p75, 4G), with no login wall and no ads.
- I see "Send wishes back" (reply text, delivered to the sender's My Cards in V1) and **"Make your own [festival] card →"**.
- **When** I tap "Make your own", **then** the same template opens in my browser language (or the card's language), with the slots empty.
- The attribution (`ref_card_id`) is stored for viral-coefficient analysis.

### US-11 Switch language
- **Given** I switch from English to Marathi, **then** the UI, collection names, festival names and template **variants** switch to Marathi. An in-progress card keeps its content, and I'm asked whether to switch the template variant too.
- My choice is remembered (cookie + profile). The app never auto-switches after an explicit choice.

### US-12 Admin publishes a festival collection
- **Given** I'm a content manager, **when** I upload a template with assets, the slot schema and 4 language variants, **then** the validation checks: required fonts are licensed and present, each variant renders all slots with the sample long-name test set, and the output is ≤ 16MB.
- **When** I set the price and schedule (publish 14 Nov 2026 00:00 IST), **then** the template goes live at that time, and the price is written to `price_history`.
- **When** I try to set a strike-through "regular price", **then** the system blocks it unless the price-history rule (§4.2) is met.

### US-13 Support processes a manual refund
- **Given** a user complains about a quality issue within 7 days, **when** a support agent issues a refund ≤ ₹500 with a reason code, **then** it's processed immediately. **When** the refund is > ₹500, **then** a finance approver must approve it (maker-checker).
- Every action is written to `audit_logs` with the actor, before/after state and IP.

### US-14 Privacy: consent and deletion
- **Given** first use, **then** a notice in my language explains what we collect (photos, names, phone) and why. Analytics cookies are off until I accept.
- **When** I request account deletion, **then** my photos, drafts and renders are deleted within 30 days. Invoices are retained as required by tax law, and I'm told this.

---

## 7. Localisation strategy

### 7.1 Scope
- **UI languages at launch:** हिन्दी (hi), English (en), मराठी (mr), தமிழ் (ta).
- **Content:** every template has native variants (copy written natively, not machine-translated). Not every template needs all 4 variants; festival relevance decides. For example, Gudi Padwa needs mr/hi/en, and Pongal needs ta/en/hi.
- **Next languages (V1/Later):** Telugu, Bengali, Gujarati, Kannada. The architecture keeps adding a language cheap: strings in the i18n system, template variants in data, and fonts as configuration.

### 7.2 Framework and workflow
- **i18n:** ICU MessageFormat (plurals, gender, honorifics) through `next-intl` in the web app. Server-generated messages (receipts, notifications) use the same message catalogues.
- **Workflow:** source strings in English → a translation-management tool (e.g. Tolgee self-hosted or Crowdin) → **professional native translators** + an in-house reviewer per language → a shared glossary (festival names, honorifics like *ji*, *avargal*; formal vs. informal *tu/tum/aap*; Tamil *nee/neenga*).
- **Tone:** respectful by default (*aap*, *neenga*). Youth collections can use a casual tone.
- **Pseudo-localisation in CI** catches clipped text. Tamil and Marathi strings often run **30–40% longer** than English **[E]**.

### 7.3 Typography (the quality bar competitors miss)
- **Fonts (OFL, free to embed):**
  - Devanagari: Noto Sans/Serif Devanagari, Mukta, Hind, Baloo 2, **Tiro Devanagari Marathi** (Marathi-specific letter forms).
  - Tamil: Noto Sans/Serif Tamil, Mukta Malar, Catamaran, Baloo Thambi 2.
  - We'll license or commission 2–3 **premium display faces** per script for paid templates.
- **Shaping:** set the correct `lang` (`mr` vs `hi`) so OpenType language features apply (e.g. Marathi forms of श and ल). Both preview and final render use **HarfBuzz-based shaping**: browser preview, and Chromium-rendered text layers on the server. Preview and final output must match exactly; never use a canvas library without complex-script shaping.
- **Web performance:** subset fonts by `unicode-range` per script, preload only the active script, `font-display: swap`.
- **QA set:** a test corpus of long names, conjuncts (क्ष, ज्ञ, श्र), Tamil grantha letters (ஜ, ஷ, ஸ, ஹ), mixed-script names, and emoji in names.

### 7.4 Numbers, dates and currency
- Indian digit grouping (₹1,19,900) through `Intl.NumberFormat('en-IN')`. Western digits in all languages, since Tamil and Devanagari digits are rarely expected.
- Dates use `Intl.DateTimeFormat` in each locale. Invitations can optionally show the Hindu calendar tithi (V1).
- Time format: 12-hour with the localised "सुबह/शाम" style day-period words in Hindi and Marathi.

### 7.5 Language detection and switching
- First visit: the explicit language in the URL (`/mr/...`) wins. Otherwise the browser `Accept-Language` plus the geo-IP state suggests a language through a **one-tap language picker in all 4 scripts** (not a silent redirect).
- The choice persists in a cookie and the user profile, and the switcher is always visible in the header.

### 7.6 Hinglish and mixed-language use
- Hinglish (and "Marathi in English letters") is supported as a **search and content style**, not as a UI language:
  - Search aliases map transliterations to festivals and occasions ("shubh deepawali", "vadhdivas" → birthday in Marathi, "iniya pongal").
  - A "Hinglish wishes" content collection serves younger users.
  - Transliteration suggestions help with name entry (IndicXlit).

### 7.7 Festival names and dates
- The festival table stores **per-year, per-region dates**. Lunar festivals move about 11 days earlier each year, and some (e.g. Makar Sankranti/Pongal, Eid) differ by region or depend on moon sighting.
- Source: a licensed panchang data feed or careful manual curation against the Government of India holiday list + 2 reputable panchangs. A **content owner signs off on next year's calendar by 1 September**.
- Moon-sighting festivals (Eid) show the collection from 3 days before the expected date, with the copy "Eid Mubarak" (no date claims).

### 7.8 SEO per language
- URL structure `/{locale}/{collection}/{slug}`, with **Latin transliterated slugs** for clean sharing (e.g. `/mr/sana/gudi-padwa-shubhechha`) and localised titles and H1s.
- `hreflang` between language versions, a sitemap per locale, structured data (`ImageObject`/`CreativeWork`), and localised OG images.
- Content pages for search demand: "Diwali wishes in Marathi" style pages with **text wishes** + templates. These pages are produced every festival cycle.

---

## 8. Edge cases and failure states (user's point of view)

| Situation | What the user experiences |
|---|---|
| Very long name / multiple names | Auto-shrink → wrap → friendly "too long" message; never clipped text in the output |
| Name in a different script than the template | Allowed; the font falls back to the matching script family; a preview warning if the style mismatches |
| Emoji in a name/message | Rendered with Noto Color Emoji; blocked in slots that are marked "text-only" |
| Photo too small/dark/blurry | A gentle warning with "Use anyway / Choose another"; auto-enhance (Could) |
| Group photo with background removal | Detects multiple people; offers "Keep background" |
| Upload interrupted | Resumes automatically; the draft is kept locally |
| User closes the browser mid-edit | Draft restored on return (local storage; server if logged in) |
| UPI app not installed / intent fails | Fall back to QR or "Enter UPI ID"; other methods visible |
| Payment success but browser closed | Webhook completes the order; WhatsApp/SMS/email with the download link |
| Double tap on "Pay" | A single order (idempotency key); the second tap is ignored |
| Price changed while on checkout | The price shown at checkout start is honoured for 30 minutes |
| Coupon invalid or expired | A clear reason, with the best valid alternative auto-applied |
| Render queue busy on festival day | "You're #120 in line — about 2 minutes. We'll notify you." No silent wait |
| Render fails | Auto-retry; if still failing, automatic refund + apology credit (US-08) |
| Typo noticed after purchase | Free re-edit within the window; no refund needed |
| Recipient on an old browser | A static image fallback + download link |
| WhatsApp in-app browser can't download | "Tap ⋮ → Open in Chrome" hint, or long-press to save |
| iOS share limitations | Link share as the default on iOS; download as the secondary option |
| Wrong festival date for a region | Region-specific dates; a "Report an issue" link on the festival page |
| Offensive text in a card | Blocked at entry with a polite message; repeat offenders are rate-limited; shared pages can be reported |
| Card link shared publicly | Unguessable link; the sender can **make the card private or delete it** from My Cards |
| User wants their data deleted | A self-serve deletion; confirmation message; done within 30 days |
| OTP not received | Resend after 30s; switch channel (WhatsApp ⇄ SMS); max 5 attempts an hour |

---

## 9. Content and legal risks

| Risk | Requirement |
|---|---|
| **Music copyright** | **No film or Bollywood songs.** Recorded music rights sit with labels (represented by PPL) and composition rights with IPRS **[V]**. Public-performance single windows (e.g. Sangeet Dwar) don't cover putting songs into user videos. Use **commissioned or royalty-free tracks with a licence that explicitly allows embedding in user-generated videos for redistribution**; keep a licence register per track. Revisit label deals in V2 if demand is strong. |
| **Fonts** | Use only fonts whose licence allows **embedding in rendered output that is redistributed** (OFL is fine). Commercial fonts need a server/app licence. Keep a licence register. |
| **Stock art and illustrations** | Many stock licences (standard Freepik/Envato/Shutterstock) **forbid use in templates for resale**. Use commissioned art, extended licences that allow templates, or our own AI-assisted art with human curation. |
| **Religious imagery** | A content guideline: deities only in a respectful context; **never under user-editable text or behind photos**; no deity imagery in comic templates; no footwear, alcohol or meat imagery near religious symbols. Native-speaker and cultural review for each festival collection. A "Report" button on every template and shared card. |
| **Trademarks and personality rights** | No brand logos, team logos (IPL), celebrity faces or names in templates. Celebrity personality rights are actively enforced in India. |
| **AI-generated content** | Under the IT Rules amendment (in force 20 Feb 2026), **photo-realistic synthetic audio/visual content must carry a continuous visible label** **[V]**. MVP AI use is limited to **text wishes** and **stylised illustration backgrounds**. Any AI-made visual that could look real gets a visible "AI-generated" label. **No face-swap or photo-realistic edits of people.** |
| **Intermediary obligations** (user content) | Grievance officer, published rules, takedown within the mandated timelines, and a 3-hour takedown for flagged unlawful content under the 2026 amendment **[V]**. |
| **Consumer protection** | Consumer Protection (E-Commerce) Rules 2020: seller details, total price, refund and cancellation policy, grievance redressal. **CCPA dark-pattern guidelines (2023)**: see the compliance table in `03-ux-and-conversion.md` §4. |
| **Personal data** | DPDP Act 2023 + Rules (notified 14 Nov 2025; full compliance due 13 May 2027 **[V]**). Build compliant from day 1: notice in 4 languages, consent, deletion, breach plan. **Children:** accounts for 18+ only (declared). Under-18 data needs verifiable parental consent; we avoid it in the MVP. See `07-security-and-compliance.md`. |
| **Payments** | Hosted checkout (no card data), RBI tokenisation handled by the gateway, payment data stored in India (AWS Mumbai). GST invoices. See `05-payments-and-refunds.md`. |
| **Messaging** | SMS OTP needs TRAI DLT entity and template registration. WhatsApp messages need opt-in and approved templates. |

---

## 10. Platform decision

**Recommendation:** a **mobile-first responsive web app, installable as a PWA, for the MVP**. An **Android app (Trusted Web Activity wrapper) in V1** for Play Store discovery. A native iOS app only if data shows demand.

| Option | Pros | Cons | Verdict |
|---|---|---|---|
| Responsive web + PWA | Opens instantly from WhatsApp links; **SEO** for "<festival> wishes in <language>"; one codebase; fast to ship; **no Play billing fee** on digital goods | Weaker push on iOS; no Play Store discovery | **MVP** |
| Android app (TWA wrapper) | Play Store discovery (where Crafto and the poster apps win); home-screen presence; better notifications | Digital goods sold inside Play-distributed apps must use Play Billing or user-choice billing (the fee is reduced by only 4% for alternative billing) **[V]**, so it adds a 11–26% fee **[E]** | **V1**, after modelling the fee impact |
| Native Android/iOS | Best performance and offline use | Two more codebases; slower; billing fees | **Later**, if needed |

**Path:** the PWA ships Jan 2027 → the TWA Android app ships before Raksha Bandhan (Aug 2027) with Play Billing/user-choice billing for in-app purchases → re-evaluate native apps after Diwali 2027.

---

## 11. Key decisions (PRD)
- **The north star is Weekly Shared Cards.** Revenue and trust metrics are guardrails.
- **Validate before building:** a Diwali 2026 concierge pilot (pricing ₹19/₹29/₹49) and a wedding-season invite pilot (Nov–Dec 2026).
- **Pricing:** free image greetings; ₹19–₹49 premium greetings; ₹149/₹399/₹799 video invitations; packs and bundles. The Pass and Business Plan come in V1. **No ads; never on recipient pages.**
- **Honesty is built into the system:** strike-through prices need 30 of the last 90 days at that price; badges are computed from data; automatic refunds.
- **Platform:** mobile web + PWA first; Android TWA in V1; strong 4-language SEO.

## 12. Open questions for the founder
1. Confirm the **revenue mix ambition**: invitations-led (recommended) or greetings-led?
2. Approve the **Diwali 2026 pilot** budget: about ₹1–1.5 lakh (ads ₹50k + 3 freelance designers + a landing page).
3. **Brand name:** shortlist 5 names so we can check .in/.com domains and trademarks (classes 9, 35, 42).
4. **GST registration** and the entity: is the company incorporated? Payment gateway KYC needs this.
5. Is a **"from ₹19" launch price** acceptable? It's the impulse threshold; the pilot will tell us whether ₹29 converts as well.
6. Should **Muslim and Christian festivals** (Eid, Christmas) be in the MVP catalogue? Recommended: yes for Eid and Christmas in Hindi, English and Tamil.

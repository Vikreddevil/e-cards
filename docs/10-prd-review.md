# 10 — Independent PRD Review (neutral expert review)

**Reviewed:** `02-prd.md`, `03-ux-and-conversion.md`, `09-lean-launch-plan.md` (and the supporting docs 04–08)
**Date:** 25 October 2026 · **Reviewer stance:** neutral. The aim is to find what's missing or weak, not to defend earlier decisions.
**Method:** feature-by-feature comparison with similar products in India and abroad, plus a check for internal consistency and launch readiness.
Labels: **[V]** sourced · **[E]** estimate · **[A]** assumption.

---

## 1. Verdict

The PRD is **strong on research, payments, honesty and localisation**. It is **weak on the invitation host experience, recipient trust, and Indian invitation content conventions**. It also has two **planning problems** that need fixing before any build starts:

1. **The launch date in the lean plan (16 Oct) has passed, and no code exists yet.** Diwali is on 8 Nov, **14 days away**. The plan needs a new date and an explicit choice of which product leads the launch (§5).
2. **Two documents disagree** on prices, accounts, GST and scope. The PRD (`02`) describes the full product, while the lean plan (`09`) describes the launch. The review fixes this by making `09` the single source of truth for launch (§6).

### Scorecard

| Area | Score (1–5) | Comment |
|---|---|---|
| Problem, market, personas | **4.5** | Evidence-backed (Crafto revenue, invitation price ranges); clear wedge |
| Monetisation and pricing | **3.5** | Clear and honest, but **underestimates free competitors in invitations** (Crafty Art, InvitiApp's free tier, Desievite free) |
| Feature completeness: greetings | **4** | Good. Missing: sender "viewed" feedback, scheduled reminders |
| Feature completeness: invitations | **2.5** | **Biggest gap.** Missing: bride's-side/groom's-side versions, Indian family-name conventions, multi-function schedule, PDF/QR outputs, event updates, guest reminders, co-hosts |
| Recipient experience and trust | **3** | Good performance and viral loop, but **no answer to the "wedding invitation APK" scam fear** that makes Indians wary of invitation links |
| Localisation | **4.5** | Unusually deep (typography, Hinglish, festival dates) |
| Payments, refunds, security | **4.5** | Production-grade |
| Analytics and measurement | **3** | Detailed event list, but no common event format, no definition of "engagement", and the lean plan has no launch KPIs. Fixed in `11-analytics-tracking-plan.md` |
| Operations (support, content lifecycle) | **2.5** | No support model in the PRD, and no rule for how long card and event links stay live |
| Launch readiness | **2** | Date passed; scope conflict between docs; Razorpay KYC status unknown |

---

## 2. Products compared

| Product | Type | What we checked |
|---|---|---|
| **Evite** (US) | Invitations + RSVP | Automatic RSVP reminders to guests who haven't replied; event reminders to guests who said yes; free tier shows ads to guests **[V]** |
| **Paperless Post** (US) | Premium invitations | Animated envelope opening (premium); guest check-in at the event with export; per-guest "coins" pricing (~$0.50–$1.44 per guest) **[V]** |
| **Punchbowl** (US) | E-cards + invitations | **Scheduled delivery** (date, time, time zone); **"Viewed By"** list for the sender; subscription tiers $2.99–$5.99/month **[V]** |
| **Kudoboard / GroupGreeting** | Group cards | **Many contributors sign one card** (messages, photos, videos); deliver by link, slideshow or printed book; $5.99–$19.99 per board **[V]** |
| **InvitiApp** | Invitations + RSVP via WhatsApp | Free plan with RSVP, message wall, photo gallery, music and map, but **capped at 10 views** and watermarked; one-time premium from $19.99; WhatsApp RSVP alerts on premium **[V]** |
| **NIMNTRN** (India) | Interactive web invitations | RSVP, Google Maps directions, **event schedule**, **guest photo uploads to a shared gallery**, one link for WhatsApp **[V]** |
| **Crafty Art** (India) | Invitation + video maker | Hindi and English; print sizes (A4, A5, 5×7, 4×6); **video invitations marketed as free and watermark-free** **[V]** |
| **Desievite** (India) | Invitations | Free tier with watermark; RSVP; Hindi and regional templates; paid ₹70–₹499 **[V]** |
| **WedMeGood e-invites**, **WishNWed**, **VideoGiri** (India) | Wedding video invites | Prices ₹299–₹4,999; "see your invite, then pay" **[V]** |
| **Crafto** (India) | Personalised greetings | Photo + name personalisation, daily content, subscription with UPI Autopay **[V]** |
| **Canva** (India) | Design tool | Huge template library; fast-growing Indian user base **[V]** |

---

## 3. Gap analysis: what similar products have that the PRD lacks or under-specifies

Severity: **P0** = crucial before go-live · **P1** = within 30 days of launch · **P2** = later.

| # | Capability | Seen on | In the PRD? | Severity | Recommendation |
|---|---|---|---|---|---|
| G1 | **Bride's-side and groom's-side versions** of the same wedding invitation (different inviting family, different RSVP contacts) | Standard practice in Indian printed cards; Indian invite makers | Missing | **P0** | One wedding order includes **2 "side" versions** (*kanya paksh / var paksh*) that share the design and event page. A big perceived-value win at no extra cost |
| G2 | **Indian invitation content model**: inviting family names; list of relatives ("swagatotsuk", "darshanabhilashi", "nimantrak"); muhurat/time; ritual list; RSVP contact persons; children's line | Printed lagna patrika conventions; Indian invite makers | Partial (bride/groom names, parents only) | **P0** | Add **structured slots** for these, with sensible defaults per community and language. Fields are optional, so short invites stay short |
| G3 | **Religious header choice**: Ganesha / Om / Ik Onkar / Bismillah / Cross / Jain symbol / none | Indian invite makers | Missing (only a "community style" filter) | **P0** | A header-symbol slot per template where the design supports it; respectful-use rules from PRD §9 apply |
| G4 | **Multi-function schedule on one event page** (haldi, mehendi, sangeet, wedding, reception, each with its own date, time and venue) | NIMNTRN schedule; Indian wedding sites | Partial (sold as separate cards in a bundle) | **P0** for wedding Premium | Event page with a **function list**. The video shows the main event; the page shows all of them |
| G5 | **PDF and image (JPG) download for invitations** | Crafty Art print sizes; Indian families still forward PDF invites and print a few | Missing (PDF only for tax invoices) | **P0** (image + PDF), P2 (print-ready with bleed) | Every invitation exports **video + JPG + PDF**. Print-ready A5/5×7 later |
| G6 | **QR code on the invitation** linking to the event page / map / RSVP | Common on modern Indian invitations | Missing | **P1** | Optional QR slot on image/PDF invitations |
| G7 | **Recipient trust against "wedding invite APK" scams.** Police across India warn about fake WhatsApp wedding invitations carrying malware **[V]** | — (a risk, not a feature) | Missing | **P0** | (a) Share the **video file itself** as the primary option, the link second. (b) Share text says "Video invitation — no app download needed". (c) Our domain and recipient pages **never ask for any download or install**. (d) Event pages show a small "Safe link: no app, no login" line. (e) User content may only contain **allowlisted links** (Google Maps, YouTube/livestream), never arbitrary URLs. (f) A "Report" button. This also stops scammers using *our* platform to host fake invites |
| G8 | **Freemium invitations** (free basic invite with branding) | Desievite free tier, InvitiApp free (10-view cap), Crafty Art free video **[V]**, Evite free with ads | Missing (all invitations paid) | **P0** | **Free image invitation** with a small logo (acquisition). Charge for **video, no logo, RSVP, both family sides, multi-function page, edits**. Keeps us competitive against free tools while the paid value stays clear |
| G9 | **Sender feedback: "Your card was viewed 23 times"** | Punchbowl "Viewed By" **[V]**; Evite RSVP tracking | Missing (views are measured internally only) | **P1** | View count on the manage page + one daily summary email/notification (opt-in). Cheap: we already log recipient views. Brings senders back |
| G10 | **Guest reminders and event updates** (RSVP nudges; "venue changed") | Evite automatic reminders **[V]**; InvitiApp WhatsApp alerts (premium) | Missing | **P1** | Phase 1: event page banner "Updated on …" + host can re-share. Phase 2: opt-in guest reminders by email/web push. WhatsApp only once the API cost is justified |
| G11 | **Co-hosts** (both families or siblings manage the same event and RSVPs) | Evite/Paperless Post co-hosts; Indian family use | Missing | P1 | Share a second "manage" link with co-host rights |
| G12 | **Scheduled sending**, or a reminder to send | Punchbowl, Kudoboard **[V]** | V1 via the WhatsApp API (costly) | P1 | Cheap version first: **"Remind me to send on {date, time}"** by web push or email. True scheduled WhatsApp sends later |
| G13 | **Lifecycle cards**: save-the-date → invitation → thank-you after the event; anniversary reminder next year | Paperless Post / Evite collections; wedding sites | Partial ("Thank you" category exists) | P1 | Link them: after the event date, suggest a thank-you card to RSVP'd guests; next year, an anniversary reminder. A strong retention loop |
| G14 | **Livestream link** on the event page | Common for Indian weddings since 2020 | Missing | P1 | Optional allowlisted link field (YouTube, Google Meet, Zoom) |
| G15 | **Guest photo gallery** after the event | NIMNTRN, InvitiApp **[V]** | Missing | P2 | Later. Adds moderation and storage cost |
| G16 | **Group cards** (many people sign one card: "From all of us") | Kudoboard, GroupGreeting **[V]** | Missing | P2 | Good for the corporate V2 and big families. Not needed for launch |
| G17 | **Guest check-in at the venue** | Paperless Post **[V]** | Missing | P2 | Not needed in India for most events |
| G18 | **Animated "opening" experience** (envelope / doors opening) | Paperless Post, Jacquie Lawson **[V]** | Partial (autoplay video) | P1 | A 1.5s branded opening animation on recipient and event pages (skippable). Adds delight and differentiates from forwarded files |
| G19 | **Bilingual single card** (e.g. Marathi + English on one card) | Common in mixed families | Partial (separate second-language version) | P1 | Templates marked "bilingual" with paired text slots |
| G20 | **WhatsApp-aware video specs** | — | Partial (≤ 16 MB rule) | **P0** | Status supports up to **90s** **[V]**, but invitations should be **≤ 60s**. 9:16, 1080×1920, H.264. **Text safe zones** clear of the Status UI. **Minimum text size** so text survives WhatsApp compression. Offer "Send as document for full HD" as a tip **[V]** |
| G21 | **Customer support model** (FAQ, WhatsApp support, order lookup) | All paid competitors | Missing in the PRD (FAQ in the lean plan only) | **P0** | FAQ in 4 languages; WhatsApp Business number with auto-replies; order lookup by the emailed manage link; reply SLA of 24h (4h on festival days) |
| G22 | **Link and data lifetime policy** | Implicit everywhere | Missing | **P0** | Free cards: links live 12 months. Paid cards: 3 years. Event pages: live until 90 days after the last function, then archived (host can extend; RSVP data deleted after 12 months). Stated on the pricing page. Controls storage cost and meets DPDP retention rules |
| G23 | **Photo consent and acceptable-use terms** (photos of other people, impersonation, fake events) | Standard terms on all platforms | Partial | **P0** | Terms: the uploader confirms they have the right to use the photos; no impersonation or fake events; we may remove content. Short version shown at upload |
| G24 | **Launch KPIs and engagement definitions** | — | Missing in the lean plan | **P0** | See `11-analytics-tracking-plan.md` §6 for the launch KPI set |

---

## 4. Other quality issues found

| # | Issue | Where | Fix |
|---|---|---|---|
| Q1 | Prices differ: PRD ₹19/₹29–49 and invites ₹149/₹399/₹799; lean plan ₹29, ₹99, ₹199/₹399 | `02` §4.2 vs `09` §5 | `09` governs launch; `02` §4.2 becomes "target pricing after launch tests" |
| Q2 | Accounts: PRD needs OTP login; lean plan has none | `02` §5.6 vs `09` §2 | Launch without login (manage link). OTP in P1 |
| Q3 | GST: PRD prices include 18% GST; lean plan assumes no GST registration | `02` §4 vs `09` §7 | Launch without GST (confirm with a CA). Turn on GST-inclusive pricing on registration; **displayed prices stay the same** so there's no visible price jump |
| Q4 | Free-tier economics: free cards cost render + storage + delivery with no revenue | `09` | Free images are cheap (< ₹0.05 each **[E]**). Free *video* is not offered. The link lifetime policy (G22) caps storage |
| Q5 | Targets are assumptions with no baseline | `02` §2 | Keep them as hypotheses. Review weekly against the tracking plan; reset after the first festival |
| Q6 | "Ready-written wishes" replace the AI writer, but there's no content quantity target | `09` §2 | 30 wishes per festival per language (Hindi, Marathi, Tamil, English), with native review |
| Q7 | The music licence is a launch blocker but has no owner or date | `09` §7 | Owner: founder. Due: before template production starts |
| Q8 | The recipient-page "Make your own" loop has no consent-safe attribution rule | `03`, `06` | Attribution uses the card ID in the URL only (no recipient tracking before consent). Defined in `11` §3 |

---

## 5. Launch re-baseline (needed now)

- **Facts:** today is 25 Oct 2026. Diwali (8 Nov) is 14 days away. The build hasn't started. Razorpay KYC status is unknown.
- **Wedding invitations** for the late-November and December muhurats go out **from early November**, after Dev Uthani Ekadashi (~20 Nov **[A: verify]**). That demand is **less time-critical than Diwali, and it pays more**.

**Recommendation: lead with invitations; treat Diwali as a bonus.**

| Option | Go-live | Hero product | When to choose it |
|---|---|---|---|
| **A: Diwali + Weddings** | **Wed 4 Nov 2026** (needs build to start by 26 Oct and Razorpay KYC approved by 2 Nov) | Diwali video greetings + wedding invites | Only if KYC is already submitted and you can test daily |
| **B: Weddings first (recommended)** | **Sat 14 Nov 2026** | Wedding + griha pravesh invitations; Diwali free image greetings during the last days if they're ready; Bhai Dooj/Chhath content optional | Default. Realistic quality, not rushed |

**Decision rule:** if Option A isn't fully tested by 3 Nov, ship Option B scope on 14 Nov. Don't launch an untested payment flow during Diwali week.

---

## 6. Changes applied to the documents

| Document | Change |
|---|---|
| `09-lean-launch-plan.md` | Re-baselined dates; added the P0 items (G1–G5, G7, G8, G20–G24) to the launch scope; pricing updated with a free image invite and both family sides in wedding Premium; launch KPIs reference |
| `02-prd.md` | New §13 "Review changes" listing the accepted gaps and the phase for each; `09` named as the source of truth for launch |
| `06-observability-and-analytics.md` | §4.2 marked as superseded by `11-analytics-tracking-plan.md` |
| `11-analytics-tracking-plan.md` | **New:** a vendor-neutral tracking plan for user engagement |

---

## 7. What's good and should not change
- The **honest-pricing rules** and **automatic refunds**. They're both a differentiator and a legal safeguard.
- The **localisation depth** (typography, Hinglish search, regional festival dates).
- The **recipient page as the growth loop**, now strengthened with the trust measures (G7).
- The **payment architecture** (verified webhooks, idempotency, reconciliation).
- **Launching lean** with managed services, then growing toward the full architecture.

## 8. Open questions for the founder
1. Have you **submitted Razorpay KYC**? This decides Option A vs B.
2. Do you accept **free image invitations** (with our logo) alongside paid video invitations?
3. Do you have contacts who can review a **Marathi and a Tamil wedding invitation** for family-name conventions? This matters more than the UI translation.

---

## Sources
- Evite (RSVP reminders, ads on the free tier): https://www.evite.com/ · https://www.greenvelope.com/blog/how-to-send-an-evite/ · https://fotify.app/blog/best-free-online-invitations-with-rsvp-2026/
- Paperless Post (features, check-in, coins): https://www.paperlesspost.com/blog/paperless-post-product-features/ · https://www.womangettingmarried.com/paperless-post/ · https://www.usecarly.com/blog/paperless-post-pricing/
- Punchbowl (scheduling, Viewed By): https://help.punchbowl.com/article/185-can-i-schedule-a-digital-card-to-be-sent-at-a-future-date · https://help.punchbowl.com/article/532-how-to-send-a-digital-greeting-card-ecard · https://www.punchbowl.com/schedule-cards
- Kudoboard (group cards): https://www.kudoboard.com/cards/farewell-cards/ · https://www.kudoboard.com/
- InvitiApp (free plan limits, WhatsApp RSVP): https://invitiapp.com/en · https://invitiapp.com/en/faq
- NIMNTRN (schedule, gallery, RSVP): https://nimntrn.com/digital-invitation
- Crafty Art (free video invites, print sizes): https://www.craftyartapp.com/ · https://play.google.com/store/apps/details?id=com.crafty.art&hl=en_US
- Wedding invitation APK scams: https://www.tribuneindia.com/news/himachal/invited-to-wedding-police-say-check-the-link-before-you-click · https://the420.in/fake-wedding-invitation-whatsapp-scam-apk-fraud-bhilwara-cybercrime-india/ · https://www.deccanherald.com/technology/new-scam-alert-this-wedding-invitation-takes-over-your-phone-steals-data-and-money-3278051
- WhatsApp Status length and HD sending: https://green-api.com/en/blog/2025/whatsapp-increases-status-length-to-90-seconds/ · https://wafiletools.com/blog/whatsapp-status-video-length-limit/ · https://mobiletrans.wondershare.com/mobile-whatsapp-manage/how-to-upload-hd-whatsapp-status.html
- Earlier competitor sources: see `01-market-research.md`.

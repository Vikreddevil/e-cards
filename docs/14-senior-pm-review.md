# 14 — Senior PM Review: CardReel launch readiness

**Date:** 25 October 2026 · **Scope:** all docs (`01`–`13`), the brand board, and the launch plan as it stands after the founder's decisions (free-first launch, CardReel, Purple & Marigold).
**Question asked:** if a solo founder builds this with Claude starting tomorrow, will it launch, learn and earn? What would a senior PM change before Day 1?

Labels: **[V]** sourced · **[E]** estimate · **[A]** assumption.

---

## 1. Verdict

**The strategy is sound. The plan is not yet buildable, and its calendar is wrong.**

- **Strong:**
  - Research quality, honest commerce, Indian invitation insight (both family sides, the scam-safety stance), localisation depth, payments design, the free-first decision, and a clear brand.
- **Not ready:**
  1. **There's no single, short PRD for what is actually being built.** The product spec is spread across 14 documents and ~46,000 words, and several of them contradict the launch plan.
  2. **The plan doesn't match the real calendar.** Diwali can't be caught. The 2026 wedding season is **much shorter** than assumed, and Phase 1's timing misses most of it.
  3. **There's no Phase 0 go-to-market and no user research.** The launch plan says how to build, not how the first 1,000 users arrive, and no real family touches the product before launch.
  4. **Paid scope is too wide for a first sale.** Four paid products are planned when one would do.

### Scorecard (1–5)

| Area | Score | One-line reason |
|---|---|---|
| Problem and market insight | 4.5 | Evidence-backed; invitations as the revenue engine is right |
| Positioning and differentiation | 3.5 | Clear on paper; must be visible in templates, which aren't designed yet |
| Scope discipline (Phase 0/1) | 2.5 | Free launch is good; Phase 1 still sells 4 things; editor scope is heavy for 14 days |
| Calendar and timing | **1.5** | Diwali missed; wedding muhurats end 12 Dec; Phase 1 date doesn't fit (§3.2) |
| Go-to-market (Phase 0) | **1.5** | None. SEO can't help within the launch window |
| User research and validation | **1** | No interviews, no usability tests, no design partners |
| Metrics and decision rules | 3 | Good spec; KPIs mix phases; switch-on rule uses weak thresholds |
| Pricing | 3.5 | ₹199/₹399 is credible; ₹29 and ₹49 add complexity for little revenue |
| Operations and support | 2.5 | Single founder; 4-hour SLA promise is unrealistic; no launch go/no-go checklist |
| Legal and compliance | 3.5 | Strong base; children's photos and music licence need owners |
| Documentation hygiene | 2 | Stale prices, dates and options across `README`, `02`, `03`, `10`, `11` |

---

## 2. What's strong (keep it)
- **Invitations as the money product and greetings as the free reach engine.** Competitors confirm the willingness to pay for invites (₹149–₹4,999) **[V]**.
- **Indian invitation conventions** (both family sides, relatives list, muhurat, religious header choice). This is a real advantage over generic tools.
- **Honest pricing** (no fake MRP, no subscription traps) and **automatic refunds**. They differentiate us and keep us compliant.
- **Scam-safe sharing** (file first, "no app needed", allowlisted links). It suits an audience that's wary of fake invitation links.
- **The free-first launch.** It removes KYC, GST and entity work from the critical path and buys real learning.
- **The lean stack** (₹6–7k/month) with a clean path to the full architecture later.
- **The CardReel brand:** a neutral name covering card + video, with a distinct logo and an accessible palette.

---

## 3. Findings and recommendations (ranked)

### F1. Write one buildable "Launch PRD" (P0)
**Problem.** What Claude will build is described across `09` (scope), `02` (full PRD), `03` (UX), `04`–`07` (full-scale tech), `10`/`11`/`12`/`13`. Several conflict with the launch plan:
- **README** executive summary points 2, 4 and 5 still describe the old pricing (₹19–₹49, ₹149/₹399/₹799), the Diwali concierge pilot and the Holi public launch.
- **`02` §4.2** prices and OTP accounts differ from `09`.
- **`03`** conversion playbook assumes payments, coupons and badges on day one.
- **`10` §5** "Option A / Option B" dates are superseded by the free launch.
- **`11` §6** KPIs mix Phase 0 and Phase 1 (checkout and payment KPIs before payments exist).

**Recommendation.** Create **`docs/15-launch-prd.md`**, a 4–6 page spec that is the **only** input for the build:
- users;
- the three jobs to be done;
- the Phase 0 screens and flows;
- the invitation content model (fields per community);
- the template list;
- acceptance criteria;
- Phase 0 metrics;
- a go/no-go checklist.

Mark `02`–`08` as **"Vision and reference"** and fix the stale lines listed above.

### F2. Re-plan around the real calendar (P0)
**Facts** (dates vary slightly by panchang):
- **Diwali 8 Nov; Bhai Dooj 10–11 Nov; Chhath 13–16 Nov (main day 15 Nov); Dev Uthani Ekadashi 20–21 Nov; Guru Nanak Jayanti 24 Nov** **[V]**.
- **Wedding muhurats 2026:** 21, 24, 25, 26 Nov and about 2–6, 11, 12 Dec. **Then none until 15 Jan 2027 (Kharmas)** **[V]**.
- Invitations go out roughly **2–4 weeks before** the wedding **[A]**.

**What this means:**
- Build starting **26 Oct** + 14 days = Phase 0 live **about 8 Nov, on Diwali day itself**. **Diwali greeting demand is lost.** Bhai Dooj and Chhath are the only near festivals left.
- Invites for the **21–26 Nov** weddings are already being sent **before** we launch. Invites for the **2–12 Dec** weddings go out roughly **10–25 Nov**, which is our first two weeks.
- After 12 Dec, **there are no weddings until mid-January**. Invites for the **Jan–Feb 2027** wave start going out in **late December**.
- The current Phase 1 target (**~20 Nov**) leaves only ~12 days of data before switching on payments, and still catches only the tail of the December wave.

**Recommendation.** Treat the next six weeks as **learning and testimonials**, and earn from the **January–February wave**:

| Window | Focus | Content |
|---|---|---|
| 8 Nov – 12 Dec | **Phase 0 free + design partners** (F4) for December weddings; Bhai Dooj, Chhath, birthdays, griha pravesh | Wedding and engagement invites (4 languages), Bhai Dooj, Chhath (Hindi), birthdays |
| ~1–14 Dec | **Build and test Phase 1** (payments in test mode; KYC approved) | Premium invitation features |
| **~15 Dec** | **Switch on premium invitations** (switch-on rule F7) | Jan–Feb 2027 wedding invites |
| 15 Dec – 15 Jan | Greetings for **Christmas, New Year, Lohri / Makar Sankranti / Pongal (13–15 Jan)**, plus wedding invites for Jan–Feb | SEO pages for Sankranti/Pongal published **by early Dec** |

The founder can still choose to rush payments for the 2–12 Dec weddings. **I'd advise against it:** it cuts testing and data to under two weeks for a payment flow, during a short window.

### F3. Sell one thing first (P0)
**Problem.** Phase 1 lists four paid products: ₹29 HD greeting, ₹49 no-logo image invite, ₹199 Standard and ₹399 Premium video invite. Each adds checkout paths, edge cases, support and refund rules. The ₹29 and ₹49 items would bring little revenue and **remove the free-output branding that drives growth**.

**Recommendation.** Phase 1 sells **only premium invitations**:
- **₹399 Premium video invitation** as the hero (both family sides, multi-function event page + RSVP, no branding, HD + PDF, 14-day edits);
- **₹199 Standard** as the lower option.

Keep greetings and image invites **free with branding**. Revisit ₹29 HD greetings only once there's demand data (e.g. "Remove logo" taps).

### F4. Put real families in the loop before launch (P0)
**Problem.** No interviews, usability tests or pilot users are planned. The invitation editor (family names, relatives, functions, muhurat, two sides) is the most complex screen we have, and it's culturally sensitive.

**Recommendation:**
1. **Design partners (start this week):** recruit **8–12 families** with weddings, engagements or griha pravesh between 21 Nov and mid-Feb, through the founder's network and WhatsApp groups.
   - **Offer:** premium free, plus hand-holding.
   - **Ask:** a 20-minute call, permission to use their (anonymised) invite as a showcase, and a testimonial.
   - Aim for **3 Hindi, 3 Marathi, 2 Tamil** and 1–2 Muslim, Christian or Sikh families, to test the header and content model.
2. **Usability tests:** **5 sessions** on a low-end Android phone inside WhatsApp, at the end of build week 1. Task: "make a wedding invite for your cousin and send it to the family group".
   - **Success bar:** ≥ 4/5 finish in under 6 minutes without help.
3. **Show it on the site:** "Made by real families" gallery (with consent) as launch social proof.

### F5. Add a Phase 0 go-to-market (P0)
**Problem.** The lean plan has no acquisition plan beyond "soft launch to family and friends". SEO takes months; it won't move numbers before mid-December.

**Recommendation: a 30-day plan for one person, ₹10–20k budget:**

| Channel | Action | Weekly target **[A]** |
|---|---|---|
| Founder network + WhatsApp groups | Personal sends of real invites and greetings; ask design partners to share | 300–500 visitors |
| **Wedding-vendor partners** | 10–15 local mehendi artists, decorators, caterers, pandits and photographers get free premium invites for their clients in exchange for sharing CardReel | 5–10 invitation creators per partner |
| Instagram / YouTube Shorts | 3 Reels a week: "Marathi wedding invite in 60 seconds", "both family sides in one tap" | 1–2k views |
| Local Facebook and WhatsApp communities | City, society and community groups; value posts, not spam | 100–300 visitors |
| Paid boosts (from week 2) | ₹2.5–5k a week on the best-performing Reel, in Hindi and Marathi | Cost per invitation creator ≤ ₹40 |
| SEO (for January) | Publish Lohri/Sankranti/Pongal and Jan–Feb wedding pages by early Dec | Indexing ready for January |

### F6. Templates are the product, so set a quality bar (P0)
**Problem.** Templates built as code by Claude are the plan. For an emotional, once-in-a-lifetime purchase, design quality decides conversion. There's no benchmark or budget for quality yet.

**Recommendation:**
- **Quality bar:** before launch, put **each wedding template side by side with 3 competitor templates** (WedMeGood, Desievite, Crafty Art). Ship only those that a design partner says they would actually send.
- **Budget ₹10–20k** to buy or commission **3–5 hero wedding templates** (one per language style), as backgrounds and motion assets with licences that allow redistribution. Claude builds the slot system around them.
- **Launch catalogue minimum:**
  - 8 wedding/engagement video invites (2 per language style);
  - 6 image invites;
  - 4 griha pravesh invites;
  - 6 greetings (Bhai Dooj, Chhath, birthday);
  - 30 ready-written wishes per language for these occasions.

### F7. Fix the metrics and the switch-on rule (P1)
**Problems:**
1. **Weak thresholds.** "≥ 500 shares/week" and "≥ 3% notify-me" are arbitrary and percentage-based on small numbers.
2. **The denominator is undefined.** It should be "invitation creators who **saw** the premium teaser", not all creators.
3. **Consent bias.** Before analytics consent we only collect anonymous essentials (`11` §7.6). If many users decline, decision metrics will be biased.
4. **Mixed KPI table.** `11` §6 mixes Phase 0 and Phase 1 KPIs.

**Recommendations:**
- **Phase 0 scorecard** (decision date **~10 Dec**):

  | Metric | Target |
  |---|---|
  | Invitation creators | ≥ 300 |
  | Creation completion | ≥ 55% |
  | Share rate | ≥ 70% |
  | Waitlist sign-ups | **≥ 60 absolute**, at ≥ 5% of teaser viewers |
  | Design-partner NPS | ≥ 40 |
  | Render failure | ≤ 1% |
  | Recipient → creator | ≥ 2% |

- **Switch-on rule:** KYC approved **and** waitlist ≥ 60 **and** 5 design-partner families say they would have paid **₹199 or more**. This is an absolute-number rule.
- **Consent-free counts:** keep **server-side aggregate counters** for renders, shares (share-button pings), invitation completions and waitlist sign-ups. They are anonymous and need no consent, so the decision metrics stay unbiased.
- **Split `11` §6** into a Phase 0 table and a Phase 1 table.

### F8. Make the free tier and premium boundary clear (P1)
- **Leak:** a host can make two free basic invites (one per family side), so "both sides" alone won't sell premium. The premium **core** must be what free can't do:
  - multi-function event page with RSVP and guest count;
  - multiple photos;
  - no branding;
  - HD + PDF;
  - the edit window.

  Show "Both sides in one tap" as a convenience, not as the main reason to pay.
- **Branding:** keep the end card short (2s) and tasteful. Wedding hosts may resent heavy branding, and that is the moment we offer premium.

### F9. Founder capacity, support and a go/no-go checklist (P1)
- **Founder time budget:** testing, native review co-ordination, content QA, partners, Reels and support come to roughly **25–30 hours a week** in launch weeks **[E]**. Plan it explicitly, or cut channels.
- **Support promise:** change "4h on festival days" to **"within 24 hours"**. Add a **"Send to myself on WhatsApp"** button for the manage link (users lose emails; they don't lose their own chat).
- **Go/no-go checklist for Phase 0 (all must pass):**
  - every policy page live;
  - native review signed off for every live template;
  - ≥ 99% render success on 200 test renders;
  - recipient page LCP ≤ 2.5s on a low-end Android inside WhatsApp;
  - analytics events firing (`11` §7.8);
  - Turnstile and render caps on;
  - support FAQ live;
  - zero known bugs that block creating or sharing.

### F10. Close the compliance owners (P1)
- **Children's photos:** naming-ceremony, birthday and baby-shower cards will contain **children's photos uploaded by parents**. Add an upload notice and consent line (parent/guardian), keep the photos private (noindex, unguessable links, deletion), and confirm the DPDP position with counsel.
- **Music:** name the source (a licence pack or 5 commissioned tracks) and a due date (**before template build, ~Day 9**). Without music, video templates can't ship.
- **WhatsApp follow-ups:** in Phase 0, contact waitlist users by **email only**, or by WhatsApp **only after explicit opt-in**, sent manually. Automated WhatsApp messages need the Business API and approved templates (Phase 1+).

### F11. Smaller notes (P2)
- **Language UI:** keep all 4 languages, but launch the **Tamil UI only once the Tamil review is complete**. Tamil templates can launch with an English UI if needed. This decouples the critical path.
- **The "Notify me" teaser** should show the real premium price, so the waitlist measures willingness to pay at that price.
- **Brand:** buy `cardreel.in` today (it's ₹1, but available names disappear) and reserve handles before any public post.
- **Analytics:** add `ui_click(element_id=remove_logo_interest)` so we learn whether ₹29 HD greetings are worth building.

---

## 4. Decisions needed from the founder

| # | Decision | Recommendation |
|---|---|---|
| 1 | Write the single **Launch PRD** (`15`) and mark `02`–`08` as reference | **Yes** |
| 2 | Switch on paid invitations for the **2–12 Dec weddings** (rushed), or for the **Jan–Feb wave (~15 Dec)** | **~15 Dec** |
| 3 | Phase 1 sells **only premium invitations** (₹199/₹399); greetings and image invites stay free | **Yes** |
| 4 | Recruit **8–12 design-partner families** this week | **Yes** |
| 5 | Budget **₹10–20k** for 3–5 hero wedding templates and **₹10–20k** for boosts | **Yes** |
| 6 | New switch-on rule: absolute counts (waitlist ≥ 60, 5 "would pay" families) + KYC | **Yes** |

---

## 5. Re-baselined launch calendar (if the decisions above are accepted)

> **Superseded:** the founder set the timeline instead: staging in 2–3 days, a testing week, a free production week, then payments. See `15-release-plan.md`. Payments now open around **14 Nov**, for the **2–12 Dec** weddings, rather than 15 Dec.

| Date (2026) | Milestone |
|---|---|
| 25–27 Oct | Buy `cardreel.in`; reserve handles; start design-partner outreach; source music |
| **26 Oct** | Build Day 1 (site + policy pages live; KYC submitted once pages are up) |
| ~1 Nov | 5 usability tests on the invitation editor |
| ~6 Nov | Go/no-go review (§F9 checklist) |
| **~8 Nov** | **Phase 0 live** (free); Bhai Dooj and Chhath greetings; wedding invites for December |
| 8 Nov – 10 Dec | Phase 0 GTM (F5); design partners using it; Phase 1 built in test mode from ~16 Nov |
| ~10 Dec | Phase 0 scorecard review; switch-on decision |
| **~15 Dec** | **Phase 1: premium invitations on sale** for Jan–Feb weddings |
| by 5 Dec | SEO pages for Lohri / Makar Sankranti / Pongal (13–15 Jan 2027) and Jan–Feb wedding invites |

---

## Sources
- Festival dates 2026: [Bhai Dooj 2026 (Samvat)](https://samvat.in/festivals/bhai-dooj-2026/) · [Dev Uthani Ekadashi 2026](https://www.divinehindu.in/blogs/news/dev-uthani-ekadashi-2026) · [Holidays in November 2026](https://www.borntobanger.in/blog/holidays-in-november-2026-india) · [Hindu festival calendar 2026](https://www.devaastha.com/blogs/festivals/hindu-festival-vrat-calendar-2026)
- Wedding muhurats Nov–Dec 2026 and Kharmas: [Shubh Vivah Muhurat Nov–Dec 2026](https://dreamweddinghub.com/blog/shubh-vivah-muhurat-november-december-2026) · [Vivah Muhurat Nov–Dec 2026](https://vjvnow.com/blogs/vjv-now/vivah-muhurat-november-december-2026-wedding-dates) · [Hindu wedding dates 2026](https://www.indianweddingplanners.in/Blogs/Auspicious-Hindu-Wedding-Dates-2025-2026) · [Wedding muhurat Nov 2026 – Dec 2027](https://purohit.app/blog/wedding-muhurat-dates-2026-2027)
- Competitor pricing and features: see `01-market-research.md` and `10-prd-review.md`.

# 01 — Market and Competitor Research

**Status:** Draft v1 · **As of:** 2 October 2026 · **Owner:** Product

## How to read the numbers

Every figure carries one of these labels:

| Label | Meaning |
|---|---|
| **[V]** | Reported by a cited public source (news report, company release, store listing, regulator). Not independently audited. |
| **[E]** | Our estimate, with the logic shown. |
| **[A]** | Assumption that must be validated (see the validation plan in `02-prd.md` §3). |

**Research method.** Web search across company releases, financial reporting (Entrackr, Forbes India, YourStory), regulator notices (PIB, NPCI, MeitY, CCPA), app-store listings and competitor pricing pages. Direct page fetches were blocked in the research environment, so some figures come from search-result summaries of those pages. Sources are listed at the end. Anything that drives a pricing or investment decision should be re-checked on the live page before you commit money.

---

## 1. Executive summary

1. **The category is proven, and Bharat pays.** Crafto (personalised greetings and "status" images, made by Kutumb/Primetrace) grew revenue **173% to ₹128.6 crore in FY25**, mostly from subscriptions, and turned profitable **[V]**. By February 2026 its parent reported a **₹550 crore annual revenue run-rate** **[V]**. "Indians won't pay for greetings" is wrong. The correct version is: *Indians pay for greetings that carry their own photo and name, are easy to make, and can be paid for by UPI.*
2. **The market splits into three jobs with very different willingness to pay:**
   - **Everyday and festival greetings** (high volume, ₹ low per unit). Crafto dominates this, with heavy ad spend: 62% of its costs **[V]**.
   - **Event invitations** (weddings, griha pravesh, birthdays, pujas). This has the **highest price per unit**: ₹70 to ₹4,999 per invite **[V]**. It is fragmented, sold year-round, and paid for once per event, with no subscription.
   - **Small-business festival posters** (shop owners branding greetings with their logo and phone number): ₹99–₹199/month subscriptions **[V]**, with 7.8 crore Udyam-registered enterprises **[V]**.
3. **The old e-card sites are weak.** 123Greetings, Dgreetings and 365Greetings are ad-supported and dated, and interest in them is falling **[V]**. Their SEO footprint is still useful to study.
4. **Trust is an open gap.** Crafto users publicly complain about hidden UPI Autopay, short renewals and a strict no-refund policy **[V]**. India's CCPA now names *subscription traps*, *false urgency* and *drip pricing* as illegal dark patterns **[V]**. A brand built on **honest prices, cancel-anytime, and refund-if-it-fails** stands out and is easier to defend legally.
5. **Recommended positioning:**
   > *"Beautiful cards and invitations in your language, with your photo and name, ready to share on WhatsApp in a minute. Pay once, only for what you love. If it fails, your money is back automatically."*

   The wedge has three parts:
   - **Free festival greetings** drive acquisition and virality.
   - **Paid premium video greetings and event invitations** drive revenue.
   - **A business plan** follows in V1.

---

## 2. Market context: India in 2026

| Indicator | Value | Label | Why it matters |
|---|---|---|---|
| Internet users | 950M+ (2025) | [V] IAMAI–Kantar | The addressable base is huge. |
| Rural share of active internet users | ~57% | [V] IAMAI–Kantar | Bharat is the majority; design for it. |
| Users who consume Indic-language content | ~98% | [V] IAMAI–Kantar | Language is the default, not a feature. |
| Urban users who prefer regional languages | ~57% | [V] IAMAI–Kantar | Metro users also want vernacular. |
| WhatsApp users in India | ~536M | [V] (secondary stats aggregator) | WhatsApp is the distribution channel. |
| Smartphone users | ~650–690M | [V] (secondary) | Mobile-first is mandatory. |
| Android share of smartphones | ~92% | [V] IMARC | Optimise for Android Chrome and the WhatsApp in-app browser. |
| Low-end segment ($100–200) | ~30% of devices | [V] IMARC | Performance budget matters. |
| UPI volume, Aug 2026 | 24.51B transactions/month, avg ₹1,217 | [V] NPCI via Business Standard | UPI is the primary way people pay; ₹19–₹999 purchases feel normal. |
| Weddings, Nov–Dec 2025 season | ~46 lakh weddings, ₹6.5 lakh crore spend | [V] CAIT | A big, high-intent invitation market. |
| Udyam-registered enterprises | 7.83 crore (Feb 2026) | [V] PIB | The pool for the business plan. |

**Language coverage at launch (Census 2011 first-language speakers):**

| Language | Speakers | Share of population |
|---|---|---|
| Hindi | 52.8 crore | 43.6% |
| Marathi | 8.3 crore | 6.9% |
| Tamil | 6.9 crore | 5.7% |
| English | Mostly a second language; ~13 crore total speakers | — |

Hindi + Marathi + Tamil first-language speakers are about **56% of the population** **[V]**. English covers urban and cross-language users. **Telugu, Bengali, Gujarati and Kannada** are the natural next languages.

---

## 3. Competitor study

### 3.1 Who we studied, and why

| Cluster | Products | Why studied |
|---|---|---|
| Personalised greetings for Bharat | **Crafto** (Kutumb/Primetrace) | Proves willingness to pay; sets the UX bar for photo and name personalisation. |
| Business festival posters | **Brands.live**, **Festival Poster Maker**, **Graphicwaale**, Brand Post apps | Pricing for small-business subscriptions. |
| Indian invitations, including video | **Desievite**, **WedMeGood e-invites**, **VideoGiri**, **Selfanimate**, **WishNWed**, **VideoInvites.net** | The highest willingness to pay per unit. |
| Legacy e-card sites | **123Greetings**, **Dgreetings**, **365Greetings** | SEO incumbents; lessons in what not to do (ads). |
| Horizontal design tools | **Canva** (India) | The quality benchmark; the threat from above. |
| Global paid e-card benchmarks | **Jacquie Lawson**, **Punchbowl**, **123Greetings Pro**, Greetings Island | Paid models for e-cards. Used for template quality and model design, **not** as evidence of Indian willingness to pay. |
| Commerce UX references | **Meesho**, **Flipkart**, **Myntra**, **Swiggy/Zomato** | Discount display, badges and social proof patterns Indian users already trust. |

### 3.2 Competitor profiles

#### Crafto (Kutumb / Primetrace): the category leader in personalised greetings
- **What it is:** an Android-first app to make daily "status" images, quotes, shayari, and festival and Good Morning greetings with **your own photo and name**, in many Indian languages. Now positioned as an AI content-creation app.
- **Scale:**
  - Play Store: ~120M downloads, ~4M a month, 4.7★ from ~1.06M reviews **[V]** (store-listing aggregators).
  - The parent company claims "200 million users" for Crafto AI **[V]**.
- **Money:**
  - FY25 revenue was **₹128.6 crore** (+173% year on year), driven mainly by Crafto subscriptions. Net profit was ₹12 crore **[V]**.
  - **Ads and marketing were 62% of total costs**: ₹84.6 crore, up 2.8x **[V]**.
  - Primetrace's run-rate was ₹550 crore ARR and ₹200 crore EBITDA as of February 2026 **[V]**.
- **Model:** Monthly, quarterly and annual subscriptions through **UPI Autopay** **[V]**. Renewals can be charged up to 96 hours before expiry **[V]**. Strict no-refund policy **[V]**.
- **Strengths:**
  - Photo + name personalisation.
  - Vernacular-first.
  - A huge template catalogue.
  - Daily habit content ("Good Morning").
  - Paid acquisition at scale.
- **Weaknesses and gaps:**
  - Trust complaints: "hidden autopay", refunds refused **[V]**.
  - App-only, so web links and SEO are weak.
  - Built on subscriptions; no single-purchase option for occasional users.
  - Not built for event invitations or a recipient experience.
- **Lesson:** Copy the personalisation depth and the daily-use loop. Do **not** copy the subscription-first, no-refund model.

#### Brands.live / Festival Poster Maker / Graphicwaale: small-business festival posters
- **What they are:** apps that put a shop's logo, name, phone number and address on festival and daily marketing posters, for sharing on WhatsApp, Instagram and Facebook.
- **Pricing:**
  - Brands.live: **₹199/month, ₹499/quarter**, with a 7-day free trial **[V]**.
  - Graphicwaale: **₹99/year** for unlimited, watermark-free HD **[V]**.
  - Festival Poster Maker: claims 500+ business categories, 3,000+ festival categories, and "10 lakh+" posts **[V]**.
- **Strengths:** a clear job ("my shop's Diwali post, ready daily"), a cheap annual price, and a huge catalogue.
- **Weaknesses:** design quality is commoditised and there is a race to the bottom on price (₹99/year). They are app-only and English-heavy in their UI.
- **Lesson:** Business branding is a real, sticky segment, but the price ceiling is low. Treat it as a **V1 add-on** (a branding layer on our existing templates), not as the wedge.

#### Indian invitation platforms (Desievite, WedMeGood, VideoGiri, Selfanimate, WishNWed, VideoInvites.net)

| Product | Price range (₹) | Notable patterns | Label |
|---|---|---|---|
| Desievite | Static from ₹70, animated from ₹99; premium ₹149–₹499 | Free with watermark; pay to remove it. Hindi and regional templates. **RSVP tracking.** Auto-fitting templates. | [V] |
| WedMeGood e-invites | ₹500–₹4,999 (most ₹1,300–₹4,949); "on sale" ₹500–₹2,399, "up to 50% off" | MRP strike-through. Filters by culture, theme and price. "Share link on WhatsApp". Part of a wedding marketplace. | [V] |
| VideoGiri | ₹999–₹3,999 by design complexity | Premium wedding video focus. | [V] |
| Selfanimate | Puja/ceremony from ₹199; wedding from ₹499 | Ceremony-specific templates. | [V] |
| WishNWed | From ₹299; HD 1080p, no watermark, delivered in under 5 minutes | **"See your invite. Then pay."** Preview before paying. | [V] |
| VideoInvites.net | ₹149–₹799 | Tiered catalogue. | [V] |

- **Strengths:** high intent, high price per unit, year-round demand (wedding seasons, birthdays, pujas, griha pravesh), and an emotional purchase where quality matters.
- **Weaknesses:**
  - The market is fragmented, with many small players.
  - The "premium" tier is usually done by hand with slow turnaround.
  - Limited Marathi and Tamil-first premium design **[A]**: we need to verify this by counting the language catalogue of the top 5 players.
  - RSVP and guest experience are basic.
  - Most are not set up for festival greetings, so there's no reason to come back between events.
- **Lesson:** Invitations are the **revenue engine**. Add "preview free, pay to unlock HD without watermark" (Desievite/WishNWed), honest MRP anchoring (WedMeGood), RSVP, and **fast automated rendering** (WishNWed's "under 5 minutes").

#### 123Greetings / Dgreetings / 365Greetings: legacy e-card sites
- **Model:** free and ad-supported. 123Greetings removed pop-up ads after user complaints. Ads show to **both sender and recipient**, which spoils the moment. 123Greetings Pro removes ads for **$5.99/year** **[V]**.
- **Trend:** search interest is far below its 2000s–2010s peak **[V]**.
- **Strengths:** decades of SEO ("Diwali ecards", "Hindi greeting cards"), a broad occasion taxonomy, and Hindi sections.
- **Weaknesses:** desktop-era UX, weak photo and name personalisation, poor WhatsApp-native sharing, and ads on the recipient's page.
- **Lesson:** The festival and occasion taxonomy and landing-page SEO still work. **Never show ads on the recipient's page.**

#### Canva (India): the horizontal design tool
- **Scale:** India is Canva's **4th-largest market**. The user base doubled in a year, Indian users have made 2.8 billion+ designs, and the company says it wants India to be its #1 market **[V]**. Globally: 265M MAU and 31M paid users (end of 2025) **[V]**.
- **Pricing (India):** Pro at **₹499/month or ₹3,999/year** plus 18% GST. Pro Lite ₹1,590/year **[V]**.
- **Strengths:** best-in-class editor, huge template library, many wedding and festival templates, Hindi interface.
- **Weaknesses for our users:**
  - Too complex for a 50-year-old making a Diwali wish in 60 seconds.
  - Indic typography inside templates is uneven **[A]**.
  - Not built around festival moments, WhatsApp sharing, or a recipient page.
  - Annual pricing is too high for an occasional sender.
- **Lesson:** Don't compete on editor power. Compete on **speed to a finished, beautiful, in-language card** (fill in 2–3 fields, done). Canva is also our **biggest copycat risk** (see Risks in `08-roadmap-team-cost-gtm.md`).

#### Global benchmarks: Jacquie Lawson, Punchbowl, Greetings Island
- **Jacquie Lawson:** subscription only, **$36/year** or $8/month, unlimited animated cards, premium art quality **[V]**.
- **Punchbowl:** freemium; paid tiers **$2.99–$5.99/month** by send volume **[V]**.
- **Greetings Island:** free cards; premium membership for the best designs and downloads **[V]**.
- **Lesson:** Animated cards of very high quality can carry a subscription in mature markets. In India, the price point has to be **5–10x lower** and offered per card first. These products are references for **template quality and the recipient's opening experience** (envelope animation, music), not for pricing.

#### Commerce UX references (Meesho, Flipkart, Myntra, Swiggy/Zomato)
- Patterns Indian users understand instantly:
  - MRP with a strike-through and "% off" in green.
  - "Bestseller" and "Trending" ribbons.
  - "X people bought this".
  - Coupons applied automatically at checkout.
  - Price ladders where the middle option is "Most popular".
  - "Free delivery" style thresholds.
- **Regulatory caution:** these patterns are legal only when **truthful**. See the dark-pattern mapping in `03-ux-and-conversion.md` §4.

### 3.3 Comparison table

| Dimension | Crafto | Business poster apps | Invitation platforms | Legacy e-card sites | Canva | **Our target** |
|---|---|---|---|---|---|---|
| Core job | Daily/festival status with my photo | Shop's festival marketing post | Event invitation (often video) | Send a free e-card | Design anything | **Festival greeting + event invitation, in my language, in 60s** |
| Primary user | Bharat, 30–60, vernacular | Micro-business owners | Families hosting events | Older web users, NRIs | Creators, SMBs, students | **Bharat + urban families; hosts; (V1) micro-business** |
| Platform | Android app | Android app | Web + app | Web | Web + app | **Mobile web/PWA first; Android wrapper in V1** |
| Languages | Many Indic | Mostly Hindi/English | Hindi + some regional | English/Hindi | Hindi + 100 others in UI | **Hindi, English, Marathi, Tamil, with native typography** |
| Photo + name personalisation | Excellent | Logo, name, phone | Names, dates, photos | Weak | Full editor | **Excellent, guided slots** |
| Video | Some | Some | Core | Animated (Flash-era) | Yes (complex) | **Core** |
| Monetisation | Subscription (UPI Autopay) | Subscription ₹99/yr–₹199/mo | Per invite ₹70–₹4,999 | Ads; Pro $5.99/yr | Subscription ₹3,999/yr | **Pay per card/invite first; optional pass in V1; business plan in V1** |
| Refund trust | Weak (no refunds) | Unknown | Varies | n/a | Standard | **Automatic refund on failure; clear policy** |
| Recipient experience | Image forwarded | Image forwarded | Link + RSVP (some) | Link with ads | Link/download | **Fast, ad-free recipient page + "make your own"** |
| SEO | Weak (app) | Weak | Medium | Strong (legacy) | Strong | **Strong in 4 languages** |

---

## 4. Willingness to pay: what Indians actually pay for here

| Use | Evidence | Typical price | WTP verdict |
|---|---|---|---|
| Wedding invitation video | WedMeGood ₹500–₹4,999; VideoGiri ₹999–₹3,999; WishNWed ₹299+ **[V]** | **₹299–₹1,499** sweet spot **[E]** | **High.** Once per event, emotional, visible to hundreds of guests. |
| Other ceremony invites (griha pravesh, puja, naming, birthday) | Selfanimate puja ₹199; Desievite ₹99–₹499 **[V]** | **₹99–₹299** **[E]** | **Medium–high.** Frequent across a family's year. |
| Personalised greetings with own photo, as a habit | Crafto subscription revenue ₹128.6 crore FY25 **[V]** | Subscriptions; per-card sachets untested **[A]** | **Medium** when it's a habit and very easy. A per-card "sachet" (₹19–₹49) is our hypothesis to test. |
| One-off festival greeting, no personalisation | Ubiquitous free content (WhatsApp forwards, free apps) | ₹0 | **Low.** Keep it free; it's the acquisition hook. |
| Small-business festival branding | Brands.live ₹199/mo, ₹499/qtr; Graphicwaale ₹99/yr **[V]** | ₹99–₹999/year **[E]** | **Medium**, but with commodity price pressure. |
| Corporate/HR greetings | No strong Indian benchmark found | ₹5,000–₹50,000/year per company **[A]** | Unknown; V2. |
| Subscriptions in Bharat (analogy) | Kuku FM: ~11% download-to-paid conversion, ARPU ~₹600, plans ₹199/mo to ₹1,499/yr **[V]** (GrowthX analysis) | — | Shows that Tier 2+ users pay for subscriptions when the habit is strong and UPI Autopay is smooth. |

**Takeaways**
1. **Keep festival greetings free** to win distribution. Charge for **personalisation depth and quality**: HD video, own photo, no watermark, premium art.
2. **Make invitations the revenue engine.** Price per event, in tiers, with a free preview.
3. **Start with per-item ("sachet") purchases.** Add an *optional* pass in V1 only after repeat purchase is proven. The pass must be easy to cancel and auto-renew only with clear consent, so we don't repeat Crafto's trust problem.

---

## 5. Market sizing (₹)

All of this is estimated **[E]** from the figures above. Ranges reflect uncertainty.

### 5.1 TAM: every Indian digital-greeting and invitation purchase we could serve

| Segment | Logic | Annual TAM |
|---|---|---|
| A. Personal greetings (festival, daily, birthday) | ~200M heavy greeting senders (≈40% of ~536M WhatsApp users) **[A]** × 2–4% paying **[A]** × ₹300–₹600/yr **[E]** | **₹1,200–₹4,800 cr** |
| B. Wedding invitations | ~1.0–1.2 crore weddings/yr **[E]** (46 lakh in a single 6-week season **[V]**) × 1.5 paid invites per wedding (both families) **[A]** × 40% buy a paid digital invite **[A]** × ₹500 avg **[E]** | **₹300–₹360 cr** |
| C. Other event invitations | ~5 crore hosted events/yr (birthdays, griha pravesh, pujas, naming, engagements, anniversaries) **[A]** × 10% buy paid **[A]** × ₹150 **[E]** | **~₹750 cr** |
| D. Small-business festival branding | ~5 crore micro-enterprises **[V/E]** × 30% market on WhatsApp **[A]** × 5% pay **[A]** × ₹600/yr **[E]** | **~₹450 cr** |
| **Total TAM** | | **≈ ₹2,700–₹6,400 cr/year** |

**Sanity check:** a single player (Primetrace) already has a ₹550 crore run-rate **[V]**, mostly in segment A. So the total market is at least several thousand crore. ✔︎

### 5.2 SAM: what we can serve with 4 languages on mobile web

- **Languages:** Hindi, Marathi, Tamil and English cover ~55–65% of demand **[E]**. This weights toward the Hindi belt and Maharashtra, which are rich in weddings and festivals.
- **Channel:** web and PWA, with no Play Store presence until V1. Assume 70–80% reach **[A]**.
- **SAM ≈ ₹1,100–₹3,300 cr/year.**

### 5.3 SOM: realistic 3-year capture

| | Year 1 (FY27-28) | Year 3 |
|---|---|---|
| Share of SAM | 0.1–0.2% | 1–2% |
| Revenue | **₹1.5–₹5 cr** | **₹15–₹50 cr ARR** |
| Mix | 70% invitations, 30% greetings | 50% invitations, 30% greetings, 20% business plans |

**Note:** Invitations are a smaller TAM than greetings, but they let a new brand **win share faster**. Prices are higher, the market is fragmented with no Crafto-sized leader, quality wins, and you don't need habit-level retention to make money.

---

## 6. Personas

### P1. "Sunita Aunty": the daily wisher (Bharat, 45+)
- **Profile:** 52, homemaker, Indore. Hindi. ₹10k Android phone; PhonePe. In 15 WhatsApp groups (family, kitty party, society).
- **Behaviour:** Sends Good Morning and festival wishes every day. Loves seeing **her photo and name** on the card ("from Sunita Sharma and family"). Forwards whatever looks *bhavya* (grand).
- **Needs:** Hindi interface and big tap targets; no typing in English; results in under 60 seconds; works on a slow 4G connection.
- **Will pay:** ₹19–₹49 for a "special" Diwali or anniversary card. Wary of autopay after a bad experience.
- **Wins us:** Hindi-first home screen, photo slot, a "Make it yours in 3 taps" flow, and "no autopay, pay once".

### P2. "Rahul & Priya": the couple getting married (urban, 25–32)
- **Profile:** Pune. Marathi and English. iPhone and Android. GPay/cards.
- **Behaviour:** Want a stylish **video invitation in Marathi and English**, plus separate invites for haldi, mehendi and sangeet. Compare WedMeGood, Instagram designers and Canva. Parents must approve.
- **Needs:** premium design, both languages, family names in the correct order, quick edits after "Papa's feedback", RSVP for out-of-town guests, a link that looks good on WhatsApp.
- **Will pay:** ₹499–₹1,999 for the main invite plus add-ons.
- **Wins us:** a Marathi-first premium catalogue, a free watermarked preview to share with parents, unlimited edits for 30 days, and RSVP.

### P3. "Karthik": the quality-conscious professional (metro, 30–40)
- **Profile:** Chennai, IT. Tamil and English. Laptop at work, Android at home.
- **Behaviour:** Sends Pongal, Puthandu and Deepavali wishes to family and colleagues; also LinkedIn and Instagram stories. Dislikes kitschy designs.
- **Needs:** tasteful Tamil typography (not machine-looking), and short animated cards with tasteful music.
- **Will pay:** ₹29–₹99 per premium animated card. Might buy a ₹299/year pass if the catalogue stays fresh.
- **Wins us:** a design-led Tamil collection, desktop editing, and multiple export formats (Status 9:16, square, landscape).

### P4. "Ramesh Bhai": the shopkeeper (micro-business, 35–55) — V1 persona
- **Profile:** Nashik, mobile and accessories shop. Marathi and Hindi. WhatsApp Business with ~800 customers.
- **Behaviour:** Posts festival greetings with his shop logo and phone number on Status and in broadcasts. Currently uses a poster app.
- **Needs:** auto-branding on every card, a daily post ready by 7am, no design effort, and a GST invoice.
- **Will pay:** ₹99–₹999/year.
- **Wins us:** our premium festival catalogue with a one-time brand setup, plus a "Today's post" daily push.

### P5. "Anjali": the student (Gen Z, 18–24)
- **Profile:** Lucknow. Hinglish. Instagram-first. UPI only, ₹50 spending limit.
- **Behaviour:** Birthday wishes for friends, Reels and Stories, funny or aesthetic. Searches in Hinglish ("bday wish for bestie").
- **Needs:** trendy templates, 9:16 formats, quick music cards, Hinglish search.
- **Will pay:** ₹19 impulse purchases; rewards for sharing.
- **Wins us:** trendy sub-collections, "unlock by sharing", and the speed of a Reels-style editor.

### P6. "Meena": the HR manager (corporate, 30–45) — V2 persona
- **Profile:** Bengaluru, 200-employee company.
- **Behaviour:** Diwali, work anniversary and birthday cards for employees; wants the company brand and bulk personalisation from a CSV file.
- **Will pay:** ₹5,000–₹50,000/year with a GST invoice.
- **Wins us:** bulk mode and scheduled sends. Later.

### P7 (watch). "Vikram": the NRI (40–55)
- Sends Diwali and Rakhi cards to family in India. Pays in USD/GBP and has higher willingness to pay.
- Later: needs international cards and multi-currency pricing.

---

## 7. Key insights and white space

1. **Personalisation is what people pay for, not access.** People don't pay for "a Diwali image". They pay for *"a beautiful Diwali card with my family photo and our names, in Marathi"*. Build the product around **guided personalisation slots** (photo, names, relation, city, a message), not around a blank-canvas editor.
2. **Invitations fix the business model; greetings fix distribution.** Greetings are free, viral and seasonal. Invitations are paid, year-round and high-value. One brand that does both gets **festival-driven acquisition** and **event-driven revenue**, and every invitation sent introduces 100–500 guests to the brand.
3. **The recipient page is an unused growth channel.** Competitors mostly hand out an image file. A fast, ad-free, beautiful card page with **"Make your own Diwali card →"** turns every recipient into a potential creator. This is our cheapest acquisition channel.
4. **Honest commerce is a moat.** Visible trust markers will convert better with a 45+ audience that has already been burned by autopay:
   - "Pay once, no autopay."
   - "Automatic refund if your card fails."
   - A truthful "Bestseller" badge.

   This also keeps us safe under the CCPA dark-pattern guidelines and the DPDP Act.
5. **Regional premium content is under-served.** Hindi has the volume. **Marathi** (Gudi Padwa, Ganeshotsav, Marathi weddings) and **Tamil** (Pongal, Puthandu, Deepavali, Tamil weddings) look under-served at the premium end **[A]**. The audit of the top 5 catalogues will confirm this.
6. **Web plus WhatsApp beats app-only for event and search traffic.** People search "Gudi Padwa wishes in Marathi" or "griha pravesh invitation in Hindi". Mobile web wins those searches, opens instantly from a WhatsApp link, and **avoids Google Play billing fees** on digital goods (Play's service fee is reduced by only 4% under user-choice billing) **[V]**.
7. **Seasonality is real, so plan the calendar.** Demand spikes at festivals. Wedding seasons (Nov–Feb and Apr–Jun) and birthdays fill the gaps, and daily "Good Morning" content (V1) builds the habit.
8. **AI is now expected, and now regulated.** AI-written wishes in 4 languages are cheap and useful. Photo-realistic **synthetic** imagery (e.g. a user's face placed in a scene) now needs **continuous visible labels** under the IT Rules amendment of February 2026 **[V]**. Keep AI to text and stylisation in the MVP.

---

## 8. Key decisions (Phase 1)

- **Wedge:** free festival greetings for reach, plus **paid personalised premium cards and event invitations** for revenue. The business plan comes in V1 and corporate in V2.
- **Positioning:** "Your language, your photo, ready in a minute; pay once; automatic refund if it fails."
- **Pricing philosophy:** per-item (sachet and event pricing) first. An optional pass only after repeat purchase is proven. **No ads on recipient pages.**
- **Platform:** mobile-first web/PWA with strong SEO in 4 languages. Android app wrapper in V1.
- **Differentiators to invest in:** typography quality in Marathi and Tamil, a fast video render pipeline (WishNWed-level speed), and a recipient page with a viral loop.

## 9. Open questions for the founder

1. Are you comfortable leading with **invitations as the revenue engine**, even though the original idea leans toward festival greetings?
2. What is the **marketing budget** for the first 6 months? Crafto spent ₹84.6 crore on ads in FY25, so we need a capital-efficient plan (SEO, viral loop, creators). See `08-roadmap-team-cost-gtm.md`.
3. Do you have **in-house design capacity**, or will the template supply be freelance and AI-assisted?
4. Is a **Diwali 2026 pilot** (5 weeks from today) feasible on your side? It's the cheapest way to test willingness to pay before building.
5. Any **brand name** shortlist? We need domain availability in .in/.com and trademark checks.

---

## Sources

- Entrackr — Kutumb turns profitable, FY25 revenue ₹128.6 cr: https://entrackr.com/fintrackr/peak-xv-funded-kutumb-turns-profitable-enters-indicorn-club-11112881
- Angel One — Kutumb 173% revenue growth: https://www.angelone.in/news/unlisted-companies/kutumb-turns-profitable-enters-indicorn-club-with-173-revenue-growth
- Forbes India / YourStory / UNI — Primetrace ₹550 cr ARR, ₹200 cr EBITDA run-rate: https://www.forbesindia.com/article/upfront/brand-connect/consumer-ai-startup-primetrace-parent-of-kutumb-hits-%E2%82%B9200-cr-ebitda-run-rate/2992427/1 · https://yourstory.com/2026/03/consumer-ai-startup-primetrace-reports-rs-200-cr-ebitda-run-rate · https://www.uniindia.com/news/business-economy/primetrace-results/3779174.html
- Tracxn — Crafto profile: https://tracxn.com/d/companies/crafto/__94ihgKfxgAzlq7BMlLcqVz2nmIZyaEuu3S9Er2ZZfpg
- Crafto store listings and aggregators: https://play.google.com/store/apps/details?id=com.crafto.android&hl=en_IN · https://www.appbrain.com/app/crafto/com.crafto.android · https://app.sensortower.com/overview/com.crafto.android?country=IN
- Crafto pricing, autopay and refund terms: https://www.craftoapp.com/pricing-info/ · https://crafto.app/tos
- Brands.live (App Store): https://apps.apple.com/in/app/brands-live-festival-post/id1565767273
- Festival Poster Maker / Graphicwaale (Play Store): https://play.google.com/store/apps/details?id=com.festivalpost.brandpost · https://play.google.com/store/apps/details?id=com.graphicwaale&hl=en_IN
- Business Festival Poster (App Store): https://apps.apple.com/in/app/business-festival-poster/id6639621661
- Desievite: https://www.desievite.com/
- WedMeGood video invites: https://www.wedmegood.com/wedding-invitations/video-templates
- VideoGiri: https://videogiri.com/ · Selfanimate: https://www.selfanimate.com/wedding-video-invitations.html · WishNWed: https://www.wishnwed.com/ · VideoInvites.net: https://videoinvites.net/category/wedding?filter=Elite
- 123Greetings press room (pop-up ads removed): https://info.123greetings.com/company/pressroom/release_230608.html · Best ecard websites 2026 (123Greetings Pro pricing): https://recocards.com/blog/ecards/best-ecard-websites/
- Dgreetings: https://www.dgreetings.com/login1.jsp · 365Greetings: https://www.365greetings.com/
- Canva India 4th-largest market: https://www.storyboard18.com/digital/india-becomes-canvas-fourth-largest-market-as-users-create-billions-of-designs-83205.htm · Canva statistics: https://www.demandsage.com/canva-statistics/ · Canva India pricing: https://appadvisor.in/software/canva
- Jacquie Lawson pricing: https://www.jacquielawson.com/prices-membership · Punchbowl and others: https://www.kudoboard.com/blog/best-ecards/
- IAMAI–Kantar Internet in India: https://yourstory.com/2026/01/indias-internet-user-base-crosses-950-million-2025-iamai-report · https://www.business-standard.com/india-news/india-s-internet-users-to-exceed-900-mn-in-2025-driven-by-indic-languages-125011600835_1.html
- WhatsApp India users: https://couponsly.in/whatsapp-users-in-india-statistics/
- Smartphone market (IMARC): https://www.imarcgroup.com/india-smartphone-market
- UPI August 2026: https://www.business-standard.com/finance/news/upi-transactions-august-2026-record-volume-npci-126090100506_1.html
- CAIT wedding season 2025: https://cait.in/wedding-season-2025-to-generate-%E2%82%B96-5-lakh-crore-business-from-46-lakh-weddings-across-india-cait-delhi-alone-to-witness-%E2%82%B91-8-lakh-crore-trade-from-4-8-lakh-weddings-indian/
- Udyam registrations (PIB): https://www.pib.gov.in/PressReleasePage.aspx?PRID=2246892&reg=3&lang=1
- Kuku FM business model (GrowthX): https://growthx.club/blog/kukufm-business-model
- Google Play billing in India: https://support.google.com/googleplay/android-developer/answer/13306652?hl=en
- MeitY IT Rules amendment 2026 (synthetic content): https://www.hoganlovells.com/en/publications/india-introduces-mandatory-labelling-for-ai-and-3hour-takedown-for-illegal-content · https://www.medianama.com/2026/04/223-meity-ai-label-rules-mandates-continuous-disclosure/
- CCPA dark patterns guidelines: https://www.legal500.com/intelligence/india/consumer-protection/ccpas-guidelines-on-dark-patterns-an-overview · https://www.pib.gov.in/PressReleasePage.aspx?PRID=2191948&reg=3&lang=2
- Census 2011 language data: Office of the Registrar General & Census Commissioner, India (Statement 1, Scheduled Languages).

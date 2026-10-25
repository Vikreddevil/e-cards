# 09 — Lean Launch Plan (solo founder + Claude)

**Status:** Draft v2 · **Updated:** 25 October 2026 after the independent review (`10-prd-review.md`) · **First written:** 2 October 2026 · **Supersedes** the timeline, team and budget in `08-roadmap-team-cost-gtm.md` for the first launch. The full PRD (`02`–`07`) stays the long-term target. This document is the **bare minimum version** we ship first.

Labels: **[V]** sourced · **[E]** estimate · **[A]** assumption.

---

## 1. What changed and why

| Founder input | What it means for the plan |
|---|---|
| Go live after 1–2 weeks of build + testing | The original target (16 Oct) passed before the build started. **New target: Sat 14 Nov 2026, weddings first (Option B)**. Option A (Diwali + weddings, Wed 4 Nov) only if Razorpay KYC is approved by 2 Nov and testing is complete. See `10-prd-review.md` §5 for the decision rule. |
| Solo founder; Claude does the development | No team payroll. Managed services only. Claude also writes the templates as code. You handle accounts, KYC, testing on phones, and marketing. |
| Video invitations for Diwali and weddings | Launch catalogue = Diwali greetings (free images, paid videos) + Diwali puja/party video invites + wedding video invites. |
| Start slow on budget | About **₹6–7k a month** of fixed cost; no ads needed to launch (₹10–20k optional for Diwali). |

**Why the PRD's validation pilot is no longer separate:** launching now *is* the pilot. Diwali 2026 tests whether people pay, and the wedding season tests invitations, on the real product.

---

## 2. Scope: what's in at go-live

| Area | In at go-live | Deferred (from the PRD) |
|---|---|---|
| Catalogue | Diwali collection (Dhanteras, Diwali, Bhai Dooj), Diwali puja/party invites, wedding invites | Other festivals (added each week after launch), birthdays, Good Morning |
| Languages | Hindi, English, Marathi, Tamil: UI + template text. Claude translates; **a native speaker reviews Marathi and Tamil** | More languages |
| Editor | Guided slots: names, photo, message, date/time/venue (invites), music choice, language | Background removal, transliteration, AI wish writer (replaced by **ready-written wishes per language**) |
| Output | Image (free, small logo) and video (paid, HD, no watermark); watermarked preview before paying. **Invitations also export JPG + PDF.** **WhatsApp-safe video spec:** ≤ 60s, 9:16 1080×1920, H.264, text kept inside safe zones and above a minimum size so it survives WhatsApp compression; a "send as document for full HD" tip | Multiple formats beyond Status 9:16 and Square; print-ready PDF with bleed |
| Sharing | WhatsApp **file** share first, then link share and download; **recipient page with "Make your own"** | Replies ("send wishes back"); sender "viewed N times" summary (P1) |
| Trust and safety | Share text says "Video invitation — no app download needed"; pages **never** ask for a download or install; "Safe link: no app, no login" line on event pages; user content may only link to allowlisted sites (Google Maps, YouTube); "Report" button; photo-rights and acceptable-use terms shown at upload | Automated content moderation (P1) |
| Invitations | **Indian invitation content model:** inviting family, optional relatives list, muhurat/time, ritual/function list, RSVP contact persons, religious header symbol (Ganesha / Om / Ik Onkar / Bismillah / Cross / Jain / none). **Bride's-side and groom's-side versions** in wedding Premium. Event page with a **multi-function schedule**, map link, add to calendar; RSVP for wedding Premium | RSVP export, co-hosts, guest reminders and update notices, QR code on invites, livestream link, save-the-date and thank-you cards (all P1) |
| Accounts | **No login.** Buyer gives phone + email at checkout. A private "manage" link is emailed for re-download and edits | OTP login, My Cards dashboard |
| Payments | Razorpay hosted checkout (UPI intent/QR, cards, net banking), verified webhooks, **automatic refund if the render fails**, emailed receipt | Coupons, referrals, Pass, Business Plan, second gateway |
| Ops | Log tables (orders, payments, webhooks, renders, refunds, errors); **email alerts to you** for payment failures, refund failures and render failures; **analytics per `11-analytics-tracking-plan.md`** (core events + launch KPIs in §6 there); Sentry errors | Grafana dashboards, PagerDuty, daily reconciliation report (in week 3) |
| Admin | Templates are added in code by Claude; orders and refunds are viewed in the Supabase and Razorpay dashboards | Admin CMS |
| Support | FAQ in 4 languages; WhatsApp Business number with auto-replies; order lookup through the emailed manage link; reply within 24h (4h on festival days) | Ticketing tool |
| Link lifetime | Free cards: link live 12 months. Paid cards: 3 years. Event pages: live until 90 days after the last function, then archived; RSVP data deleted after 12 months. Stated on the pricing page | Host-paid extensions |
| Honest pricing | "Diwali launch price" label; **no strike-through until a price has actually been charged** for 30 days; badges are "New" and "Diwali special" only, until real sales data exists | Data-driven Bestseller/Trending badges (switched on once there are ≥ 25 orders in 7 days) |

---

## 3. User flow (go-live)

```mermaid
flowchart TD
  A[Visitor arrives<br/>WhatsApp link / Google / Instagram] --> B[Home: Diwali + Weddings<br/>language picker हिं / EN / म / த]
  B --> C[Pick a template]
  C --> D[Personalise: names, photo, message,<br/>date & venue for invites, music]
  D --> E[Watermarked preview]
  E -->|Free image greeting or invite| F[Render image → share / download]
  E -->|Paid video| G[Phone + email → Razorpay checkout<br/>UPI / card / net banking]
  G -->|Payment fails| G2[Clear message, retry, card kept]
  G -->|Verified paid| H[Render HD video ≤ 60s]
  H -->|Render fails 3×| R[Automatic refund + email to buyer + alert to you]
  H -->|Success| I[Share to WhatsApp / download<br/>+ email with manage link]
  I --> J[Recipient opens card or event page<br/>RSVP for weddings]
  F --> J
  J -->|Make your own| C
```

---

## 4. Lean tech stack

| Need | Choice | Monthly cost **[E]** | Why |
|---|---|---|---|
| Website (Next.js, 4 languages, SEO) | **Vercel Pro** | ~₹1,800 ($20) | Fastest to ship and deploy; commercial use needs the Pro plan |
| Database + file storage for uploads | **Supabase Pro**, Mumbai region | ~₹2,200 ($25) | Postgres + storage + daily backups in one service, hosted in India |
| Video render worker (FFmpeg + headless Chromium) | **DigitalOcean droplet in Bangalore**, 2 vCPU / 4 GB | ~₹2,100 ($24); bump to 4 vCPU for Diwali week (+~₹1,000 prorated) | Renders 200–300 videos an hour **[E]**; jobs queue in Postgres |
| Rendered videos/images delivery | **Cloudflare R2** + Cloudflare DNS/CDN/WAF (free plan) | ₹0–500 | Zero egress fees; DDoS protection |
| Payments | **Razorpay** | ₹0 fixed; ~2% per payment + 18% GST on the fee **[V]** | Best docs, UPI intent, refund API, quick onboarding |
| Email (receipts, alerts) | **Resend** (free tier 3,000/month) | ₹0 | Simple API |
| Analytics / errors | **PostHog** + **Sentry** (free tiers) | ₹0 | Funnels and error tracking from day 1 |
| Domain (.com + .in) | Any registrar | ~₹2,500/year | — |
| Business email (support@yourbrand) | Zoho Mail | ₹0–₹60 | Needed for Razorpay and trust |
| **Total fixed** | | **≈ ₹6,000–7,000/month** | |

**Video templates are built as code:** each template is an HTML/CSS/SVG animation with slots. The worker opens it in headless Chromium (which shapes Hindi, Marathi and Tamil text correctly), captures the frames, and encodes an MP4 with FFmpeg, adding the music. Claude can author these templates. Quality will be clean and modern, but not at the level of a professional motion designer. Once revenue comes in, buy or commission 3–5 hero templates.

---

## 5. Pricing at launch

Unregistered for GST at launch (see §7), so these are the full prices.

| Product | Price | What you get |
|---|---|---|
| Diwali image greeting | **Free** | Name + photo, small corner logo, share/download |
| Diwali video greeting (15–20s, music) | **₹29** (Diwali launch price; Option A only) | HD, no watermark, 3 edits within 7 days |
| Image invitation (wedding, Diwali puja/party, griha pravesh) | **Free** with small logo · **₹49** without logo | JPG + PDF + event page; the free version keeps us competitive with free invitation makers |
| Diwali puja / party video invitation | **₹99** | 20–30s video + event page with map and add-to-calendar (Option A only) |
| Wedding video invitation: Standard | **₹199** | 30–40s, 2 photos, 1 language, 1 family side, JPG + PDF |
| Wedding video invitation: **Premium (Recommended)** | **₹399** | 45–60s, up to 6 photos, **both family-side versions**, multi-function event page + RSVP, JPG + PDF, unlimited edits for 14 days |

**Unit economics per sale [E]:**

| Sale | Gateway fee | Render + delivery | You keep |
|---|---|---|---|
| ₹29 video greeting | ~₹0.70 | ~₹0.20 | ≈ ₹28 |
| ₹399 wedding invite | ~₹9.40 | ~₹1 | ≈ ₹388 |

**Break-even on fixed costs:** about **17 Premium wedding invites** or **230 Diwali videos** a month.

**About the discount display you wanted:** India's 2023 rules on dark patterns (from the consumer protection authority, CCPA) make a strike-through "MRP" that was never charged illegal. At launch we use **"Diwali launch price"** and **"Recommended"** labels instead. Real strike-through prices and "Bestseller" badges switch on automatically once there's genuine price history and sales data.

---

## 6. Schedule (re-baselined 25 Oct 2026)

**Option B (recommended):** build starts **Mon 26 Oct**; go-live **Sat 14 Nov**, about 3 weeks including buffer for KYC and native review. Option A uses the same sequence compressed to Diwali scope, with go-live Wed 4 Nov.

| Days (Option B) | Claude builds | You do |
|---|---|---|
| **1** (Mon 26 Oct) | Project setup; a **live landing page with Terms (incl. photo rights and acceptable use), Privacy, Refund policy, Pricing, FAQ and Contact pages** (Razorpay checks these during KYC) | Buy the domain; create the accounts (§7); **submit Razorpay KYC today** if not already done |
| 2–5 (27–30 Oct) | Catalogue, 4-language UI, editor with the Indian invitation slots, watermarked preview, render worker (image, video, PDF), uploads, analytics core (envelope + automatic events) | Find Marathi and Tamil reviewers who know **wedding invitation conventions**; collect sample photos |
| 6–9 (31 Oct – 3 Nov) | Razorpay checkout + webhooks + automatic refunds; receipt emails with the manage link; recipient and event pages (multi-function schedule, RSVP, trust measures); log tables; email alerts; remaining analytics events | Review the policy texts; set up support@ email and the WhatsApp Business number |
| 10–13 (4–7 Nov) | Templates: 8 wedding video invites (Hindi / Marathi / Tamil / modern English styles, both family sides), 6 image invites, 4 griha pravesh invites; Diwali free images if time allows; ready-written wishes (30 per language for the launch occasions) | Native-speaker review; pick the music tracks (licence check) |
| 14–17 (8–11 Nov) | Fixes; security checklist; analytics QA (`11` §7.8); light load test | **Test on real phones**: a cheap Android phone, an iPhone, inside WhatsApp; test payments in test mode, including a failed payment and a refund |
| **18–20** (12–14 Nov) | Switch to Razorpay live keys; Search Console + sitemap; launch-day monitoring | Soft launch to family and friends + WhatsApp groups; Instagram posts |

**Critical path:** **Razorpay KYC approval** (usually a few working days **[A]**). If it isn't approved by launch day, we go live with free greetings and turn on paid videos the day it clears.

---

## 7. Bare-minimum go-live checklist (what you provide)

### Must have
- [ ] **PAN + a bank account** in your name (individual/proprietorship is fine to start)
- [ ] **Razorpay account** with KYC submitted on Day 1 (they'll check that the website and policy pages are live)
- [ ] **Domain name** (.com and .in if you can)
- [ ] **Business email** (support@yourdomain) and a **contact phone number** shown on the site, as the consumer e-commerce rules require
- [ ] **Accounts** (free to create; Claude will guide you): GitHub, Vercel, Supabase, Cloudflare, DigitalOcean, Resend, PostHog, Sentry, Google Search Console
- [ ] **API keys entered by you** into Vercel/Supabase settings. Never paste secrets into chat
- [ ] **Native-speaker reviewers** for Marathi and Tamil (a few hours each), ideally people who know **wedding invitation conventions** (family names, relatives list, wording)
- [ ] **WhatsApp Business number** for support (can be your second number)
- [ ] **Music**: 5–8 tracks with a licence that allows use inside videos you sell (royalty-free packs; ₹0–5,000)
- [ ] **Test devices**: one low-end Android phone + WhatsApp; an iPhone if possible
- [ ] **Grievance contact** (can be you) named on the site

### Not needed on day 1
- **GST registration:** service providers under ₹20 lakh annual turnover generally don't need to register, even when selling across states **[A: confirm with a CA]**. Register once you get close to that.
- A company or LLP (start as an individual; incorporate later).
- A lawyer (Claude drafts the policies; an optional ₹5–15k review later).
- Trademark registration (file once the name is final; ~₹4,500 for an individual filing in one class **[A]**).

---

## 8. Brand name

**Recommendation: Toran** (तोरण · தோரணம்)
- **Why it works:** a toran is the auspicious decorated door-hanging put up for **both festivals and weddings**, exactly our two collections.
- **Pan-Indian:** the same word in Hindi and Marathi (तोरण), and *thoranam* (தோரணம்) in Tamil, so one name works in all four languages.
- **Easy to say and spell:** short, two syllables, easy in English and Devanagari, and memorable.
- **Open in this category:** searches didn't find an e-card or invitation product called Toran (unlike "Shubh Cards", which is an established Chennai invitation printer with shubhcards.com, and "Nimantran", which is crowded).
- **Domain suggestions:** `toran.cards`, `torancards.in` / `torancards.com`, `gettoran.com`, `toran.app`. Adding "cards" gives a light keyword signal.

**Backups:**
- **Rangoli Cards:** festive and universal, but "rangoli" is generic, so it's harder to own in search and to trademark.
- **Utsavam:** "festival" in Tamil/Telugu (Hindi *utsav*); pan-Indian, but many "Utsav" brands already exist.

**The truth about SEO and names:** an exact-match domain gives little ranking benefit today. Rankings come from **per-language festival pages** (e.g. "Diwali wishes in Marathi"), page speed and backlinks, and the plan builds those. The name matters for **memorability, trust and click-through on WhatsApp previews**.

**Before you buy:** domain availability and trademarks could not be checked from this environment.
1. Check the domain on a registrar.
2. Run a **trademark search** on the IP India public search (classes 9, 35, 42).

---

## 9. Costs summary

| | Amount **[E]** |
|---|---|
| One-time to launch | ₹3–8k (domain, music, test-phone data) |
| Fixed monthly | ₹6–7k |
| Diwali-week server bump | ~₹1k |
| Per sale | ~2.4% gateway fee (incl. GST on the fee) + < ₹1 render |
| Optional Diwali promotion | ₹10–20k (Instagram/Facebook boosts in Hindi/Marathi/Tamil) |
| Your Claude subscription | Separate (whatever plan you're on) |

**Scale-up path:** stay on this stack up to roughly 50–100k monthly users. Then move step by step to the PRD architecture (`04-system-design.md`): queue-based worker autoscaling, OTP accounts, an admin CMS and a second gateway. Most of the code carries over.

---

## 10. Risks of the lean approach

| Risk | Mitigation |
|---|---|
| Razorpay KYC is delayed | Apply on Day 1; launch free greetings first if needed |
| Template quality from code-built designs | Strong typography and colour; buy 3–5 hero templates from first revenue |
| One render server becomes a bottleneck on Diwali day | Queue with "we'll email you when it's ready"; resize the droplet the week before; free images render separately |
| Marathi/Tamil quality | Native review is mandatory before those templates go live |
| Support load on a single person | Clear FAQ, automatic refunds, a WhatsApp Business auto-reply |

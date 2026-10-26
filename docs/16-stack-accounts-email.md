# 16 — Final Stack, Accounts and Email (decision record)

**Status:** Decided · **Owner:** Founder + Claude
**Supersedes:** the launch stack in `09` §4 (Vercel, Supabase, DigitalOcean, Resend) and the environments in `15` §2. The scale-up architecture in `04` stays the long-term target.

**Decisions:**
1. **Cloud: AWS**, in Mumbai (`ap-south-1`).
2. **Stack: lean, pay-per-use AWS.** No always-on servers or databases. About **₹100–600/month at launch** **[E]**.
3. **Database: DynamoDB** for launch, behind a repository layer so a later move to PostgreSQL stays contained.
4. **Email: Zoho Mail Free** for `support@cardreel.in`. App emails (receipts, manage links, alerts) go through Amazon SES.

Labels: **[V]** sourced · **[E]** estimate · **[A]** assumption.

---

## 1. Final stack

| Layer | Choice | How it protects quality and user experience |
|---|---|---|
| Website | **Next.js** (TypeScript), built as static files in 4 languages, served from **S3 + CloudFront** | Very fast pages, including inside WhatsApp's in-app browser; good for SEO; fonts loaded per script |
| Personal card and event pages | **Small static HTML files generated when a card renders**, served from CloudFront | Open instantly from WhatsApp, with a correct preview image and title |
| Template search | **Runs in the browser** over a pre-built template index with Hinglish aliases | Instant results; no server cost |
| API (save, render, Notify me, RSVP, payments) | **API Gateway (HTTP API) + Lambda** (Node.js 22, TypeScript) | Built-in throttling (rate limits); scales automatically |
| Database | **DynamoDB** on-demand, point-in-time recovery on | Data copied across 3 zones; handles festival spikes; no maintenance |
| Photos and finished cards | **S3** with lifecycle rules (uploaded originals deleted after 30 days; free renders after 12 months) | Durable; storage cost stays low |
| Rendering | **SQS → Lambda container** (FFmpeg + headless Chromium), concurrency capped at 20, keep-warm ping in busy hours | **Correct Hindi, Marathi and Tamil lettering** in final videos; queue with progress messages; instant browser preview while the final file renders |
| Payments (Phase 1) | **Razorpay** Standard Checkout + verified webhooks → Lambda; keys in **SSM Parameter Store** | No double charges ("only update if still pending" writes); automatic refunds when a render fails |
| Email sending | **Amazon SES** | Reliable, ~₹9 per 1,000 emails |
| Support inbox | **Zoho Mail Free** (§6) | A real `support@cardreel.in` mailbox |
| Bot check and DNS | **Cloudflare Turnstile + Cloudflare DNS** (free) | Stops bots without annoying people |
| Monitoring | **CloudWatch** alarms → email, **Sentry** errors, **PostHog** analytics | Problems reach you before users complain |
| Infrastructure and deploys | **AWS CDK** (TypeScript) + **GitHub Actions** with OIDC; separate staging and production | Repeatable releases; no AWS keys in chat or in this session |

**Consciously given up for now** (users don't see these):
- an admin dashboard (templates are added in code);
- complex SQL reporting (S3 export + Athena, plus PostHog, instead);
- server-rendered pages (not needed with pre-built pages).

---

## 2. Why the lean setup

The first AWS proposal (**Aurora Serverless + Amplify Hosting**) came to **≈ ₹5,500–9,000/month** **[E]**. Two always-on parts drove that cost:
- Aurora's **minimum capacity** (~₹4–5k/month even with zero users);
- Amplify's **server-rendering compute** (~₹0.5–1.3k).

The lean setup is pay-per-use and fits within AWS's **always-free allowances**:
- Lambda: 1M requests + 400,000 GB-seconds a month.
- DynamoDB: 25 GB.
- SQS: 1M requests.
- CloudFront: 1 TB data out + 10M requests a month **[V]**.

New accounts also get **up to $200 in credits** (about ₹17k) on the free plan **[V]**.

**Cost guardrails:**
- **AWS Budgets** emails at **₹500** and **₹2,000**; **Cost Anomaly Detection** on.
- **Render concurrency cap of 20**, so a bot can't run up costs; 5 free videos per device per day; API throttling; Turnstile.
- **CloudWatch log retention** of 14 days, plus a **daily export to S3 kept for 180 days** (CERT-In requires 180 days of logs in India; `17` §8.5); **S3 lifecycle** clean-up.
- **Never, until there's a clear need:**
  - NAT gateways (~₹3k/month);
  - idle EC2 or RDS;
  - public IPv4 addresses (~₹300/month each);
  - Secrets Manager (SSM Parameter Store is free);
  - AWS WAF (added only if we're attacked).

---

## 3. DynamoDB vs PostgreSQL

### 3.1 Cost (Mumbai, per month) **[E]**

| Option | Launch (~10k users) | ~50k users | ~200k users | Hidden extras |
|---|---|---|---|---|
| **DynamoDB** (on-demand) | **₹0–200** | ₹200–800 | ₹800–3,000 | None. 25 GB free forever; on-demand prices were cut 50% in Nov 2024 **[V]** |
| **RDS PostgreSQL** (db.t4g.micro, one zone) | ~₹1,600 (instance ~$15 + storage) **[V/E]** | ₹2,500–4,000 | ₹8,000–15,000 (two-zone copy) | Lambda needs a **NAT gateway (~₹3k/month)** or a **database open to the internet**; RDS Proxy for many concurrent Lambdas |
| **Aurora Serverless** (PostgreSQL) + Data API | ₹4,000–5,000 | ₹5,000–8,000 | ₹10,000+ | Minimum charge even with zero users |

### 3.2 Quality for our use case

| What matters | DynamoDB | PostgreSQL (small RDS) |
|---|---|---|
| Speed for users | Very fast (single-digit ms) | Very fast |
| Staying up | Copies across 3 zones; no maintenance downtime | One machine in one zone; brief downtime for maintenance or failures unless you pay for two zones |
| Festival spikes | Scales instantly | A small instance can run out of connections or memory |
| Payment correctness | ✅ Conditional writes + transactions | ✅ Classic transactions (slightly easier to reason about) |
| Security | No open port; IAM over HTTPS | Private network + NAT, or an internet-facing database |
| Maintenance | None | Upgrades, storage, connection limits |
| Reports and ad-hoc questions | ❌ Weak. Export to S3 + Athena; PostHog for user analytics | ✅ Strong. Any SQL query |
| Changing the data model later | Needs planning (indexes per access pattern) | Flexible |
| Lock-in | AWS only | Portable |

**Decision: DynamoDB for launch.** It wins on cost, uptime, spikes, security and maintenance. PostgreSQL only wins on reporting, which we cover with PostHog and occasional exports.

All database access goes through **one repository layer** in the code. If reporting needs grow with revenue, moving to PostgreSQL (the design in `04` §6) is a contained, one-time job.

**Design notes:**
- **Tables:** `cards`, `events`, `rsvps`, `waitlist`, `orders`, `payments`, `webhook_events`, `render_jobs`, `counters`, `idempotency_keys`, `reports`. A few clear tables rather than one complex single-table design. Refunds live in the `payments` ledger, one partition per order (keys and indexes in `17` §4.1).
- **Idempotency:** conditional writes on status transitions; webhook event IDs stored with a uniqueness condition.
- **Daily export** to S3 for analysis.

---

## 4. Monthly cost by scale **[E]**

| Monthly users | AWS cost | Main driver |
|---|---|---|
| Launch (5–10k) | **₹100–600** (≈ ₹0 while credits last) | SES, S3 storage, DynamoDB requests |
| ~50k | ₹1,500–3,000 | Renders beyond the free tier |
| ~200k | ₹6,000–12,000 | Renders + downloads beyond 1 TB |

**Other running costs:**
- Domain: ₹1 for the first year, then the normal renewal price.
- Razorpay: ~2% per sale (Phase 1).
- Email: **₹0** (Zoho Free, §6).
- PostHog, Sentry and Cloudflare: free tiers.

---

## 5. Accounts

### 5.1 Kept

| Account | Purpose | Cost |
|---|---|---|
| **AWS** | Hosting, API, database, storage, rendering, email sending, monitoring | ≈ ₹100–600/month |
| **GoDaddy** | Domain registrar for `cardreel.in` | ₹1 first year |
| **GitHub** | Code; GitHub Actions deploys to AWS | Free |
| **Cloudflare** | **Turnstile bot check + DNS only** | Free |
| **Razorpay** | Payments (sign up now; KYC on launch day, `15` §3.3) | ~2% per sale |
| **PostHog** | Product analytics | Free tier |
| **Sentry** | Error tracking | Free tier |
| **Zoho Mail** | `support@cardreel.in` inbox | Free (§6) |
| **WhatsApp Business** | Customer support | Free |
| **Google Search Console** | Search visibility | Free |
| **Instagram, YouTube, Facebook, Pinterest, X** | @cardreel handles | Free |

### 5.2 Dropped
**Vercel** (replaced by S3 + CloudFront), **Supabase** (DynamoDB + S3), **DigitalOcean** (SQS + Lambda), **Resend** (SES). Cloudflare R2, CDN and Access are replaced by S3, CloudFront and CloudFront basic auth.

### 5.3 AWS setup (founder, Day 0)
1. Create the AWS account with the business email and a card enabled for international payments.
2. **Turn on MFA** for the root login. Create an everyday admin user in **IAM Identity Center** with MFA, and stop using root.
3. Use the **Mumbai** region (`ap-south-1`).
4. Set **AWS Budgets** alerts at **₹500** and **₹2,000**, and turn on **Cost Anomaly Detection**.
5. *Optional, recommended:* separate **staging** and **production** accounts under AWS Organizations (free). Note that free-tier allowances are shared across an organization.

**Day 1 (Claude provides the script):**
- Run one bootstrap script in **AWS CloudShell** to create the GitHub OIDC deploy role. From then on, GitHub Actions deploys everything.
- **Request SES production access.** Until then SES sends only to verified addresses, 200 a day, and approval usually takes 1–2 business days **[V]**.
- **Check the Lambda concurrency quota.** New accounts can start low; request an increase if needed.

---

## 6. Email: Zoho Mail (final)

**Decision:**
- **Zoho Mail Free** for `support@cardreel.in`, with `hello@cardreel.in` as an alias.
- App emails (receipts, manage links, "Notify me" confirmations, alerts) are sent by **Amazon SES** whichever inbox we use.
- **Move to Google Workspace** when revenue starts or a team member joins.

### 6.1 Comparison

| | **Zoho Mail Free** (chosen) | **Google Workspace** (Business Starter) | Free Gmail + forwarding |
|---|---|---|---|
| Address | support@cardreel.in ✅ | support@cardreel.in ✅ | support@cardreel.in ✅ (forwarded into Gmail) |
| Cost | **₹0** (up to 5 users) | ~₹270/user/month + 18% GST on the yearly plan (₹325 month to month) **[V]** | ₹0 (Cloudflare forwarding + SES for sending) |
| App | Zoho Mail app (web, Android, iPhone) | Gmail app | Gmail app |
| Storage | 5 GB per user | 30 GB per user | 15 GB |
| Other mail apps (Outlook, Apple Mail) | ❌ Not on the free plan (Mail Lite adds it) | ✅ | ✅ |
| Extras | Basic | Meet, Drive, Calendar, shared drives | Personal Gmail features |
| Reliability and spam filtering | Good | Best | Good, but **fiddly**; misconfigured sending can land replies in spam |
| Setup | ~15 minutes | ~15 minutes | ~45 minutes; two systems to maintain |

**Aliases:** each Google Workspace user can have up to 30 extra addresses for free. Cost depends on **people who need their own login**, not on the number of addresses.

### 6.2 Cost as the team grows (per month, incl. 18% GST) **[V/E]**

Assumptions:
- **Google Workspace:** ₹270 + GST = **~₹319 per user** (yearly plan).
- **Zoho:** free up to 5 users. From the 6th user, everyone moves to **Mail Lite**, at ~₹59 + GST = **~₹70 per user** (the free plan doesn't mix with paid users **[A]**; confirm at checkout).

| Users | Google Workspace / month | Google Workspace / year | Zoho Mail / month | Zoho Mail / year |
|---|---|---|---|---|
| 0 | ₹0 | ₹0 | ₹0 | ₹0 |
| 1 | ₹319 | ₹3,823 | **₹0** (Free) | ₹0 |
| 2 | ₹637 | ₹7,646 | **₹0** (Free) | ₹0 |
| 3 | ₹956 | ₹11,470 | **₹0** (Free) | ₹0 |
| 4 | ₹1,274 | ₹15,293 | **₹0** (Free) | ₹0 |
| 5 | ₹1,593 | ₹19,116 | **₹0** (Free) | ₹0 |
| 6 | ₹1,912 | ₹22,939 | ₹418 (Lite) | ₹5,012 |
| 7 | ₹2,230 | ₹26,762 | ₹487 | ₹5,847 |
| 8 | ₹2,549 | ₹30,586 | ₹557 | ₹6,683 |
| 9 | ₹2,867 | ₹34,409 | ₹627 | ₹7,518 |
| 10 | ₹3,186 | ₹38,232 | ₹696 | ₹8,354 |

### 6.3 Setup steps
1. Sign up at **zoho.in** (confirm the Forever Free plan is offered at signup) with `cardreel.in`.
2. **Verify the domain** with a TXT record in Cloudflare DNS.
3. Add Zoho's **MX** records, plus **SPF, DKIM and DMARC**.
   - **SPF:** one record that includes both Zoho and Amazon SES (a domain can have only one SPF record).
   - **DKIM:** Zoho's key, plus SES's DKIM records on the same domain.
   - **DMARC:** start at `p=none` with reports, then tighten to `quarantine` once both senders pass.
4. Create `support@cardreel.in`, and add `hello@cardreel.in` as an alias.
5. Install the **Zoho Mail app** on your phone; set an auto-reply for festival days that points to the FAQ and WhatsApp.

---

## Sources
- AWS free tier and credits: [AWS Free Tier credits and 6-month free plan (July 2025)](https://aws.amazon.com/about-aws/whats-new/2025/07/aws-free-tier-credits-month-free-plan/) · [AWS Free Tier FAQs](https://aws.amazon.com/free/free-tier-faqs/) · [Always-free allowances in 2026](https://dev.to/aiunplugged/aws-free-tier-in-2026-what-actually-stays-free-5bcp) · [AWS Free Tier explained (Rackspace)](https://spot.rackspace.com/blog/aws-free-tier)
- Databases: [DynamoDB on-demand price reduction (Nov 2024)](https://aws.amazon.com/about-aws/whats-new/2024/11/amazon-dynamo-db-reduces-prices-on-demand-throughput-global-tables) · [RDS db.t4g.micro pricing by region](https://www.bytebase.com/dbcost/rds/instance/db.t4g.micro/) · [RDS cost breakdown 2026](https://infratally.com/articles/aws-rds-pricing-explained-2026/) · [Aurora scale to zero](https://aws.amazon.com/about-aws/whats-new/2024/11/amazon-aurora-serverless-v2-scaling-zero-capacity) · [Aurora PostgreSQL RDS Data API](https://aws.amazon.com/about-aws/whats-new/2023/12/amazon-aurora-postgresql-rds-data-api/)
- AWS services: [Amplify IAM compute roles for SSR](https://aws.amazon.com/blogs/mobile/iam-compute-roles-for-server-side-rendering-with-aws-amplify-hosting/) · [CloudFront OAC for Lambda function URLs](https://aws.amazon.com/about-aws/whats-new/2024/04/amazon-cloudfront-oac-lambda-function-url-origins) · [SES sandbox and production access](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html)
- Email: [Google Workspace pricing India](https://www.hapihhost.in/blog/google-workspace-price/) · [Google Workspace plans India 2026](https://siriusstar.in/cloud-solutions/google-workspace-for-business/) · [Zoho Mail free plan India 2026](https://www.cloudfysystems.com/blog/zoho-mail-free-forever-plan-india) · [Zoho Mail free plan limits and Mail Lite](https://markrelay.com/research/site.zoho.com-mail-free-plan-custom-domain) · [Cloudflare Email Routing is receive-only](https://community.cloudflare.com/t/email-routing-send-mail-as/581985)

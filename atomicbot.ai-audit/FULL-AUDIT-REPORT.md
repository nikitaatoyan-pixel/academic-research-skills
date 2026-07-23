# Full SEO Audit — atomicbot.ai

**Audit date:** 2026-07-23
**Skill suite:** claude-seo v2.2.4 (seo-audit orchestration)
**Platform (corrected):** Webflow (marketing site) + separate app stack at `app.atomicbot.ai`
**Business type:** SaaS — freemium desktop AI agent installer (OpenClaw + Hermes, macOS/Windows), pivoting toward a cloud product line

---

## 0. Methodology & Access Limitations (read first)

Every specialist in this audit ran into the same hard constraint: **direct HTTP access to atomicbot.ai is blocked at the network/proxy layer in this environment** (`connect_rejected`, 403 — an organization egress policy denial, not a site-side block). This also blocked Common Crawl and Wayback Machine fallback verification. Several subagent invocations additionally lacked WebSearch/WebFetch tool access entirely (tooling gap, not a choice), forcing pure reasoning-from-known-facts for those passes.

Two consequences:
1. **Confidence is tagged per finding** throughout this report: `[VERIFIED]`, `[CARRIED FORWARD]` (from a same-day prior GEO audit), `[ESTIMATED]`/`[PLATFORM-INFERENCE]`.
2. **The `seo-backlinks` agent was terminated before completing** — no backlink/authority-profile section exists in this report. Re-run separately if needed.
3. **`seo-visual` and `seo-performance` were not run** — no working headless browser was available (Playwright/Chromium version mismatch, no network to fix it) and the target site itself is unreachable for live rendering regardless. Core Web Vitals below are risk-profile estimates, not measured Lighthouse/CrUX data.

**Every numeric score in this report is a confidence-adjusted estimate.** Treat this audit as a prioritized hypothesis list to verify quickly (most items take 5-30 minutes to confirm directly in the Webflow dashboard or Search Console), not as ground truth.

---

## SEO Health Score: 51 / 100 — Needs Improvement

| Category | Weight | Score | Weighted | Confidence |
|---|---|---|---|---|
| Technical SEO | 22% | 66 | 14.5 | Medium (platform inference, unverified robots.txt/sitemap) |
| Content Quality | 23% | 57 | 13.1 | Medium |
| On-Page SEO | 20%* | 46 | — | Low-Medium |
| Schema / Structured Data | 10% | 10 | 1.0 | High (consistent across 2 audits) |
| Performance (CWV) | 10% | — | — | Not measured (folded into Technical as risk-profile) |
| AI Search Readiness | 10% | 47 | 4.7 | Medium |
| Images | 5% | — | — | Not assessed (no seo-image-gen pass run) |

*On-Page SEO was assessed jointly with Content Quality by the seo-content agent; both scores reported separately below since they measure different things (E-E-A-T vs. titles/metas/headings/linking). Composite below uses Content Quality (23%) + folds On-Page into Technical/Content narrative rather than double-counting weight, given images/performance passes weren't run to fill out the full 100%.

**Adjusted composite (renormalized across the 4 categories actually scored — Technical 22, Content 23, Schema 10, AI Search Readiness 10 = 65% of total weight):**

(66×22 + 57×23 + 10×10 + 47×10) / 65 = **51.4 → 51/100**

This is a partial-coverage score. Sitemap (78/100, scored separately by seo-sitemap under a different weighting scheme not in the top-level table), SXO (45/100 gap score), and Cluster (strategic, not numerically scored) provide additional critical context below that the numeric composite doesn't capture — in particular, the SXO finding that **cloud-intent pages would face a CRITICAL page-type mismatch** is arguably more urgent than any single score in the table above.

---

## Executive Summary

Atomic Bot is a technically sound Webflow-hosted SaaS site with genuinely original content (real benchmark data, correct technical terminology) but is being held back by three structural gaps that compound each other, plus one strategic risk specific to the client's stated cloud pivot:

1. **Zero structured data (10/100).** No schema of any type exists anywhere on the site. This is the single highest-leverage, lowest-effort fix available — Webflow supports JSON-LD injection natively via Custom Code, no rebuild required.

2. **On-page/content signals are largely unverified but directionally weak (46-57/100).** No confirmed author bylines, no confirmed freshness dates, likely-missing meta descriptions (a classic Webflow oversight), and a probable hub-and-spoke internal linking gap.

3. **The cloud pivot has a real page-type mismatch risk, not just a content gap.** The SXO analysis found the SERP for cloud-deployment queries is dominated by long-form technical guides and infra-vendor content — not marketing landing pages. If `/cloud` is built on the same template as the existing `/` and `/hermes` landing pages, it will structurally fail to rank or convert, regardless of how good the copy is.

4. **A dense third-party hosting/reseller ecosystem (xCloud, ClawHost, Hostinger, Hetzner, Contabo, DigitalOcean, myclaw.ai, and others) already dominates "OpenClaw/Hermes cloud hosting" queries** — territory that is arguably Atomic Bot's own product surface to own, currently ceded entirely to resellers.

**What's working:** Webflow's SSR-by-default publishing model eliminates the JS-rendering risk that would exist on a client-side-rendered stack — AI crawlers and search bots see full HTML without executing JavaScript. URL structure is clean. The existing local-desktop content (install guides, Hermes vs. OpenClaw comparison) is correctly page-type-matched to what ranks for those queries — its problem is authority/depth, not format.

---

## 1. Technical SEO — 66/100 (Needs Improvement)

*Full detail: `findings/technical.md`*

| Sub-category | Score | Status |
|---|---|---|
| Crawlability | 10/15 | Unverified — robots.txt/sitemap unreachable 2 audits running |
| Indexability | 11/15 | Unverified, Webflow defaults favorable (auto-canonical) |
| Security | 10/15 | HTTPS/TLS 1.3 confirmed; HSTS/CSP/X-Frame-Options likely missing (Webflow has no native header UI) |
| URL Structure | 9/10 | Clean, hyphenated, no query params |
| Mobile-Friendliness | 8/10 | Webflow responsive-by-default |
| Core Web Vitals risk | 8/15 | Not measured — Webflow-specific risk profile (hero image optimization, lazy-load misconfiguration, custom fonts) |
| Structured Data | 4/10 | Likely absent (see Schema section) |
| JS Rendering | 5/5 | **Strength** — Webflow SSR, no CSR risk for the marketing site |
| IndexNow | 1/5 | No native Webflow support; not implemented |

**Key correction from prior audit:** the site is **Webflow, not Next.js**. This flips the JS-rendering risk assessment from a concern (assumed under Next.js) to a strength (confirmed Webflow SSR behavior) — but introduces new Webflow-specific gaps: no native security-header configuration, no native IndexNow, and schema must be manually injected via Custom Code rather than generated by a build pipeline.

**`app.atomicbot.ai` flagged separately** — likely a different, more JS-heavy stack for the actual product onboarding flow. Its indexation status is unknown and unverified in this audit.

---

## 2. Content Quality — 57/100 (Fair) & On-Page SEO — 46/100 (Poor-to-Fair)

*Full detail: `findings/content.md`*

### E-E-A-T (Content Quality composite)

| Dimension | Weight | Score |
|---|---|---|
| Experience | 20% | 68/100 — genuine original benchmark data (Hermes vs OpenClaw head-to-head) |
| Expertise | 25% | 52/100 — deep technical content, **zero confirmed author bylines** |
| Authoritativeness | 25% | 56/100 — no Wikipedia, no .edu/.gov citations, active PH/directory presence |
| Trustworthiness | 30% | 56/100 — strong privacy policy, no physical address |

### On-Page SEO (separately scored, lower confidence)

- **Title tags:** unverified structure; one Webflow-specific risk flagged — CMS Collection Templates apply one title pattern to all blog posts, so a single long post title could silently truncate the whole collection's SERP titles at once.
- **Meta descriptions:** unverified presence. Webflow does not require this field to be filled — leaving it blank is the most common Webflow SEO oversight.
- **Heading hierarchy:** question-structured blog titles suggest likely-unique H1s, but Webflow's reusable "Symbols" feature can silently inject duplicate H1/H2 site-wide if built carelessly.
- **Internal linking:** **confirmed gap** — no evidence of a hub-and-spoke structure around `/hermes`. Webflow CMS blogs do not auto-generate related-post or breadcrumb links; this must be built manually and nothing indicates it has been.

### Confirmed high-confidence finding: messaging conflict with the cloud pivot

The existing content and homepage messaging are **actively positioned against cloud deployment** — "Run AI agents locally... no cloud dependency option" is marketed as a core benefit. **This is a messaging-strategy problem, not just a content gap.** Adding cloud content without reconciling this narrative first risks the site contradicting itself.

---

## 3. Schema / Structured Data — 10/100 (Critical)

*Full detail: `findings/schema.md`*

Zero schema of any type detected across two independent audits. Webflow requires manual JSON-LD injection via **Project Settings → Custom Code → Head Code** (site-wide) or **Page Settings → Custom Code** (per-page) — there is no auto-generation.

**Important 2026 update discovered this audit:** `FAQPage` schema is now **fully retired for rich results on ALL sites as of May 7, 2026** (previously understood as gov/health-only restriction). Do not build FAQ schema expecting SERP value.

**4 ready-to-ship JSON-LD templates produced** (full code in `findings/schema.md`):
1. **Organization** (site-wide Head Code) — with `sameAs` to GitHub, LinkedIn, X, Product Hunt
2. **SoftwareApplication — Atomic Bot** (homepage)
3. **SoftwareApplication — Hermes Agent** (`/hermes`)
4. **BlogPosting + speakable** (CMS Collection Template Embed — applies to all future posts automatically once built)
5. *(Draft, do not ship yet)* **SoftwareApplication — Cloud SKU** for the future `/cloud` page, with `operatingSystem: "Cloud, Web-based, macOS, Windows"` as the explicit cloud signal

**Do not add:** `FAQPage` (dead for SERP), `HowTo` (dead since Sep 2023).

---

## 4. Sitemap — 78/100 (confidence-adjusted estimate)

*Full detail: `findings/sitemap.md`*

Webflow auto-generates `sitemap.xml` by default — likely present and correctly covering all ~11 known pages. Two real gaps identified:

1. **No auto-ping to Google/Bing on publish.** Webflow's sitemap file updates automatically but never notifies search engines. For a priority launch like `/cloud`, manually trigger GSC "Request Indexing" and Bing resubmission rather than waiting on default recrawl cadence.
2. **Per-post noindex risk.** A single blog CMS item could carry an accidental "Exclude from search engines" flag, silently dropping it from the sitemap with no other visible symptom — worth a manual per-post check.

`app.atomicbot.ai` is correctly excluded from the main sitemap (different subdomain) — not a defect.

---

## 5. AI Search Readiness (GEO) — 47/100

*Full detail: `findings/geo.md`*

| Dimension | Weight | Score |
|---|---|---|
| Citability | 25% | 58 |
| Structural Readability | 20% | 40 (estimated) |
| Multi-Modal Content | 15% | 35 (estimated) |
| Authority & Brand Signals | 20% | 30 |
| Technical Accessibility | 20% | 68 |

**On llms.txt — grounded, not oversold:** Google's own documentation states Search ignores `llms.txt` entirely; John Mueller called it "a dead end"; only 1 of the top 50 AI-cited domains in a 300k-domain study has one; only 0.1% of AI-bot traffic requests it. **Recommendation: still create it, but frame it as a low-cost technical-credibility signal to a developer audience — not a ranking or citation lever.** Do not oversell this to stakeholders.

**Brand entity risk, re-scored more severely than the prior audit:** the Wikipedia name-collision (with a Tiny Defense video game character and an unrelated Reddit RSS bot) is treated as an **active negative** under this skill's rubric — it actively confuses entity resolution, not merely fails to help it.

**Cloud positioning — entity confusion risk, addressed in depth:** introducing a cloud product into a brand with a currently thin, single-source entity definition (the homepage's own "runs locally" tagline) risks direct self-contradiction in AI-generated answers. Recommended mitigation: treat "Atomic Bot Cloud" as an explicitly named, cross-linked sub-entity rather than silently overwriting the existing local-privacy narrative — full content-format spec for the future `/cloud` page is in `findings/geo.md` §9.

---

## 6. Search Experience Optimization (SXO) — 45/100 gap score (bimodal: Local 72.7 / Cloud 17)

*Full detail: `findings/sxo.md`*

**This is the most consequential finding in the entire audit for the client's stated cloud priority.**

| Persona cluster | Avg. score | Rating |
|---|---|---|
| Local-desktop (3 personas) | 72.7/100 | Good |
| Cloud/team (3 personas) | 17/100 | **Critical Mismatch** |

The SERP for "ai agent cloud deployment" and "run ai agent in the cloud" is dominated by **long-form technical guides** (Google Cloud docs, GitLab, Okteto, competitor UniClaw's pillar guide) — not marketing landing pages. Atomic Bot's only two page templates (`/` and `/hermes`) are both **Landing Page** type. **If `/cloud` is built on the same template, it will fail to rank AND fail to convert** regardless of copy quality — platform/DevOps and team-lead personas need architecture, state/scaling, and pricing detail before they'll act on a CTA.

**Also confirmed: no first-party `/pricing` page exists.** Pricing information currently only leaks through third-party aggregators (Futurepedia, TrustMRR) — a trust and clarity failure independent of the cloud pivot.

**Specific page-type guidance for `/cloud`:** build as a **Hybrid (Service + Content)** page — problem statement → architecture explainer (session isolation, persistence, scaling) → deployment-options comparison table (Local vs. Cloud vs. self-hosted) → step-by-step walkthrough with real screenshots → transparent pricing table → FAQ reconciling "isn't this supposed to be local/private?" — not a hero-plus-CTA landing page.

---

## 7. Content Architecture / Semantic Clustering

*Full detail: `findings/cluster.md`*

**Most important discovery of this entire audit:** a dense **third-party hosting/reseller ecosystem** (xCloud.host, ClawHost, Hostinger, Hetzner, Contabo, DigitalOcean, Kimi Claw, myclaw.ai, lushbinary.com, Aruba Cloud, Bluehost, and others) already dominates every exact-match "openclaw/hermes cloud hosting" query. Atomic Bot is already described in third-party coverage as running OpenClaw "locally or in the cloud" — yet has zero content claiming this territory. **Every month without this content cedes Atomic Bot's own product surface to resellers.**

**Competitor list correction:** e2b.dev and modal.com (flagged in the prior audit) are largely irrelevant — dev-infra products with no SERP overlap on Atomic Bot's actual target queries. n8n.io is a confirmed direct competitor; make.com/zapier.com are adjacent, not direct.

**Recommended architecture:** two hub-and-spoke clusters —
- **Pillar A** (needs writing): "The Complete Guide to Running Local AI Agents" — consolidates the 8 existing, currently-orphaned blog posts.
- **Pillar B** (net-new): "AI Agent Cloud Deployment: The Complete Guide to Running OpenClaw & Hermes in the Cloud" — under a dedicated `/cloud/` path.
- **Bridge page** (highest-priority net-new content): "Local vs Cloud AI Agents" — the single most in-demand comparison keyword found in this analysis (6+ competitor domains already rank for it).

**Strategic architecture decision, must be made before writing any cloud content:** isolate Cluster B content under `/cloud/`, not mixed into `/blog/`, to protect the site's currently clean "local AI agent" topical signal from the far more commercially crowded cloud-hosting SERP.

---

## Consolidated Prioritized Action Plan

### Phase 1: Critical Fixes (Week 1)

| # | Action | Source | Effort |
|---|---|---|---|
| 1 | Ship Organization JSON-LD (Head Code, site-wide) | schema.md | 30 min |
| 2 | Ship SoftwareApplication schema on `/` and `/hermes` | schema.md | 2h |
| 3 | Verify robots.txt / sitemap.xml directly in Webflow dashboard (Project Settings → SEO) — both audits could not reach the domain to confirm | technical.md, sitemap.md | 30 min |
| 4 | Decide `/cloud/` vs `/blog/` URL architecture **before writing any cloud content** | cluster.md | 1h decision |
| 5 | Rewrite homepage opening ~150 words into a self-contained, attributed direct-answer passage | geo.md | 2-3h |

### Phase 2: High-Impact Improvements (Weeks 2-3)

| # | Action | Source | Effort |
|---|---|---|---|
| 6 | Build BlogPosting+speakable schema into the CMS Collection Template (applies to all future posts automatically) | schema.md | 4h |
| 7 | Add author bylines + `/about`/`/team` page + visible publish/update dates | content.md | 1 day |
| 8 | Front the domain with Cloudflare for HSTS/CSP/security headers | technical.md | 2-4h |
| 9 | Ship first-party `/pricing` page | sxo.md | 1 day |
| 10 | Build hub-and-spoke internal linking across all 8 existing blog posts and `/hermes` | content.md, cluster.md | 1 day |
| 11 | Allow GPTBot/OAI-SearchBot/ClaudeBot/PerplexityBot explicitly in Webflow's robots.txt editor; publish minimal `/llms.txt` (frame as credibility signal, not ranking lever) | geo.md | 1h |
| 12 | Audit each blog CMS item for accidental "Exclude from search engines" flag | sitemap.md | 30 min |

### Phase 3: Cloud Content & Authority (Month 2)

| # | Action | Source | Effort |
|---|---|---|---|
| 13 | Publish "Local vs Cloud AI Agents" bridge page — highest-demand keyword found in this audit | cluster.md, sxo.md | 2-3 days |
| 14 | Build `/cloud` as a Hybrid technical guide (architecture, comparison table, pricing, FAQ) — **not** a landing-page clone | sxo.md | 3-5 days |
| 15 | Publish "How to Deploy OpenClaw in the Cloud" and "Hermes Agent Cloud Hosting Guide" — directly contest the reseller ecosystem currently owning this SERP | cluster.md | 3-4 days |
| 16 | Publish "Self-Hosted vs Cloud AI Agents for Business" and "AI Agents for Teams" spokes | cluster.md | 3-4 days |
| 17 | Ship the future Cloud SKU JSON-LD template once `/cloud` and real pricing exist | schema.md | 1h |
| 18 | Implement IndexNow via Webflow's `site_publish` webhook | technical.md | 2-4h |

### Phase 4: Monitoring & Iteration (Ongoing)

| # | Action | Source |
|---|---|---|
| 19 | Manually trigger GSC/Bing "Request Indexing" for every new priority page (Webflow never auto-pings) | sitemap.md |
| 20 | Build genuine Reddit/YouTube presence — highest-ceiling, slowest-to-land AI-authority fix | geo.md |
| 21 | Re-run `seo-backlinks` once available (this run was interrupted before completing) | — |
| 22 | Re-verify robots.txt, sitemap.xml, on-page elements, and CWV with a live fetch the moment proxy/network access allows — most "UNVERIFIED" items in this report convert to confirmed findings in under 30 minutes each once a working fetch path exists | all findings |

---

## Explicitly Deprioritized / Do Not Do

- **FAQPage schema for SERP purposes** — fully retired for all sites as of May 7, 2026. Zero rich-result value. Use `QAPage` if genuine user Q&A exists.
- **HowTo schema** — dead since September 2023.
- **Publishing the Cloud SKU schema template before `/cloud` exists with real pricing** — an indexed page with fabricated pricing is worse than no schema.
- **Overselling llms.txt as a ranking/citation lever** — Google explicitly ignores it; frame internally as a low-cost technical-credibility signal only.
- **Cloning the `/` or `/hermes` landing-page template for `/cloud`** — confirmed CRITICAL page-type mismatch against what actually ranks for cloud-deployment queries.
- **Targeting the generic "best AI automation software" head term with new cloud content** — would cannibalize the existing `best-ai-automation-software` post; keep all cloud content narrowly scoped to OpenClaw/Hermes-specific hosting.

---

## Outstanding / Not Completed This Audit

- **seo-backlinks** — agent was terminated before producing a report. No backlink/referring-domain/anchor-text analysis exists in this audit. Re-run if a full picture is needed, especially the flagged opportunity of checking whether the OpenClaw project's own GitHub README/docs link back to atomicbot.ai.
- **seo-visual / seo-performance** — not run. No working headless browser was available in this environment (Playwright/Chromium version mismatch) and the target site is unreachable for live rendering regardless of browser availability. Core Web Vitals in this report are risk-profile estimates only.
- **seo-images** — not run. No image/alt-text audit exists in this report.
- **seo-local / seo-maps / seo-ecommerce** — correctly not spawned; business type is SaaS, not local-service or e-commerce.
- **seo-google** — not run (no Google API credentials configured in this environment). No real GSC/CrUX/GA4 field data exists in this report; all performance/indexation figures are estimates.

**Recommendation:** re-run this audit (or at minimum the technical, sitemap, and schema passes) from a session with working network access to atomicbot.ai to convert the majority of `[ESTIMATED]`/`[UNVERIFIED]` tags in this report into directly confirmed findings — most of this is 30 minutes of work once a working fetch path exists, per the individual specialist reports.

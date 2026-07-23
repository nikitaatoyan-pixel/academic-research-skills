# SXO Audit — atomicbot.ai

**Date:** 2026-07-23
**Scope:** 6 target queries spanning existing local-desktop positioning and the new cloud-deployment growth direction.
**Fetch method:** `render_page.py --mode auto` was attempted on `https://atomicbot.ai` and returned a hard network-layer failure (proxy could not resolve the outbound connection — confirms the stated constraint that direct HTTP fetch to atomicbot.ai is blocked). `WebFetch` to `atomicbot.ai/` and `atomicbot.ai/hermes` returned HTTP 403 (bot protection, likely Webflow-side). All page-content evidence below is therefore reconstructed from Google SERP snippets, third-party summaries/reviews of the site, and the business context supplied in the task. See **Limitations** at the end.

---

## PRIMARY FINDING (lead with this)

atomicbot.ai has **zero indexed pages** addressing cloud-deployment intent (`site:atomicbot.ai` returns only `/`, `/privacy-policy`, `/terms-of-service`, `/blog` + 6 posts, `/hermes`, and the app subdomain — no `/cloud`, no `/pricing`). Meanwhile, the SERP for both new-direction queries ("ai agent cloud deployment," "run ai agent in the cloud") is dominated by **long-form technical guides and infra-vendor blogs** (Google Cloud docs, GitLab, MachineLearningMastery, Okteto, and direct competitor UniClaw's "How to Deploy an AI Agent to the Cloud in 2026" pillar guide) — not short marketing landing pages.

This is a **CRITICAL page-type mismatch risk**, not yet realized but highly likely if the client proceeds naively: the site's two existing templates (`/` and `/hermes`) are both **Landing Page** type (hero + single CTA + minimal nav + "one-click" framing). If `/cloud` is built on the same template, it will structurally mismatch what ranks and converts for cloud-deployment queries. Full detail in Task 6 analysis below.

Secondarily: even where atomicbot.ai's existing page TYPE is correctly aligned with SERP expectations (its OpenClaw install guides, its Hermes-vs-OpenClaw comparison post), **none of these pages appeared in the top links WebSearch returned** for their target queries — a likely authority/depth gap consistent with a 7-month-old domain competing against established docs sites, cloud vendors, and content-marketing-heavy competitors (Zilliz, Composio, Firecrawl, UniClaw).

---

## 1–3. SERP Analysis and Page-Type Mismatch by Query

| # | Query | SERP dominant page type | Confidence | Evidence | atomicbot.ai's matching asset | Mismatch severity |
|---|-------|--------------------------|------------|----------|-------------------------------|--------------------|
| 1 | openclaw installer mac | Blog Post / tutorial (step-by-step, code blocks, system-requirements sections) + Official Docs | ~78% (7/9 results) | Cult of Mac, Kimi, alexhost.com FAQ, Medium, Zilliz blog all use numbered-step "how to install" framing; `docs.openclaw.ai/install` and openclawroadmap.com round out the rest | `/blog/set-up-openclaw-on-mac` — same format (Blog Post) | **ALIGNED (type)** — but page did not surface in top links returned; likely authority/depth gap, not a type problem |
| 2 | how to install openclaw | Blog Post / tutorial + Official Docs | ~75% | ACEMAGIC, Tencent Cloud techpedia, Lark Suite, PromptLayer are all step-by-step guides; `docs.openclaw.ai`, NVIDIA build docs, HuggingFace docs supply the reference-doc portion | `/blog/set-up-openclaw-on-mac`, `/blog/how-to-install-openclaw-on-windows` | **ALIGNED (type)**, same visibility gap as #1 |
| 3 | hermes agent setup | Blog Post / tutorial (step-by-step) + Official Quickstart docs | ~70% | `hermes-agent.nousresearch.com/docs/.../quickstart`, DataCamp tutorial, hermes-agent.ai blog guide, braincuber.com and techjacksolutions.com "setup guide" posts | `/hermes` — this is a **Landing Page** (hero, one-click CTA, feature list, minimal nav), not a procedural tutorial | **CRITICAL** — per taxonomy, "Landing Page targeting how-to keyword" = CRITICAL. Atomic Bot has dedicated OpenClaw setup *blog posts* for Mac/Windows but **no equivalent standalone Hermes setup tutorial** — the gap is structural, not just a missing topic |
| 4 | ai agent cloud deployment | Hybrid / long-form technical guide (architecture explainers, deployment-model comparisons) from cloud vendors + infra blogs | ~80% (8/9 results are Google Cloud docs, Medium/TowardsAI, MachineLearningMastery, GitLab, InfoQ, or UniClaw's pillar guide) | Content covers serverless vs. Kubernetes vs. managed-runtime tradeoffs, CI/CD, session state — deep technical framing, not sales copy | **None exists** | **CRITICAL — total content gap.** No page exists to mismatch yet, but the SERP format (deep technical guide) is the opposite of the site's existing Landing Page template |
| 5 | run ai agent in the cloud | Hybrid / technical tutorial + infra-vendor blog | ~75% | Towards Data Science tutorial, Google Cloud Run docs, Okteto blog (persistent state, session isolation, scaling), TheUnwindAI | **None exists** | **CRITICAL — total content gap**, same profile as #4 |
| 6 | openclaw vs hermes agent | Comparison Page ("vs" framing, feature/architecture tables, verdict/summary section) | ~85% (5/5 top results are comparison articles: MindStudio, Composio, Firecrawl, innFactory, Flowtivity) | All five use side-by-side architecture framing ("control plane" vs. "runtime") and end in a recommendation/verdict | `/blog/hermes-agent-vs-openclaw` — same format (Comparison-style Blog Post) | **ALIGNED (type)** — atomicbot.ai's strongest type-match asset, but again did not appear in the top links returned; competitors likely have deeper tables/more backlinks |

**SERP consensus summary:** Existing-positioning queries (1–3, 6) converge on **Blog Post / tutorial / comparison** formats — informational, procedural, low ad density, dominated by docs sites and content-marketing blogs. Cloud queries (4–5) converge on **Hybrid, long-form technical guide** formats aimed at a technical/DevOps audience, produced by hyperscalers (Google Cloud) and infra-vendors, not by simple SaaS landing pages.

---

## 4. User Story Derivation

Following the local-desktop vs. cloud/team intent split requested, with signal citations.

### Cluster A: Local-Desktop Users (existing positioning)

**Story A1 — Awareness stage.**
As a **non-technical Mac/Windows owner**, I want to install OpenClaw without touching Terminal, because I've heard AI agents can automate my computer but command-line setup intimidates me, but I'm blocked by **technical confusion** about Docker/SSH/API keys.
*Signal: atomicbot.ai's own title "How to install OpenClaw on Windows in 5 minutes (No Command Line)"; third-party guides (ACEMAGIC, alexhost.com) repeatedly foreground ease-of-setup and time-to-first-run as the deciding factor.*

**Story A2 — Decision stage.**
As a **technical evaluator** picking a framework for my team, I want a clear side-by-side of OpenClaw's multi-agent "control plane" model vs. Hermes's single self-improving "runtime," because we need to standardize on one architecture, but I'm blocked by **comparison fatigue** — five competing "vs" articles (MindStudio, Composio, Firecrawl, innFactory, Flowtivity) each frame the tradeoff differently.
*Signal: SERP query #6 — all five top comparison pieces use "control plane" vs. "runtime" framing with a final verdict/recommendation section.*

**Story A3 — Decision stage.**
As a **privacy-focused individual**, I want assurance that my agent's data never leaves my machine, because I don't trust cloud AI vendors with sensitive local files and credentials, but I'm blocked by **trust ambiguity** about whether "free" cloud-adjacent options quietly route through third-party APIs.
*Signal: repeated "runs on your computer... maximum privacy and full control," "no API keys, fully offline" messaging surfacing in search snippets and the GitHub repo description, contrasted with the same summaries mentioning an "encrypted cloud environment" option with no on-site elaboration.*

### Cluster B: Cloud/Team Users (new growth direction)

**Story B1 — Consideration stage.**
As a **platform/DevOps engineer**, I want to deploy my agent so it persists with proper session isolation, state management, and scaling, because ad-hoc local runs aren't reliable for production workloads, but I'm blocked by an **information gap** — nothing on atomicbot.ai explains how its cloud mode handles state, scaling, or uptime.
*Signal: Google Cloud Run docs and Okteto's blog both frame "persistent state, session isolation, scaling, security" as the non-negotiable requirements for agent hosting — none of this vocabulary appears anywhere in atomicbot.ai's current SERP-visible copy.*

**Story B2 — Decision stage.**
As a **team lead**, I want every teammate to reach the same always-on agent without each person installing anything locally, because our workflow needs shared, continuous access, but I'm blocked by **comparison fatigue** against managed competitors like UniClaw, which explicitly promises "fill out a form and click a button" to get a dedicated VM — no local install required.
*Signal: UniClaw's positioning ("managed platform," "dedicated virtual machine," "no laptop required") directly contrasts with atomicbot.ai's homepage title, "The Fastest Way to Run OpenClaw," which is still centered on a desktop install.*

**Story B3 — Decision stage.**
As a **cost-conscious builder**, I want transparent pay-as-you-go pricing for cloud hosting before I commit, because unpredictable AI usage costs are my top fear, but I'm blocked by a **price-sensitivity/trust gap** — pricing details ("$25/month," "pay as you go, top up your balance") only surface through third-party aggregators (Futurepedia, TrustMRR), not on a first-party pricing page (none was found via `site:atomicbot.ai pricing`).
*Signal: absence of any `/pricing` URL in site-restricted search results, combined with third-party sites independently reporting figures the primary site doesn't surface.*

Coverage check: 6 stories, 2 clusters, 3 journey stages (awareness: A1; consideration: A2/B1; decision: A3/B2/B3) — meets the framework's minimum.

---

## 5–6. Persona Scoring (0–25 per dimension, 100 max)

| Persona | Journey stage | Relevance | Clarity | Trust | Action | Total | Rating |
|---|---|---|---|---|---|---|---|
| A. Non-technical Mac/Windows owner (local) | Awareness | 22/25 | 16/25 | 12/25 | 20/25 | **70/100** | Good |
| B. Technical evaluator (OpenClaw vs Hermes) | Decision | 23/25 | 15/25 | 11/25 | 14/25 | **63/100** | Good |
| C. Privacy-focused individual | Decision | 24/25 | 21/25 | 18/25 | 22/25 | **85/100** | Excellent |
| D. Platform/DevOps engineer (cloud) | Consideration | 5/25 | 3/25 | 4/25 | 5/25 | **17/100** | Critical Mismatch |
| E. Team lead needing shared/always-on access (cloud) | Decision | 4/25 | 3/25 | 3/25 | 4/25 | **14/100** | Critical Mismatch |
| F. Cost-conscious builder (cloud pricing) | Decision | 6/25 | 4/25 | 5/25 | 5/25 | **20/100** | Critical Mismatch |

**SXO Gap Score (separate from SEO Health Score): 45/100 — Needs Work overall**, but the number hides a sharp bimodal split:
- Local-cluster average (A/B/C): **72.7/100 — Good**
- Cloud-cluster average (D/E/F): **17/100 — Critical Mismatch**

### Weakest persona: E — Team lead needing shared/always-on access (14/100)
**Top issue:** No content anywhere on the site addresses multi-user/team access, managed hosting, or "no local install" convenience — the entire site architecture (both `/` and `/hermes`) assumes a single individual installing on their own device.
**Recommended fix:** Before writing marketing copy, decide the actual product mechanic for team access (shared workspace? per-seat cloud instance? SSO?) — then build a page around that mechanic, framed against the "click a button, get a dedicated environment" pattern that UniClaw already validated in this SERP.

### Systemic issues (span all personas)
- **Trust dimension is the weakest across the board** (11–18/25 even for well-served personas A/B/C): no visible bylines/author credentials, no case studies, no third-party security/audit signals — consistent with a 7-month-old domain that hasn't yet accumulated E-E-A-T signals.
- **Clarity collapses to near-zero the moment intent shifts to cloud** (3/25 for D and E) — not a wording problem, a total-absence problem.

### Priority actions (weakest-first)
1. Address Persona D/E/F (cloud cluster) — see Top 3 Priority Fixes below.
2. Fix the systemic Trust gap — add author identity, credentials, and case studies to existing local-cluster content (Persona B's Trust score, 11/25, is the single lowest score among "aligned" pages).
3. Address Persona A's Clarity (16/25) — the install tutorials exist but aren't winning the SERP; likely needs more scannable structure (numbered steps, screenshots, TOC) to match Zilliz/Cult of Mac-caliber competitors.

---

## Top 3 Priority Fixes

### 1. Build `/cloud` as a Hybrid technical guide, not a Landing Page clone — highest risk item
**The specific risk requested in this task:** if Atomic Bot ships a `/cloud` page using the same template as `/` and `/hermes` (hero banner + one CTA + 3-5 marketing bullets), it will fail on two fronts simultaneously:
- **It will not rank.** The SERP for "ai agent cloud deployment" and "run ai agent in the cloud" is won by long-form technical guides (Google Cloud docs, GitLab engineering blog, MachineLearningMastery's architecture roadmap, Okteto, and direct competitor UniClaw's dedicated deployment pillar page) — none of which resemble a SaaS marketing landing page. Per the taxonomy, this is the "Landing Page without educational depth" mismatch pattern (severity: MEDIUM–HIGH at minimum, and functionally CRITICAL given how technical this specific SERP is).
- **It will not convert.** Personas D (platform engineer) and E (team lead) both need architecture/state/scaling answers or a "click a button, get a dedicated environment" mechanic *before* they'll trust a CTA — a thin page gives them nothing to evaluate.

**Concrete guidance for the page TYPE (not just content):** Build `/cloud` as a **Hybrid (Service + Content)** page structured like the winning SERP results:
- Problem statement: why local-only agents break down for teams/production (state loss, no uptime, single-machine risk)
- "How Atomic Bot cloud mode works" — architecture section addressing session isolation, persistence, scaling (directly answers Story B1)
- A deployment-options comparison table: Local (existing) vs. Atomic Bot Cloud vs. self-hosted VM — mirroring the self-hosted-vs-managed framing that UniClaw and Google Cloud docs both use
- Step-by-step "get started in the cloud" walkthrough with real UI screenshots (not just a "one-click" claim — show the click)
- Transparent, on-page pay-as-you-go pricing table (fixes Story B3 and Persona F simultaneously)
- FAQ addressing the trust reconciliation question this pivot creates: "If Atomic Bot is about local privacy, is the cloud mode still private?" — this must be answered explicitly or the pivot will confuse and erode trust with the existing local-privacy audience (Persona C, currently the best-served persona at 85/100)
- CTA tiering: a "Deploy Now" CTA for ready buyers alongside a lower-friction "See how cloud mode works" anchor link for evaluators still reading

### 2. Ship a first-party `/pricing` page
Currently pricing information only surfaces through third-party aggregators (Futurepedia citing "$25/month," TrustMRR citing revenue figures) — confirmed absent from `site:atomicbot.ai` results. This is both a Clarity and Trust failure for Persona F and undermines any cloud pivot, since cloud buyers (Personas D/E/F) specifically need to evaluate pay-as-you-go economics before committing. Include: free-local tier, paid-convenience tier (no manual API keys), and cloud pay-as-you-go tier, side by side.

### 3. Reinforce authority on the already-aligned pages (OpenClaw Mac/Windows guides, Hermes-vs-OpenClaw comparison)
These pages have the *correct* page type for their target queries but did not appear in the top links returned by search for any of queries 1, 2, 3, or 6 — pointing to an authority/depth gap rather than a type problem. Add author bylines and credentials, expand step depth with real screenshots, add a structured comparison table with schema markup to the Hermes-vs-OpenClaw post, and pursue backlinks/citations. Recommend `/seo content` for a full E-E-A-T deep dive and `/seo schema` to generate Article/BlogPosting/comparison-table schema, since none was observable in the current SERP snippets.

---

## Cross-Skill References
- **E-E-A-T / authority gap** (Personas A, B, C Trust scores all below 20/25; young 7-month domain) → recommend `/seo content` for a deep author-authority and content-depth analysis.
- **Missing schema signals** on comparison and tutorial pages (no Comparison/Table or Article/BlogPosting schema observable) → recommend `/seo schema`.
- **No dedicated `/pricing` and no `/cloud` page** (thin content risk for the entire new business direction) → recommend `/seo page` for a page-level content-depth audit once `/cloud` is drafted, before publishing.

---

## Limitations

- Direct HTTP fetch to atomicbot.ai via `render_page.py --mode auto` failed at the network layer (proxy DNS-resolution refusal), confirming the stated constraint. `WebFetch` to `atomicbot.ai/` and `/hermes` also returned HTTP 403. **No page in this audit was directly rendered or parsed** — all target-site evidence is reconstructed from Google SERP snippets, AI-summarized third-party reviews/aggregators (Futurepedia, TrustMRR, theresanaiforthat, GitHub descriptions), and the business context supplied in the task. Treat page-type classifications for atomicbot.ai's own pages as high-confidence inferences, not direct DOM observations.
- `WebSearch` returns AI-summarized aggregations of a limited result set (typically 6-9 links), not raw SERP HTML — People Also Ask boxes, ad copy, featured-snippet formatting, and AI Overview citations **could not be directly observed** for any of the 6 queries. SERP-feature signals in this report (PAA-like themes, "vs" framing, etc.) are inferred from result titles/summaries only, per the user-story-framework's signal sources, not confirmed SERP feature presence.
- Search results are time- and geography-sensitive; this audit reflects a single WebSearch snapshot taken 2026-07-23 and may not match what a different searcher sees.
- Domain age (7 months) and lack of first-party pricing/cloud pages are treated as explanatory factors for low rankings and low Trust/Action scores, but backlink profile, Core Web Vitals, and actual on-page word counts were not measured — only inferred from third-party summaries.
- Persona scores are directional (evidence-based estimates), not derived from analytics, heatmaps, or user testing.

---

Generate a PDF report? Use `/seo google report`

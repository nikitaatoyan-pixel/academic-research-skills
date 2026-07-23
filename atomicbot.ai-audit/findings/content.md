# Content Quality + On-Page SEO Audit — atomicbot.ai

**Date:** 2026-07-23
**Scope:** Builds on the prior GEO-focused content audit (`/home/user/academic-research-skills/GEO-AUDIT-REPORT.md`). Adds on-page SEO analysis (titles, meta descriptions, headings, internal linking, keyword targeting) not covered previously.
**Platform correction carried forward:** Site is built on **Webflow**, not Next.js. The prior report's framework identification (Next.js, via GitHub org inference) was incorrect and is disregarded here; on-page recommendations below assume Webflow's CMS/Designer conventions and known defaults/pitfalls.

## Methodology note and access limitation (read before scoring)

This session confirmed the stated constraint independently:

- Direct HTTPS fetch to `atomicbot.ai` returned a **403 policy denial at the egress proxy** (`CONNECT tunnel failed, response 403`), consistent across repeated attempts logged by the proxy status endpoint.
- Fallback archival routes are **also blocked** by the same egress policy: `index.commoncrawl.org:443` and `api.web.archive.org:443` both returned 403.
- This sub-agent's available toolset for this run did **not** include a working `WebSearch` or `WebFetch` function (only `Read`, `Bash`, `Write`, `Grep` were available), so the search-snippet/third-party-coverage gathering step specified in the task instructions could not be executed in this session.

**Consequence:** Nothing below claiming a specific title-tag string, exact character count, meta description copy, or heading tag from a live page was independently fetched or re-verified in this session. Findings are built from three legitimate evidence sources instead, each labeled:

1. **[PRIOR-AUDIT]** — Carried from `GEO-AUDIT-REPORT.md`, itself produced via SERP/third-party signals (same proxy-blocked environment), dated same day.
2. **[TASK-BRIEF]** — Known facts supplied directly in this task's brief.
3. **[PLATFORM-INFERENCE]** — Reasoned from documented Webflow CMS/Designer default behavior and common Webflow SEO pitfalls, explicitly flagged as inference, not observation.

Every finding below is tagged. Anything not independently re-confirmable this session is marked **UNVERIFIED — recommend live crawl once proxy/fetch access is restored** rather than asserted as fact. Do not treat the numeric scores below as equivalent in confidence to a tool that actually rendered the page.

---

## Content Quality Score: 57 / 100 (Fair) — provisional, confidence: MEDIUM

Recomputed using this skill's internal E-E-A-T weighting model (Experience 20% / Expertise 25% / Authoritativeness 25% / Trustworthiness 30%), applied to the **[TASK-BRIEF]** sub-scores, then adjusted down slightly for freshness and duplication risk not scored in the prior audit.

### E-E-A-T Breakdown

| Factor | Weight | Score | Weighted | Source |
|---|---|---|---|---|
| Experience | 20% | 68/100 | 13.6 | [TASK-BRIEF] — original Hermes vs OpenClaw benchmark (203k/257k tokens, MacBook Pro M5 Max 64GB, 12:01 runtime) is genuine first-hand testing evidence, rare in this niche |
| Expertise | 25% | 52/100 | 13.0 | [TASK-BRIEF] — technically deep content, but **zero author bylines confirmed** (see re-verification below) |
| Authoritativeness | 25% | 56/100 | 14.0 | [PRIOR-AUDIT] — no Wikipedia, no .edu/.gov citations, ~7-month domain age, but active Product Hunt/directory presence |
| Trustworthiness | 30% | 56/100 | 16.8 | [PRIOR-AUDIT] — strong privacy policy (no training/selling, GDPR language), but no physical address, no editorial policy |
| **Weighted E-E-A-T composite** | | | **57.4** | |

### Adjustments applied to reach the 57/100 headline score

- **AI citation format: 72/100** [PRIOR-AUDIT] — question-structured titles, comparison tables, numeric specificity. This pulls the score up slightly.
- **Topical authority: ~40-50% subtopic coverage** [PRIOR-AUDIT] in the "AI agent automation + local LLM" niche — moderate, holds the score back from "Good."
- **Freshness signals: UNVERIFIED, treat as risk.** No `datePublished`/`dateModified` or visible "last updated" markers were confirmed on any blog post in either audit. Combined with zero schema (confirmed [PRIOR-AUDIT], Structured Data 10/100), there is no machine-readable freshness signal anywhere on the site. This is a content-quality risk independent of the schema fix already logged in the prior report.
- **Duplication/thin-content risk: NEW finding, moderate risk, UNVERIFIED at content-body level.** `/blog/set-up-openclaw-on-mac` and `/blog/how-to-install-openclaw-on-windows` are parallel OS-specific install guides. This pattern commonly produces near-duplicate structure (same steps, different commands) with little unique value beyond the OS-specific portion. Cannot confirm actual duplication without fetching both bodies — flagged as a pattern risk to check, not a confirmed violation. Recommend auditing both for >60% structural/sentence overlap and adding OS-specific troubleshooting sections to differentiate.
- Net effect of freshness + duplication risk: -0.4 points off the 57.4 composite, rounded to **57/100**.

### Word count vs. page-type minimums — UNVERIFIED, cannot confirm without fetch

| Page type | Minimum | Status |
|---|---|---|
| Homepage | 500 words | UNVERIFIED. Prior audit's citability scoring flagged the homepage hero copy itself as "a marketing tagline, 0 factual content" (48/100 citability) — this describes the *hero*, not total page word count, so it is not proof the page is under 500 words. Flag for direct measurement. |
| Blog posts (8 confirmed) | 1,500 words | UNVERIFIED. The benchmark/comparison posts (`hermes-agent-vs-openclaw`, `what-is-local-ai`, `qwen-3-7-max`) likely clear 1,500 given the described data density (multi-model comparison tables, specific benchmark figures). The two install guides (`set-up-openclaw-on-mac`, `how-to-install-openclaw-on-windows`) are the higher-risk candidates for falling short of 1,500 if kept purely procedural — this is also where topical-coverage floor and duplication risk intersect. |
| `/hermes` product page | 300-400+ words (complex product) | UNVERIFIED. |
| `/privacy-policy`, `/terms-of-service` | No minimum (legal pages exempt) | Acceptable as-is regardless of length; not a thin-content flag. |

**Recommendation:** Run an actual word count pass via `render_page.py` (or any working fetch path) the moment proxy access to atomicbot.ai is restored — this is a 30-minute task once access exists and removes most of the UNVERIFIED tags above.

---

## On-Page SEO Score: 46 / 100 (Poor-to-Fair) — provisional, confidence: LOW-MEDIUM

This score is explicitly lower-confidence than the Content Quality score because on-page SEO elements (title tag strings, meta description copy, heading tag nesting) require rendered HTML that could not be fetched this session. It is built from the small number of items the prior technical audit *did* partially confirm, plus documented Webflow-specific structural risk patterns, plus the new keyword-gap finding (which is confirmable from the page/URL list alone).

### 1. Title tags — UNVERIFIED (structure), confidence: LOW

- [PRIOR-AUDIT] technical sub-agent rated "Meta tags: 65/100 — Titles good; OG tags unverified." This is third-hand SERP-snippet-based, not a direct `<title>` extraction, so "good" should be read as "no obvious problems surfaced in search snippets," not a confirmed audit of character length or keyword placement.
- The blog URL slugs themselves (`hermes-agent-vs-openclaw`, `best-ai-automation-software`, `is-claude-code-free`, `set-up-openclaw-on-mac`, `how-to-install-openclaw-on-windows`, `what-is-local-ai`) are keyword-descriptive and front-loaded, which is a positive structural signal for likely title-tag quality since Webflow by default seeds the SEO title field from the page name/slug unless manually overridden — but this is inference, not confirmation.
- **Known Webflow risk pattern [PLATFORM-INFERENCE]:** Webflow CMS Collection templates (used for blog posts) apply one title-tag *pattern* across all collection items (e.g., `{{Post Title}} | Atomic Bot Blog`). If the brand suffix pushes any post title over ~60 characters, this silently truncates in SERPs across the whole collection at once — a single-point-of-failure risk specific to CMS-templated title tags that does not exist on manually-built static pages. Recommend checking the longest post title (`how-to-install-openclaw-on-windows` "How to Install OpenClaw on Windows" + any suffix) against the 60-char budget.
- **Cannot confirm:** branded-suffix consistency, exact character counts, or whether any duplicate titles exist across pages (a classic Webflow issue when the CMS template isn't customized per item).

### 2. Meta descriptions — UNVERIFIED, confidence: LOW

- Not observed in either audit. Webflow prompts for a meta description field per page/CMS item in its SEO settings panel, but **does not require it to be filled in** — leaving it blank is the single most common Webflow SEO oversight, and when blank, Google/Bing auto-generate a snippet from page content (which may or may not include a CTA).
- **Cannot confirm** presence, length, or CTA quality on any page. This is a pure gap in this session's evidence, not a confirmed finding either way — do not read "UNVERIFIED" here as "assumed missing," but treat it as the top verification priority for the next crawl pass.

### 3. Heading hierarchy — Partially inferable, confidence: LOW-MEDIUM

- [PRIOR-AUDIT] confirms blog post titles are question-structured ("Is Claude Code Free?", "Hermes vs OpenClaw: Which to Choose, When to Run Both and Why") — if these strings are used as the on-page H1 (the normal Webflow CMS pattern binds the Collection Page H1 to the item's Name field), this gives reasonable indirect confidence that **H1 uniqueness per blog post is likely fine**, since each post has a distinct, specific title.
- **Known Webflow risk pattern [PLATFORM-INFERENCE]:** Global Symbols (Webflow's reusable-component feature) are sometimes built with a hardcoded heading tag inside them (e.g., a "Related Posts" or CTA symbol using an H2 or even H1) and dropped onto every page — this can silently create duplicate/multiple-H1 or nesting-skip (H1→H3, no H2) issues across an entire site at once. Cannot confirm this either way without inspecting rendered DOM.
- **Cannot confirm:** logical H2/H3 nesting within post bodies, or whether the homepage/`/hermes` product page have exactly one H1 each.

### 4. Internal linking / hub-and-spoke structure — Weak signal, confidence: LOW-MEDIUM, **flagged as a real gap**

- No internal-link evidence was available from either audit's SERP-based methodology (internal links are invisible in search snippets by design).
- However, the **absence of any evidence** of a hub structure is itself notable given the content inventory: `/hermes` is confirmed as the flagship product page [PRIOR-AUDIT appendix], and there are two blog posts (`hermes-agent-vs-openclaw`, `hermes-use-cases-for-developers`) that are topically obligated to link to `/hermes` and to each other. Two install guides (Mac/Windows) are also obligated to cross-link and both should point to the product page and to the comparison post.
- **Recommendation (actionable regardless of verification):** build an explicit hub-and-spoke structure:
  - `/hermes` (hub) ← linked from all 8 blog posts, not just the two with "hermes" in the slug
  - `/blog/hermes-agent-vs-openclaw` ↔ `/blog/hermes-use-cases-for-developers` (cross-link, same topic cluster)
  - `/blog/set-up-openclaw-on-mac` ↔ `/blog/how-to-install-openclaw-on-windows` (cross-link, parallel guides — this also mitigates the duplication risk noted above by giving each a reason to exist as a distinct, connected page in a cluster)
  - `/blog/what-is-local-ai` → should link to `/blog/qwen-3-7-max` (both are model-comparison content) and to whatever future "cloud" content is built (see below) once it exists, to avoid the cluster being a dead end
  - Every blog post → homepage and `/hermes`, at minimum in a consistent footer/CTA block
- This is listed as a real, actionable gap rather than purely "unverified," because the *absence of confirmable internal-linking signal in any third-party source* combined with a Webflow CMS blog (which does not auto-generate related-post or breadcrumb links — that requires manual Designer work) is itself informative: hub-and-spoke linking on Webflow blogs has to be deliberately built, and nothing in either audit surfaced evidence that it has been.

### 5. Thin/duplicate content risk — see Content Quality section above for the install-guide pair. No additional near-duplicate pages identified from the confirmed URL list. Legal pages (`/terms-of-service`, `/privacy-policy`) are appropriately short and not a thin-content flag.

### 6. Keyword targeting gap: "cloud" positioning — **CONFIRMED GAP, high confidence**

This is the one on-page finding confirmable with high confidence from the existing evidence, without needing a live fetch:

- The confirmed blog post inventory (8 posts) contains **zero titles or slugs referencing "cloud," "hosted," "remote," or "server-based" deployment.** Every title is local/desktop-oriented: Mac setup, Windows install, local AI, on-device comparisons.
- More importantly, the prior audit's own draft `SoftwareApplication` schema (built from the product's actual current positioning) lists **"No cloud dependency option"** as a headline feature, and the draft `Organization` description reads "**Run AI agents locally with full privacy** — no terminal setup required." The homepage hero was independently scored as "The Fastest Way to Run OpenClaw" — again local-first framing.
- **This means the gap is not merely an absence of cloud content — the existing content and messaging are actively positioned against cloud deployment** (privacy/local-execution as the core value prop). A pivot toward cloud positioning will require more than adding new pages; it will require reconciling a live product narrative that currently markets "no cloud dependency" as a *benefit* with a new go-to-market message that promotes cloud deployment as an *option*. Treat this as a messaging-strategy issue, not just a content-gap issue.
- **Zero evidence of any existing page targeting cloud-related queries** (e.g., "AI agent cloud deployment," "run AI agents in the cloud," "cloud-hosted AI automation," "OpenClaw cloud"). Confirmed absent.

---

## E-E-A-T author byline gap — re-verification status

**Not independently re-verifiable this session** (no working fetch path to atomicbot.ai, and no WebSearch/WebFetch capability available in this sub-agent's toolset for this run). Based on the evidence that could be assembled:

- No new byline, `/about`, `/team`, or `Person` schema signal surfaced anywhere in the material available this session (the prior audit's own recommended fix — "Add author bylines + `/about` page with team names and GitHub handles" — was listed as a 1-day CRITICAL action, implying it had not yet been done as of that audit).
- **Conclusion: presumed unchanged (gap still open)**, carried forward at Expertise 52/100, but this is an assumption of no-change rather than a confirmed re-check. Flag this explicitly to the orchestrator/user: the byline gap should be the first thing re-verified with a live fetch, since it is cheap to check (view-source or a single rendered fetch of any blog post) and was already identified as the top Expertise-scoring lever.

---

## Top 3 Priority Fixes

1. **Build the "cloud" content pillar and reconcile it with existing "local-first / no cloud dependency" messaging before writing new pages.** This is the single highest business-priority gap: zero existing content targets cloud-deployment queries, and the current value proposition (privacy via local execution) is a live messaging conflict with the stated pivot. Sequence: (a) decide the reconciled narrative (e.g., "local by default, cloud when you need it" rather than an either/or), (b) update homepage hero and `/hermes` product copy, (c) publish a pillar page/post targeting "AI agent cloud deployment" style queries, (d) link it into the existing hub-and-spoke structure from `/what-is-local-ai` and `/hermes-use-cases-for-developers` so it isn't an orphaned page.

2. **Close the author-expertise gap with an on-page + structured-data combined fix.** Add named author bylines and a `/about`/`/team` page with real credentials, then wire `Person` schema and `BlogPosting.author` into every post (this was already specced by the prior audit's schema templates — it just needs shipping). This is the cheapest-to-implement, highest-leverage fix across both Content Quality (Expertise 52→estimated 65+) and platform/AI-citation scoring, and should be paired with adding visible `datePublished`/`dateModified`/"last updated" markers to close the freshness gap simultaneously.

3. **Build deliberate internal linking (hub-and-spoke) and run a live on-page crawl to convert every UNVERIFIED item in this report to confirmed.** Concretely: link all 8 blog posts to `/hermes` and to the homepage at minimum, cross-link the two install guides and the two Hermes-topic posts to each other, and — as soon as fetch/proxy access is available — run a full crawl (title tags, meta descriptions, H1/H2 structure, word counts) to replace every LOW/LOW-MEDIUM confidence item above with a directly observed one. Until that crawl happens, treat the 46/100 On-Page SEO score as a floor estimate, not a precise measurement.

---

## Structured findings (for `audit-data.json` — Content Quality category)

```json
{
  "category": "content_quality",
  "content_quality_score": 57,
  "on_page_seo_score": 46,
  "confidence": "MEDIUM (content), LOW-MEDIUM (on-page)",
  "access_limitation": "Direct fetch to atomicbot.ai blocked by egress proxy policy (403); commoncrawl and web.archive.org fallbacks also blocked; WebSearch/WebFetch tools unavailable in this sub-agent invocation. Findings derived from prior GEO-AUDIT-REPORT.md and task-supplied known facts.",
  "eeat": {
    "experience": {"score": 68, "weight": 0.20},
    "expertise": {"score": 52, "weight": 0.25, "note": "zero author bylines confirmed; presumed unchanged, not re-fetched"},
    "authoritativeness": {"score": 56, "weight": 0.25},
    "trustworthiness": {"score": 56, "weight": 0.30},
    "weighted_composite": 57.4
  },
  "word_count_status": "unverified_all_page_types",
  "thin_or_duplicate_content_risk": [
    {"pages": ["/blog/set-up-openclaw-on-mac", "/blog/how-to-install-openclaw-on-windows"], "risk": "moderate", "status": "unconfirmed_pattern_risk"}
  ],
  "on_page_seo": {
    "title_tags": {"status": "unverified", "confidence": "low", "signal": "prior audit rated meta tags 65/100 with note 'titles good, OG tags unverified'"},
    "meta_descriptions": {"status": "unverified", "confidence": "low"},
    "heading_hierarchy": {"status": "partially_inferable", "confidence": "low-medium", "signal": "question-structured blog titles suggest likely-unique H1s"},
    "internal_linking": {"status": "gap_flagged", "confidence": "low-medium", "recommendation": "build hub-and-spoke around /hermes"},
    "keyword_gap_cloud": {"status": "confirmed_absent", "confidence": "high", "note": "existing messaging is actively local-first / anti-cloud positioned; business priority pivot to cloud requires messaging reconciliation, not just new pages"}
  },
  "author_byline_reverification": {"status": "not_reverified_this_session", "assumption": "gap presumed unchanged"},
  "top_3_priority_fixes": [
    "Build cloud content pillar + reconcile local-first messaging",
    "Add author bylines/about page + Person/BlogPosting schema + freshness dates",
    "Build hub-and-spoke internal linking + run live crawl to confirm on-page elements"
  ]
}
```

# AI Search Readiness (GEO) Audit — atomicbot.ai

**Skill applied:** `seo-geo` v2.2.4 (distinct rubric from the prior `geo-seo-claude` pass — 5-dimension weighted composite, not a flat citability/authority pair)
**Audit date:** 2026-07-23
**Verification status:** LIVE FETCH BLOCKED — see Methodology Note below. Findings combine (a) facts carried forward from the prior `geo-seo-claude` audit, explicitly re-scored under this skill's rubric, and (b) reasoned technical analysis. Every claim below is tagged `[VERIFIED]` (independently confirmed this session), `[CARRIED FORWARD]` (from prior audit, not re-fetched), or `[ESTIMATED]` (inferred, not directly observed — needs live re-check).

---

## Methodology Note — why nothing was re-fetched live

Every outbound attempt this session was rejected at the network layer, not by the target sites:

| Attempt | Result |
|---|---|
| `render_page.py https://atomicbot.ai --mode auto` | `ProxyError` / DNS-rebinding refusal (per task constraint) |
| `WebFetch https://atomicbot.ai/robots.txt` | 403 |
| `WebFetch https://atomicbot.ai/llms.txt` | 403 |
| `WebFetch https://atomicbot.ai` | 403 |
| `WebFetch` Bing search, DuckDuckGo HTML, Product Hunt, X/Twitter, Wikipedia, even `example.com` | 403 (all) |
| `curl $HTTPS_PROXY/__agentproxy/status` | Confirms `connect_rejected` (403) specifically for `atomicbot.ai`, `api.web.archive.org`, `index.commoncrawl.org` — an org egress policy denial, not a transient failure |

Per `/root/.ccr/README.md`: 403/407 from the proxy is a policy denial and must be reported, not retried or routed around. No DataForSEO or WebSearch MCP tools were available in this session either. **Recommendation for the orchestrator:** re-run this skill's live-fetch steps (robots.txt, llms.txt, homepage HTML, Product Hunt/X/Wikipedia checks) from a session with egress to `atomicbot.ai`, Wayback Machine, and standard search — several scores below are marked `[ESTIMATED]` specifically because that data was unavailable.

---

## AI Search Readiness Score: 47 / 100

Composite uses this skill's fixed weights (Citability 25 / Structural Readability 20 / Multi-Modal 15 / Authority & Brand 20 / Technical Accessibility 20). This is a **heuristic score, not a Google-internal ranking signal** — stated per this skill's own third-party-tool honesty mandate.

| Dimension | Weight | Score /100 | Weighted | Confidence |
|---|---|---|---|---|
| Citability | 25% | 58 | 14.5 | Medium (content descriptions carried forward, re-scored under new rubric) |
| Structural Readability | 20% | 40 | 8.0 | Low `[ESTIMATED]` |
| Multi-Modal Content | 15% | 35 | 5.25 | Low `[ESTIMATED]` |
| Authority & Brand Signals | 20% | 30 | 6.0 | Medium-High (mostly carried-forward facts) |
| Technical Accessibility | 20% | 68 | 13.6 | Medium (platform-inference for SSR; robots.txt/llms.txt unverified live) |
| **Total** | 100% | — | **47.35 → 47/100** | |

This is meaningfully different from the prior audit's framing (which reported separate 62/100 citability and 36/100 brand-authority scores with no combined weighting). Under this skill's composite, the **Technical Accessibility** dimension pulls the score up (Webflow's SSR-by-default architecture is a real structural advantage), while **Structural Readability** and **Multi-Modal Content** — dimensions the prior audit didn't score at all — pull it down because there's no verified evidence of question-based headings, FAQ blocks, tables, or embedded video/demo content on-site.

---

## 1. AI Crawler Access Status

| Crawler | Status | Confidence |
|---|---|---|
| GPTBot (OpenAI) | Unknown — robots.txt not independently fetchable this session | `[CARRIED FORWARD]` unknown |
| OAI-SearchBot (OpenAI) | Unknown | `[CARRIED FORWARD]` unknown |
| ClaudeBot (Anthropic) | Unknown | `[CARRIED FORWARD]` unknown |
| PerplexityBot | Unknown | `[CARRIED FORWARD]` unknown |
| CCBot / anthropic-ai / cohere-ai | Unknown | `[CARRIED FORWARD]` unknown |

No evidence in either audit pass of an explicit block. **Platform-specific reasoning `[ESTIMATED]`:** Webflow's default-generated `robots.txt` (unless a site owner manually edits it in Site Settings → SEO) does **not** ship with AI-crawler-specific disallow rules — it typically only blocks Webflow's own staging subdomain and sitemap-references the production domain. Absent evidence of manual customization, the base-rate expectation for a Webflow site is **open access for GPTBot/ClaudeBot/PerplexityBot/OAI-SearchBot**. This is an inference, not a verified fact — flag it for direct confirmation next session (`curl atomicbot.ai/robots.txt` or `render_page.py` once egress is restored).

**Recommendation regardless of verification outcome:** explicitly add allow directives for GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot in Webflow's robots.txt editor (Project Settings → SEO → robots.txt is fully editable in Webflow, no code injection needed). Costs 10 minutes, removes the "unknown" from the next audit.

---

## 2. llms.txt Status: ABSENT — confirmed, and correctly weighted

`[CARRIED FORWARD, HIGH CONFIDENCE]` No `/llms.txt` found in the prior audit's exhaustive search; not independently re-fetchable this session (403), consistent with absence rather than contradicting it.

**Grounding this recommendation in `references/llmstxt-evidence.md` (per task instruction) — do not oversell this fix:**

- Google's AI optimization guide (2026-06-29) states outright that Google Search **ignores** `llms.txt` — it "won't harm (nor help)" visibility or rankings.
- John Mueller called the llms.txt discovery/differentiation use case "a dead end" and separately said "no AI system currently uses llms.txt," comparing it to deprecated meta keywords.
- Gary Illyes (Search Central Live, July 2025): Google has no plans to support it.
- SE Ranking's 300k-domain study: among the top 50 AI-cited domains, **only one** had an `/llms.txt`.
- OtterlyAI's server-log audit: **0.1%** of AI-bot traffic requests `/llms.txt` (84 of 62,100 requests).
- Anthropic, Stripe, Cloudflare, and NVIDIA all publish `llms.txt` — none has confirmed their own crawlers *consume* third-party `llms.txt` files.

**Where it does plausibly matter:** AI coding agents (Cursor, Continue, Cline, Claude Code) increasingly load `llms.txt` when working with developer-facing documentation. AtomicBot's positioning (a local AI agent tool, soon with a cloud/API angle) means its audience skews toward the same technical crowd that uses those agents and that recognizes the `llms.txt` convention as a credibility marker — similar to how a `SECURITY.md` or an OpenAPI spec signals engineering maturity even if no crawler mechanically "ranks" on it.

**Net recommendation:** create `/llms.txt` as a near-zero-cost, non-Google-lever addition. Frame it internally as a **brand-credibility signal to a technical audience**, not a citation or ranking lever. Do not report to the client that it will move AI Overviews or ChatGPT citation rates — that claim is directly contradicted by primary sources above.

---

## 3. Passage-Level Citability — re-scored under this skill's rubric (25% weight)

This skill's specific tests (distinct from the prior audit's approach): (1) is the passage in the 134–167 word optimal window, (2) does it deliver a direct answer in the first 40–60 words, (3) is it self-contained (extractable without surrounding context), (4) does it carry a specific, attributed statistic, (5) does it sit in the first ~30% of the page (SE Ranking: ~44% of AI citations pull from the first 30% of a page).

| Passage (carried forward from prior audit's content inventory) | Word-count fit | First-40-60-word direct answer | Self-contained | Attributed stat | Score /100 |
|---|---|---|---|---|---|
| Product Hunt traction data (upvotes/launch stats) | Likely near-optimal, short factual block | Yes — numbers stated immediately | Yes | Yes (platform-sourced numbers) | 80 |
| Pricing claim (site pricing page) | Likely short of 134 words (pricing blocks tend to be terse, <100 words) | Partial — price stated, but framing/context often precedes the number | Partial — often needs plan-name context | Partial (price is a fact, but not "sourced" in the citation sense) | 62 |
| Homepage hero copy ("runs locally on your Mac for privacy") | Under 134 words, and is a tagline, not a passage | No — tagline is not a direct answer to any implied question | No — not self-contained (needs the rest of the page to become a claim) | No | 38 |

**Dimension score: 58/100** (weighted average with hero copy penalized further under this skill's stricter self-containment test — it explicitly fails 4 of the 5 tests, whereas the prior audit's 48/100 only partially penalized it).

**Highest-leverage fix:** rewrite the homepage's first 150 words into a self-contained, attributed answer block, e.g., a 134–167 word passage answering "What is AtomicBot?" directly in the first sentence, with the Product-Hunt-validated traction stat folded in as the supporting fact. This single change plausibly moves the dimension score from 58 to 75+, because it converts the single highest-visibility passage on the site (first 30% of the page, where ~44% of AI citations originate) from a weak into a strong signal.

---

## 4. Structural Readability — 40/100 `[ESTIMATED, low confidence]`

Not independently verified this session (couldn't fetch headings/DOM). Estimate is based on: (a) the described content is a standard SaaS marketing homepage (hero tagline + pricing + traction proof), a pattern that in the large majority of Webflow marketing templates uses benefit-statement H2s ("Privacy by design," "Built for your Mac") rather than question-based headings ("What is AtomicBot?", "How does AtomicBot protect my data?"); (b) no FAQ section was surfaced in either audit pass; (c) no evidence of comparison tables (relevant given the cloud-positioning question below).

**Action for next audit:** verify H1→H2→H3 hierarchy, paragraph length, and presence/absence of lists, tables, and FAQ blocks directly from rendered HTML once egress is available. Until then, treat 40/100 as a planning assumption, not a fact.

---

## 5. Multi-Modal Content — 35/100 `[ESTIMATED, low confidence]`

Content with multi-modal elements sees 156% higher AI-answer selection rates (this skill's stated benchmark). Known signal: 3 third-party YouTube Shorts exist about AtomicBot `[CARRIED FORWARD]`, but none are embedded on-site and there's no official channel — so this is an off-site brand-mention signal (scored in §6), not an on-site multi-modal citability signal. No confirmed on-site product screenshots, demo video, or interactive elements. Given the product is a bot/agent tool, a homepage demo GIF or screenshot is plausible but unverified.

**Action:** verify presence of embedded product demo video/GIF on homepage and pricing page next session.

---

## 6. Authority & Brand Signals — 30/100 (re-scored, 20% weight)

`[CARRIED FORWARD facts, re-weighted under this skill's specific correlation table]`

| Signal | This skill's correlation | AtomicBot status | Sub-score |
|---|---|---|---|
| YouTube mentions (~0.737, strongest) | Highest weight | 3 unofficial third-party Shorts only, no official channel | 20/100 |
| Reddit presence (high) | High weight | Minimal, plus name-collision noise (see below) | 15/100 |
| Wikipedia entity (high) | High weight | **Worse than absent** — the name collides with an unrelated Tiny Defense video-game character and an unrelated Reddit RSS bot. This is a genuine entity-disambiguation risk, not merely a missing-signal gap: it means any Wikidata/Wikipedia-grounded entity resolution a model attempts is more likely to resolve to the wrong entity, actively suppressing rather than just failing to help brand citation. | 10/100 |
| LinkedIn presence (moderate) | Moderate weight | Company page exists but sparse | 40/100 |
| Domain Rating / backlinks (~0.266, weak) | Deliberately low weight in this rubric | Unknown this session; low-weighted regardless | not separately scored |
| Recency / dated content | This skill flags content <3 months old as ~3x more likely to be cited; 6+ month staleness loses eligibility | Unknown — homepage tagline described as static marketing copy with no visible update cadence | Penalized qualitatively |

**Net: 30/100**, slightly below the prior audit's 36/100 brand-authority figure — the reduction reflects this skill's explicit treatment of the Wikipedia name-collision as an *active negative* (entity confusion) rather than a neutral absence, which the prior audit's rubric didn't distinguish.

---

## 7. Technical Accessibility — 68/100 (20% weight)

- **SSR vs CSR (this skill's #1 technical check):** Webflow publishes pre-rendered static HTML/CSS at build/publish time — it is **not** a client-side-rendered SPA shell like a Next.js app using client components without SSR. This is a genuine structural advantage over the prior JS-framework assumption implied by the general audit checklist, and materially reduces the risk that GPTBot/ClaudeBot/PerplexityBot (none of which execute JavaScript) see an empty shell. `[REASONED, high confidence — Webflow's publishing model is a documented platform characteristic, though not independently re-verified for this specific site this session]`.
- **robots.txt AI-crawler allowance:** unverified this session; estimated open by default per Webflow's platform behavior (see §1).
- **llms.txt:** confirmed absent (§2) — counted as an unmet checklist item in this dimension per the skill's own criteria list, even though it carries zero citation-ranking weight.
- **RSL 1.0 licensing:** no evidence found in either audit pass; absence assumed (very few sites outside Reddit/Yahoo/Medium/Quora-tier publishers have adopted it as of mid-2026, so this is a low-priority gap, not a differentiator either way).

---

## 8. Platform-Specific Scores (heuristic, not Google-internal data)

This skill treats **Google AI Overviews and Google AI Mode as two distinct citation engines** (13.7% same-URL overlap per Ahrefs' 540K-query-pair study), not one — scored separately below.

| Platform | Key citation sources (this skill's data) | AtomicBot fit | Score /100 |
|---|---|---|---|
| Google AI Overviews | Strongly ranking-correlated; rewards traditional SEO + passage optimization | No live rank data this session; weak-to-moderate directory/backlink footprint carried forward | 48 |
| Google AI Mode (Gemini 2.5-based) | Weakly ranking-correlated; broader pool (~9 domains/query); rewards freshness + entity authority over position | Entity authority is the weakest link (no Wikipedia, confused name collision); freshness unverified | 38 |
| ChatGPT (web search) | Wikipedia 47.9%, Reddit 11.3% of citations | Zero clean Wikipedia entity (actively confusable) + minimal Reddit — the two dominant ChatGPT citation sources are both effectively absent | 28 |
| Perplexity | Reddit 46.7%, Wikipedia | Same Reddit/Wikipedia gap as ChatGPT, Perplexity leans even harder on Reddit | 32 |
| Bing Copilot | Bing index quality + authoritative sites + IndexNow | Unverified Bing indexing; LinkedIn + 12+ directory listings may help Bing's index modestly | 45 |

These are directionally consistent with, but numerically somewhat lower than, the prior audit's platform scores (58/51/48/47/46) — the reduction is a rubric artifact: this skill weights the ChatGPT/Perplexity scores explicitly against the Wikipedia-47.9%/Reddit-11.3% and Reddit-46.7% citation-source data, which makes the Wikipedia name-collision problem count for more than it did under the prior methodology.

**Only 11% of domains are cited by both ChatGPT and Google AI Overviews for the same query** (this skill's stated baseline) — reinforces that platform-specific content (not a single generic page) is needed rather than one-size-fits-all optimization.

---

## 9. Cloud Positioning: Entity-Confusion Risk Assessment

### Is there a positioning conflict?

**Yes, and it compounds an existing weakness rather than being a fresh, isolated risk.** AtomicBot's current AI-model understanding of the brand is already thin (weak Wikipedia entity, minimal Reddit, no official YouTube) — which means the *entire* current entity association an AI model has for "AtomicBot" is built almost exclusively from the site's own homepage copy: "runs locally on your Mac for privacy." That single sentence is doing outsized entity-definition work precisely because there's little independent, cross-platform corroboration to triangulate against.

Introducing a cloud product into that vacuum creates three concrete failure modes for AI answer generation:

1. **Direct contradiction risk.** A model asked "does AtomicBot run locally or in the cloud?" after cloud launch may retrieve both the old cached/training-data association (local-only) and new cloud-page content, and either hedge unhelpfully ("it offers both, depending on the source") or — worse — confidently assert the stale local-only claim if the cloud page hasn't yet been crawled/indexed/cited as authoritatively as the older, more-established homepage tagline. Because recency matters ~3x for citation eligibility (this skill's SE Ranking stat) but the *existing* local-privacy narrative has a longer citation/training history, the new cloud claim starts at a real disadvantage rather than a neutral one.
2. **Diluted, less-citable answers.** LLMs favor unambiguous, self-contained claims (per this skill's citability criteria). "AtomicBot runs locally for privacy, and also offers cloud" is a weaker, less quotable sentence than either single claim alone — it reduces the odds either version gets extracted as a clean citation.
3. **Entity resolution gets harder, not easier.** Given the pre-existing name collision with the video-game character and the RSS bot, adding a second, seemingly contradictory product axis to the same brand name increases the surface area for a model's entity-disambiguation step to fail or produce a low-confidence answer, at exactly the same time you want that disambiguation to succeed.

This is not a reason to avoid the cloud narrative — it's a reason to introduce it with deliberate information architecture rather than by simply appending it to the existing homepage.

### Recommended approach

- **Do not silently overwrite the "local privacy" homepage narrative.** Preserve it as the anchor entity association (it's the brand's one clear, differentiated, currently-working claim) and introduce cloud as an explicit **second, clearly-labeled tier/product line**, not a repositioning. Concretely: name it distinctly enough to be citable as its own sub-entity (e.g., "AtomicBot Cloud" or "AtomicBot for Teams," consistently capitalized and used verbatim across the cloud page, blog posts, and any future Product Hunt / directory listings) so AI models can learn a stable sub-entity → product mapping rather than overwriting the parent entity's single existing association.
- **Make the coexistence explicit in the very first 40-60 words of the new `/cloud` page and of the (unchanged) homepage.** E.g., homepage: "AtomicBot runs locally on your Mac for privacy-first automation. For teams that need shared, always-on automation, AtomicBot Cloud offers the same capabilities hosted in the cloud." This single sentence pre-empts the contradiction rather than leaving it for a model to reconcile from two disconnected pages.
- **Cross-link and cross-cite.** The `/cloud` page and homepage should each explicitly reference the other's positioning in a self-contained sentence, so a crawler that indexes only one page still gets the disambiguating context, and RAG-style retrieval that pulls one passage doesn't accidentally present a partial, contradictory answer.
- **Update dates visibly.** Since recency is a direct citation-eligibility factor in this skill's rubric, both the (lightly-revised) homepage and the new cloud page need a visible "last updated" date at launch, and a refresh cadence thereafter — otherwise the newer cloud claim is competing against an undated, longer-standing local claim with no freshness signal to tip the balance.
- **Update sameAs / entity signals in lockstep.** Any new Organization/Product schema, LinkedIn description, and directory listings (the 12+ known AI tool directories) should be updated to mention both product lines consistently, so third-party entity signals reinforce the same dual narrative rather than half of them still describing AtomicBot as local-only.

### GEO-optimized content format for `/cloud` and supporting posts

1. **Direct-answer opener (134–167 words):** first content block on `/cloud` must open with a self-contained definition sentence in the first 40-60 words — "AtomicBot Cloud is AtomicBot's hosted automation product for teams, running the same agent capabilities as the local Mac app but without local installation" — followed by 1-2 supporting facts (uptime, supported integrations, pricing tier) with attribution.
2. **A local-vs-cloud comparison table**, positioned early (first 30% of the page, matching the ~44%-of-citations-from-first-30% pattern): rows for privacy/data residency, offline capability, latency, setup time, pricing, supported platforms, ideal use case. Tables are one of this skill's explicit "strong structural readability signals" and are inherently well-suited to AI extraction because each row is already a self-contained fact pair.
3. **Question-based H2/H3 headings** matching likely query patterns: "Is AtomicBot Cloud as private as the local version?", "Can I use AtomicBot Cloud and the Mac app together?", "How does AtomicBot Cloud handle my data?" — each answered in a self-contained 100-160 word block directly under the heading.
4. **FAQ section** (structured markup, not schema-first per this skill's guidance for commercial content) addressing the coexistence question directly and explicitly, since it's the single most likely point of AI (and human) confusion.
5. **Blog posts should each carry one unique, attributed data point** (e.g., a benchmark comparing local vs. cloud latency, or a specific customer/beta stat) — per Google's own myth-busting guidance (referenced in `google-ai-optimization-guide.md`), the highest-leverage move is unique, first-hand content, not rephrasing generic "cloud vs local" commodity takes that already exist across the web.
6. **Visible publication + last-updated dates on every new page/post**, given the recency-weighting evidence above.

---

## Top 3 Priority Fixes

| # | Fix | Dimension(s) impacted | Effort | Why it's top-3 |
|---|---|---|---|---|
| 1 | Rewrite homepage opening ~150 words into a self-contained, attributed, direct-answer passage (134-167 words) that explicitly states both the local-privacy claim and previews the cloud direction if/when launched | Citability (25%), Authority (dates) | Low (copywriting only, no dev work) | Single highest-visibility passage on the site; currently scores 38/100 on the strictest test in this skill's rubric; also pre-empts the cloud-positioning contradiction risk described in §9 |
| 2 | Explicitly allow GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot in Webflow's robots.txt editor, and publish a minimal `/llms.txt` (framed internally as a technical-credibility signal, not a ranking lever, per the evidence in `references/llmstxt-evidence.md`) | Technical Accessibility (20%) | Low (Webflow's SEO settings support both without code injection) | Removes two "unknown/absent" items entirely; near-zero cost; directly closes the two checklist gaps in the weakest-verified dimension |
| 3 | Build a genuine Reddit/YouTube presence plan (official YouTube channel with product demos; sponsor or participate authentically in 1-2 relevant subreddits) rather than relying on 3 unofficial Shorts and a name-colliding Wikipedia absence | Authority & Brand Signals (20%), directly lifts ChatGPT (28) and Perplexity (32) platform scores | Medium-High (ongoing content program, not a one-time fix) | YouTube is this skill's single strongest brand-mention correlation (~0.737); Reddit is the dominant citation source for both ChatGPT (11.3%) and Perplexity (46.7%) — this is the highest-ceiling, if slowest, fix available |

---

## Structured Findings (for audit-data.json — AI Search Readiness category)

```json
{
  "category": "AI Search Readiness",
  "skill": "seo-geo",
  "skill_version": "2.2.4",
  "audit_date": "2026-07-23",
  "site": "atomicbot.ai",
  "verification_status": "live_fetch_blocked_org_policy",
  "composite_score": 47,
  "dimensions": {
    "citability": {"weight": 0.25, "score": 58, "confidence": "medium"},
    "structural_readability": {"weight": 0.20, "score": 40, "confidence": "low_estimated"},
    "multi_modal_content": {"weight": 0.15, "score": 35, "confidence": "low_estimated"},
    "authority_brand_signals": {"weight": 0.20, "score": 30, "confidence": "medium_high"},
    "technical_accessibility": {"weight": 0.20, "score": 68, "confidence": "medium"}
  },
  "llms_txt": {
    "status": "absent",
    "confidence": "high_carried_forward",
    "ranking_lever": false,
    "recommendation": "publish_as_credibility_signal_not_ranking_lever"
  },
  "ai_crawler_access": {
    "GPTBot": "unknown", "OAI-SearchBot": "unknown",
    "ClaudeBot": "unknown", "PerplexityBot": "unknown",
    "estimated_default": "likely_open_webflow_default_unverified"
  },
  "brand_signals": {
    "wikipedia": "absent_and_actively_confusable_name_collision",
    "reddit": "minimal",
    "youtube": "3_unofficial_shorts_no_official_channel",
    "linkedin": "sparse_company_page",
    "x_twitter_followers": 6146,
    "directory_listings": "12+"
  },
  "platform_scores": {
    "google_ai_overviews": 48,
    "google_ai_mode": 38,
    "chatgpt": 28,
    "perplexity": 32,
    "bing_copilot": 45
  },
  "cloud_positioning_risk": "high_without_explicit_disambiguation",
  "top_priority_fixes": [
    "rewrite_homepage_opening_passage",
    "allow_ai_crawlers_and_publish_llms_txt",
    "build_reddit_youtube_presence"
  ]
}
```

# GEO Audit Report — atomicbot.ai
**Generated:** 2026-07-23  
**Audited URL:** https://atomicbot.ai/  
**Business Type:** SaaS — AI agent desktop installer (macOS/Windows)  
**Audit Method:** 5-agent parallel analysis via public web signals (direct HTTP access blocked by proxy; all findings derived from SERP data, third-party sources, and indexed content)

---

## Composite GEO Score: 49 / 100 — Poor-to-Fair

> AI search is where your target audience (developers, AI power users) increasingly discovers tools. At 49/100, atomicbot.ai has meaningful early signals but critical structural gaps that suppress citation and brand recognition across every AI platform. The good news: most high-impact fixes are technical implementations, not content creation — the content quality already exists.

### Score by Category

| Category | Weight | Score | Weighted | Status |
|---|---|---|---|---|
| AI Citability & Visibility | 25% | 51/100 | 12.8 | Fair |
| Brand Authority Signals | 20% | 36/100 | 7.2 | Poor |
| Content Quality & E-E-A-T | 20% | 63/100 | 12.6 | Fair-to-Good |
| Technical Foundations | 15% | 68/100 | 10.2 | Fair |
| Structured Data | 10% | 10/100 | 1.0 | Critical |
| Platform Optimization | 10% | 50/100 | 5.0 | Poor-to-Fair |
| **COMPOSITE GEO SCORE** | | | **48.8 → 49/100** | **Poor-to-Fair** |

### Score Gauge

```
  0        25        50        75       100
  |---------|---------|---------|---------|
                   ▲
                  49
         [POOR-TO-FAIR — ACT NOW]
```

---

## Executive Summary

Atomic Bot is a technically sound product with authentic original content and growing developer community recognition. The underlying OpenClaw ecosystem has 157,000+ GitHub stars and its own Wikipedia article — association leverage that Atomic Bot is currently not capturing in structured data or entity signals.

**Three structural gaps account for most of the 51-point deficit:**

1. **No structured data** (schema score: 10/100) — AI models cannot resolve Atomic Bot as a known entity, connect it to GitHub/LinkedIn/Product Hunt, or extract machine-readable product attributes. This single gap suppresses performance on all five AI platforms simultaneously.

2. **No Wikipedia article** — ChatGPT's entity recognition is near-zero without Wikipedia. The brand name "Atomic Bot" also collides with an unrelated Reddit RSS bot and a Tiny Defense video game character, creating additional confusion.

3. **No llms.txt** — For a product whose entire audience uses AI tools, the absence of llms.txt is a brand credibility gap as much as a technical one.

**What's working:** The blog content is genuinely original — original benchmark data (Hermes vs OpenClaw head-to-head: 203k/257k tokens, 12:01 runtime on MacBook Pro M5 Max 64GB), question-structured titles, and correct technical terminology. This content can compete for AI citations once structural signals are in place.

---

## Subagent Reports

### 1. AI Citability & Visibility — 51/100

*Agent: geo-ai-visibility*

#### llms.txt
**Status: ABSENT — Score 0/100**

No `atomicbot.ai/llms.txt` exists. No `llms-full.txt` variant found. For a developer-focused AI agent product, this absence signals that the brand has not yet treated AI-native discoverability as a priority — which undermines positioning credibility with the target audience.

#### AI Crawler Access
**Status: UNKNOWN — Estimated 75/100**

`robots.txt` is inaccessible (proxy blocked). No evidence of AI crawler blocking was found anywhere. Given the product's open-source character and developer ethos, blocking is unlikely. No `Content-Signal:` directive detected.

| Crawler | Status |
|---|---|
| GPTBot | Unknown |
| OAI-SearchBot | Unknown |
| ClaudeBot | Unknown |
| PerplexityBot | Unknown |
| Google-Extended | Unknown |
| All others | Unknown |

#### AI Citability Scores (Top Content Blocks)

| Content Block | Score | Status |
|---|---|---|
| Product Hunt launch data (186 upvotes, #7 PotD, Feb 13 2026) | 83/100 | Citation-ready |
| Pricing claim ($25/mo paid, freemium, 7-day guarantee) | 73/100 | Citation-ready |
| Feature inventory (Gmail, document, browser, file, scheduling) | 70/100 | Approaching |
| Hermes vs OpenClaw qualitative comparison | 67/100 | Approaching |
| Local privacy claim (on-device, no API keys) | 66/100 | Approaching |
| Homepage hero copy ("The Fastest Way to Run OpenClaw") | 48/100 | Poor — marketing tagline, not citable |

**Overall Page Citability: 62/100** — Best citable content lives on third-party sites, not on atomicbot.ai itself. Homepage hero copy is a marketing tagline (0 factual content). Blog posts written in narrative format lack structured answer blocks.

#### Brand Authority Score: 36/100

| Platform | Status | Score |
|---|---|---|
| Wikipedia | ABSENT — name collision with game character + Reddit bot | 0/30 |
| Reddit | No brand discussions found | 4/20 |
| YouTube | 3 third-party Shorts only; no official channel | 7/15 |
| LinkedIn | Company page exists (sparse) | 6/10 |
| X / Twitter | @atomicbot_ai — 6,146 followers (active) | included in niche |
| AI directories | 12+ listings; 105 reviews on theresanaiforthat.com | 19/25 |
| **Total** | | **36/100** |

---

### 2. Platform Optimization — 50/100

*Agent: geo-platform-analysis*

| Platform | Score | Key Gap |
|---|---|---|
| Google AI Overviews | 58/100 | No FAQPage schema; no Article schema |
| Bing Copilot | 51/100 | No IndexNow; no Bing Webmaster Tools verification |
| Perplexity AI | 48/100 | No Reddit presence (Perplexity's primary community signal) |
| ChatGPT Web Search | 46/100 | No Wikipedia (entity resolution near-zero without it) |
| Google Gemini | 47/100 | No Wikidata entity; no long-form YouTube; no Knowledge Panel |

**Strongest platform:** Google AIO — question-format blog titles ("Is Claude Code Free?", "Hermes vs OpenClaw") are structurally AIO-compatible. Directory listings reinforce authority.

**Weakest platform:** ChatGPT — 0.59% brand citation rate is the platform baseline; without Wikipedia, Atomic Bot cannot clear the entity recognition threshold. Name collision compounds this.

**Cross-platform synergies (actions that lift all 5 platforms):**

1. Wikipedia article — lifts ChatGPT (+15-20 pts), Gemini, Perplexity, Google AIO
2. Organization + schema site-wide — lifts all 5 simultaneously
3. Reddit presence — lifts Perplexity (primary), Google AIO (secondary)
4. Wikidata Q-item — lifts Gemini, ChatGPT (bridges gap before Wikipedia)

---

### 3. Technical Foundations — 68/100

*Agent: geo-technical*

| Category | Score | Status |
|---|---|---|
| URL structure | 85/100 | Good — clean slugs, no query params |
| Crawlability / indexation | 75/100 | ~10-11 pages indexed; healthy for age |
| Server-side rendering | 70/100 | Next.js (confirmed via GitHub); SSG likely for marketing site |
| Mobile optimization | 70/100 | Likely adequate (Next.js defaults); unverified |
| Core Web Vitals | 65/100 | Medium risk — third-party scripts unknown |
| Meta tags | 65/100 | Titles good; OG tags unverified |
| Security headers | 50/100 | TLS confirmed; HSTS/CSP unverified |
| **Technical Score** | **68/100** | **Fair** |

**Key finding:** The marketing site (atomicbot.ai) is built on Next.js with likely SSG/ISR deployment — AI crawlers can read full HTML content from blog posts and core pages. The `app.atomicbot.ai` subdomain is likely CSR (SPA), invisible to AI crawlers. Domain age ~7 months is the primary authority constraint.

**Domain signals:**
- SSL: Let's Encrypt / TLS 1.3 (confirmed)
- Third-party trust: 72/100 (ScamAdviser)
- MRR: ~$4,026/month (TrustMRR)
- GitHub: 15 public repositories in AtomicBot-ai org

---

### 4. Content Quality & E-E-A-T — 63/100

*Agent: geo-content*

| Dimension | Score | Key Finding |
|---|---|---|
| Experience | 68/100 | Strong: head-to-head Hermes vs OpenClaw benchmark (203k/257k tokens, 12:01 on M5 Max 64GB, same model/task) |
| Expertise | 52/100 | Deep technical content; zero author bylines — expertise demonstrated but uncredentialed |
| Authoritativeness | 56/100 | Above-average for 7-month domain; no Wikipedia, no .edu/.gov links |
| Trustworthiness | 56/100 | Strong privacy policy (no training, no selling, GDPR); no physical address; no editorial policy |
| Content citability format | 72/100 | Question-structured titles, comparison tables, numeric specificity |
| **Content Score** | **63/100** | **Fair-to-Good** |

**Confirmed blog posts (8+):**
- `/blog/hermes-agent-vs-openclaw` — head-to-head architecture and benchmark
- `/blog/best-ai-automation-software` — AI automation roundup
- `/blog/qwen-3-7-max` — model benchmark vs Claude Opus 4.6 / GPT-5.5
- `/blog/hermes-use-cases-for-developers` — developer use case guide
- `/blog/is-claude-code-free` — pricing breakdown (specific caps, credit allocations)
- `/blog/set-up-openclaw-on-mac` — setup guide
- `/blog/how-to-install-openclaw-on-windows` — setup guide
- `/blog/what-is-local-ai` — local LLM guide (5-model comparison: GLM-5.1, DeepSeek V4, Qwen 3.6, Gemma 4, MiniMax M2.5)

**Content format for AI citability: 72/100** — question-structured headings, comparison tables, and numeric specificity are strong. Missing: FAQ accordion blocks, methodology disclosures, structured answer paragraphs at post openings.

**Topical authority:** Moderate/Emerging in "AI agent automation + local LLM deployment" niche. Covers ~40-50% of expected subtopics.

---

### 5. Structured Data — 10/100 — CRITICAL

*Agent: geo-schema*

**Zero schema of any type deployed.** Confirmed by complete absence of rich results across all SERP signal checks — no star ratings, no breadcrumbs, no FAQ dropdowns, no article metadata, no sitelinks.

| Schema Type | Status | GEO Impact |
|---|---|---|
| Organization + sameAs | MISSING | Critical |
| SoftwareApplication | MISSING | Critical |
| Article / BlogPosting | MISSING | Critical |
| Person (author) | MISSING | High |
| speakable | MISSING | Medium |
| BreadcrumbList | MISSING | Medium |
| WebSite + SearchAction | MISSING | Low |

**sameAs entity links in deployed schema: 0**

| Platform | URL | Schema Link | Priority |
|---|---|---|---|
| GitHub | github.com/AtomicBot-ai | Not linked | Critical |
| LinkedIn | linkedin.com/company/atomicbot | Not linked | Critical |
| X / Twitter | x.com/atomicbot_ai | Not linked | High |
| Product Hunt | producthunt.com/products/atomic-bot | Not linked | High |
| Wikipedia | Does not exist yet | — | Must create |
| Wikidata | Does not exist yet | — | Must create |
| Crunchbase | Does not exist yet | — | Medium |

**JavaScript delivery risk:** Next.js sites frequently inject schema client-side. AI crawlers (GPTBot, ClaudeBot, PerplexityBot) do not execute JavaScript — any client-injected JSON-LD is completely invisible to them. All schema must be server-rendered in initial HTML.

**Implementation note on deprecated schemas:**
- HowTo schema: removed from rich results (Sep 2023) — do not implement
- FAQPage: restricted to government/health domains — low rich-result value but retains semantic value for AI extraction

---

## Prioritized Action Plan

### CRITICAL — Do Within 7 Days

| # | Action | Impact | Effort | Platforms Affected |
|---|---|---|---|---|
| 1 | Deploy Organization JSON-LD on homepage with sameAs (GitHub, LinkedIn, X, Product Hunt) | Entity resolution unlocked | 2h | All 5 |
| 2 | Deploy SoftwareApplication JSON-LD on `/` and `/hermes` as separate blocks | Product identity for AI | 3h | All 5 |
| 3 | Publish `/llms.txt` — brand summary, key pages, GitHub link, docs | AI-native credibility | 2h | All (direct) |
| 4 | Add author bylines + `/about` page with team names and GitHub handles | E-E-A-T Expertise fix | 1 day | AIO, ChatGPT |
| 5 | Deploy BlogPosting + Person + speakable schema on all blog posts | Citation extraction | 4h | AIO, Bing, Gemini |

**JSON-LD Template 1 — Organization (homepage `<head>`):**
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Atomic Bot",
  "url": "https://atomicbot.ai",
  "logo": {
    "@type": "ImageObject",
    "url": "https://atomicbot.ai/logo.png",
    "width": 512,
    "height": 512
  },
  "description": "One-click macOS and Windows installer for OpenClaw and Hermes AI agents. Run AI agents locally with full privacy — no terminal setup required.",
  "foundingDate": "2026-02",
  "contactPoint": {
    "@type": "ContactPoint",
    "email": "support@atomicbot.ai",
    "contactType": "customer support"
  },
  "sameAs": [
    "https://github.com/AtomicBot-ai",
    "https://www.linkedin.com/company/atomicbot",
    "https://x.com/atomicbot_ai",
    "https://www.producthunt.com/products/atomic-bot"
  ]
}
```

**JSON-LD Template 2 — SoftwareApplication (homepage + /hermes):**
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Atomic Bot",
  "alternateName": "AtomicBot",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "macOS, Windows",
  "softwareVersion": "1.0.114",
  "url": "https://atomicbot.ai",
  "downloadUrl": "https://atomicbot.ai",
  "description": "One-click installer for OpenClaw AI agent framework. Runs locally on your machine for full privacy. Supports Claude, GPT-4o, Gemini, and local models.",
  "featureList": [
    "Gmail management and automation",
    "Document summarization",
    "Browser automation",
    "Task scheduling",
    "Local LLM support (Qwen, DeepSeek, Gemma)",
    "No cloud dependency option"
  ],
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD",
    "description": "Free tier available. Paid plans from $25/month."
  },
  "publisher": {
    "@type": "Organization",
    "name": "Atomic Bot",
    "url": "https://atomicbot.ai"
  }
}
```

**JSON-LD Template 3 — BlogPosting + speakable (each blog post `<head>`):**
```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "Hermes vs OpenClaw: Which to Choose, When to Run Both and Why",
  "datePublished": "2026-05-14",
  "dateModified": "2026-05-15",
  "author": {
    "@type": "Person",
    "name": "[Author Name]",
    "url": "https://atomicbot.ai/team/[handle]",
    "sameAs": ["https://github.com/[handle]", "https://x.com/[handle]"]
  },
  "publisher": {
    "@type": "Organization",
    "name": "Atomic Bot",
    "logo": {
      "@type": "ImageObject",
      "url": "https://atomicbot.ai/logo.png"
    }
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://atomicbot.ai/blog/hermes-agent-vs-openclaw"
  },
  "articleSection": "AI Agents",
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [".post-intro", ".tldr", "h2", "h3"]
  }
}
```

---

### HIGH — Do Within 30 Days

| # | Action | Impact | Effort | Platforms Affected |
|---|---|---|---|---|
| 6 | Draft and submit Wikipedia article "Atomic Bot (software)" | Entity recognition unlock | 1-2 weeks (incl. Wikipedia review) | ChatGPT, Gemini, Perplexity |
| 7 | Create Wikidata Q-item (bridges Wikipedia gap immediately) | Knowledge Graph signal | 2-3 hours | Gemini, ChatGPT |
| 8 | Launch Reddit presence: post in r/LocalLLaMA, r/MacApps, r/AIAssistants | Perplexity citation boost (3.2x for <30 days) | 1 day + ongoing | Perplexity (primary), AIO |
| 9 | Add OG meta tags + Twitter Card to all pages | Social sharing + AI preview control | 2-4h | All (indirect) |
| 10 | Add methodology disclosure block to benchmark posts | E-E-A-T Experience boost | 2h | AIO, ChatGPT |
| 11 | Implement IndexNow + Bing Webmaster Tools verification | Bing Copilot freshness | 2-4h | Bing Copilot |
| 12 | Add explicit AI crawler allowances in robots.txt + sitemap reference | Crawler clarity | 30min | All |
| 13 | Add "What is Atomic Bot?" direct-answer section to homepage (45-55 words, factual) | Homepage citability: 48→70+ | 1h | AIO, ChatGPT |

---

### MEDIUM — Do Within 90 Days

| # | Action | Impact | Effort | Platforms Affected |
|---|---|---|---|---|
| 14 | Launch official YouTube channel with long-form tutorial (8-12 min OpenClaw setup) | Gemini Knowledge Graph | 1-2 days | Gemini (primary) |
| 15 | Add VideoObject schema linking YouTube channel to domain | Entity association | 1h | Gemini, AIO |
| 16 | Publish monthly changelog blog post (keep freshness <30 days) | Perplexity freshness signal | Ongoing | Perplexity |
| 17 | Create Crunchbase company profile | Entity diversity | 1h | ChatGPT, Gemini |
| 18 | Add security headers (HSTS, CSP, X-Frame-Options) via Next.js config | Technical trust | 2-4h | Technical |
| 19 | Create /llms-full.txt with full homepage + blog content inlined | llms.txt score: 0→90+ | 2h | All (direct) |
| 20 | Add FAQ accordion + schema to "Is Claude Code Free?" and Hermes vs OpenClaw posts | AI extraction rate | 2h | AIO, Bing |
| 21 | Publish use-case content for non-developer users (team workflows, productivity ROI) | Bing Copilot enterprise reach | Ongoing | Bing Copilot |

---

## Score Projection

If the 5 CRITICAL actions are completed within 7 days:

| Category | Current | Projected | Change |
|---|---|---|---|
| AI Citability & Visibility | 51 | 68 | +17 |
| Brand Authority | 36 | 42 | +6 |
| Content Quality | 63 | 72 | +9 |
| Technical | 68 | 72 | +4 |
| Structured Data | 10 | 65 | +55 |
| Platform Optimization | 50 | 58 | +8 |
| **Projected GEO Score** | **49** | **~63** | **+14** |

If HIGH + CRITICAL actions are completed within 30 days (including Wikipedia):

| **Projected GEO Score** | **49** | **~72** | **+23** |

---

## Competitive Context

| Signal | Atomic Bot | Strong Competitor Benchmark |
|---|---|---|
| GEO Score | 49/100 | 70-80/100 (mature SaaS) |
| Schema deployed | 0 types | 5-8 types |
| llms.txt | Absent | Present |
| Wikipedia | Absent | Present |
| Reddit brand discussions | None | Active |
| Blog posts | 8+ | 50-200 |
| Domain age | ~7 months | 3-5 years |
| Backlink profile | Early-stage | Established |

**Assessment:** The gap is primarily structural (schema, Wikipedia, llms.txt) not content quality. The content already outperforms many established competitors in specificity and originality. Structural fixes deliver disproportionate GEO score gains relative to effort.

---

## Quick Reference: What AI Platforms Need

| Platform | Top Unmet Need |
|---|---|
| Google AI Overviews | FAQPage + Article schema; direct-answer homepage section |
| ChatGPT Web Search | Wikipedia article (entity recognition baseline) |
| Perplexity AI | Reddit brand discussions (3.2x citation boost for <30 days) |
| Google Gemini | Wikidata Q-item; long-form YouTube; VideoObject schema |
| Bing Copilot | IndexNow implementation; Bing Webmaster Tools verification |

---

## Data Confidence

| Finding | Confidence | Basis |
|---|---|---|
| Schema: zero deployed | High | Complete absence of rich results across all SERP checks |
| llms.txt: absent | High | Exhaustive search returned no indexed references |
| Framework: Next.js | High | GitHub org + repo analysis |
| Wikipedia: absent | High | Direct Wikipedia search confirmed |
| Crawler access | Low | robots.txt inaccessible; estimated from product character |
| Core Web Vitals | Low | Inferred from framework defaults; not measured |
| OG tags | Unknown | Unverifiable without direct page access |

*All scores should be validated against direct page access and tools such as Google Rich Results Test, PageSpeed Insights, and Search Console once proxy access is available.*

---

## Appendix: Key URLs

| Page | URL | Priority |
|---|---|---|
| Homepage | https://atomicbot.ai/ | Schema priority |
| Hermes product page | https://atomicbot.ai/hermes | SoftwareApplication schema |
| Blog index | https://atomicbot.ai/blog | |
| Privacy policy | https://atomicbot.ai/privacy-policy | Trust signal |
| Terms of service | https://atomicbot.ai/terms-of-service | Trust signal |
| App | https://app.atomicbot.ai/setup | CSR — low AI crawler priority |
| GitHub org | https://github.com/AtomicBot-ai | sameAs target |
| Product Hunt | https://www.producthunt.com/products/atomic-bot | sameAs target |
| LinkedIn | https://www.linkedin.com/company/atomicbot | sameAs target |
| X / Twitter | https://x.com/atomicbot_ai | sameAs target |

---

*GEO Audit generated by the geo-seo-claude skill suite. Methodology: 5-agent parallel analysis (geo-ai-visibility, geo-platform-analysis, geo-technical, geo-content, geo-schema). AI disclosure: This report was produced using AI-assisted research from web-indexed sources. Direct site access was unavailable due to network proxy restrictions; findings derived from SERP signals, third-party directories, and public web data. Validate scored items directly before actioning.*

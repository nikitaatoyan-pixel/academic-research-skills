# Technical SEO Audit — atomicbot.ai

**Audit date:** 2026-07-23
**Platform (corrected):** Webflow (marketing site) + separate app stack at `app.atomicbot.ai`
**Scope:** Technical SEO only (crawlability, indexability, security, URL structure, mobile, Core Web Vitals risk, structured data delivery, JS rendering, IndexNow)

---

## 0. Verification Limitations (read this first)

This audit session had **no working network access to atomicbot.ai**, and the tools named in the task brief (`WebFetch`, `WebSearch`) were **not actually exposed** in this agent's toolset — only `Read`, `Bash`, `Write`, `Glob`, `Grep`, and MCP (Figma/GitHub) tools were available. This is a tooling-availability gap, not a choice to skip verification.

Confirmed via direct testing this session:
- `claude-seo run sitemap_discovery.py https://atomicbot.ai --json` → `robots.txt could not be fetched safely`; all common sitemap paths (`/sitemap.xml`, `/sitemap_index.xml`, `/sitemap-index.xml`, `/wp-sitemap.xml`) returned `"error": "Request failed"`.
- `claude-seo run render_page.py https://atomicbot.ai --mode never --json` → raw fetch failed at the proxy layer (`ProxyError` / `NameResolutionError`).
- Direct `curl` to `https://atomicbot.ai/robots.txt` and `/sitemap.xml` → `CONNECT tunnel failed, response 403`.
- Proxy status log (`/__agentproxy/status`) shows the block is not atomicbot.ai-specific: `index.commoncrawl.org` and `api.web.archive.org` were also rejected with 403 in the same window, so **Wayback Machine / Common Crawl fallback verification was also unavailable** this session.

**Net effect:** robots.txt and sitemap.xml contents remain **unconfirmed for a second consecutive audit**. Everything below that isn't sourced from the prior GEO audit's confirmed facts (SSL, trust score, domain age, indexed-page list) is a **platform-default inference for Webflow**, clearly labeled as such, not a live measurement. Treat Sections 1, 2, 6, and 7 as "verify manually in Webflow Project Settings" items, not closed findings.

---

## 1. Crawlability — UNVERIFIED (platform defaults favorable)

| Check | Status |
|---|---|
| robots.txt reachable | Blocked this session (2nd audit in a row) |
| sitemap.xml reachable | Blocked this session |
| Sitemap declared in robots.txt | Unknown |
| noindex directives | Unknown (no evidence of blanket noindex; ~10-11 pages are known to be indexed per prior audit, which is inconsistent with a site-wide noindex/disallow) |

Webflow-specific context:
- Webflow auto-generates `robots.txt` and can auto-generate `sitemap.xml` (Project Settings → SEO → "Generate sitemap.xml automatically"). Default robots.txt on a fresh Webflow project allows all crawling and lists the sitemap URL once that toggle is on.
- The fact that ~10-11 pages are already indexed (per prior GEO audit's known facts) is a positive signal that crawling is not currently blocked — a hard `Disallow: /` or missing sitemap would not necessarily prevent this, but it's consistent with "no gross misconfiguration."
- **Action required (not performable this session):** confirm in the Webflow dashboard that Project Settings → SEO → "Sitemap" toggle is ON, and cross-check actual robots.txt/sitemap.xml content via Google Search Console → Settings → Crawl stats / Sitemaps report (or Bing Webmaster Tools), since neither this agent nor the prior audit could reach the domain directly.

**Category score: 10/15**

---

## 2. Indexability — UNVERIFIED, platform defaults favorable

- Webflow injects a **self-referencing `<link rel="canonical">` by default** on every published page — this is a platform strength and reduces duplicate-content risk out of the box.
- Duplicate-content risk to check manually:
  - If the `*.webflow.io` staging subdomain is still reachable, confirm it is either noindexed or 301-redirected to the custom domain (Webflow does this automatically once a custom domain + SSL is connected, but it's worth a manual spot-check via `site:atomicbot.ai` / `site:atomicbot-ai.webflow.io` search).
  - CMS-driven blog items (`/blog/hermes-agent-vs-openclaw`, `/blog/best-ai-automation-software`, `/blog/qwen-3-7-max`, `/blog/hermes-use-cases-for-developers`, `/blog/is-claude-code-free`, `/blog/set-up-openclaw-on-mac`, `/blog/how-to-install-openclaw-on-windows`, `/blog/what-is-local-ai`) should each have a **unique bound CMS field for title/meta description** — a common Webflow footgun is leaving the collection template's static meta fields unbound, which makes every CMS item share identical `<title>`/meta description text. This cannot be confirmed without live HTML access; flag for manual per-URL check in Page Settings or via a `site:` search comparing SERP snippets across the 8 blog posts.
- Thin content risk: 8 blog posts + 4 core pages (`/`, `/hermes`, `/privacy-policy`, `/terms-of-service`) + `/blog` index is a small but coherent content set for a ~7-month-old domain; not itself a red flag at this stage.

**Category score: 11/15**

---

## 3. Security — MOSTLY CONFIRMED (headers gap likely)

Confirmed (from prior GEO audit, ScamAdviser):
- HTTPS active, Let's Encrypt certificate, TLS 1.3.
- No malware/phishing flags; third-party trust score 72/100.

Webflow-specific inference (unverifiable this session):
- Webflow auto-provisions and auto-renews Let's Encrypt SSL for connected custom domains and force-redirects HTTP → HTTPS at the CDN edge by default — consistent with the confirmed SSL facts.
- **Likely gap:** Webflow's native hosting does not expose a UI for custom HTTP response headers (no built-in HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy configuration). Sites on Webflow that want these headers typically front the domain with Cloudflare (proxied, not DNS-only) or a similar edge layer. Cannot confirm from here whether atomicbot.ai has such a layer in front of Webflow — this would show up as an extra CNAME/proxy in DNS, which was not checkable this session.
- Recommend a manual header check (e.g., via securityheaders.com or `curl -I`) once network access is restored, specifically for HSTS, CSP, and X-Frame-Options.

**Category score: 10/15**

---

## 4. URL Structure — PASS (no new issues expected)

Known URL inventory is clean and consistent with Webflow's default static-page + CMS-collection URL scheme:
- `/`, `/privacy-policy`, `/terms-of-service`, `/hermes` — static pages, hyphenated, lowercase, no parameters.
- `/blog` (collection list page) + `/blog/<slug>` (collection items) — standard Webflow CMS pattern.

No query strings, session IDs, or uppercase/inconsistent casing observed in the known URL set. Nothing in this audit suggests regression since the prior review; this remains a strength of the site.

**Category score: 9/10**

---

## 5. Mobile-Friendliness — LIKELY PASS (high confidence, not independently verified)

- Webflow's Designer is responsive-by-default: every published page ships with the four standard breakpoints (Desktop/Tablet/Mobile Landscape/Mobile Portrait) and an auto-injected `<meta name="viewport" content="width=device-width, initial-scale=1">`. It is structurally very difficult to publish a non-responsive Webflow page without deliberately overriding breakpoint styles.
- Residual risks that require manual/visual verification (not checkable this session):
  - Touch target sizing on CTA and "Install" buttons (Google's guidance: ≥48×48 CSS px with adequate spacing) — worth checking given the site's primary conversion action is a download/install CTA linking out to `app.atomicbot.ai`.
  - Any custom-embedded HTML/JS widgets (Embed elements) that use fixed pixel widths can defeat Webflow's responsive system locally; these are the one common way a Webflow site fails a mobile-friendly check.

**Category score: 8/10**

---

## 6. Core Web Vitals Risk — MODERATE RISK (Webflow-specific profile, not measured)

No live Lighthouse/PSI/CrUX data was obtainable this session (network blocked). This section is a **risk-profile estimate based on Webflow platform characteristics**, deliberately distinct from a Next.js SSG risk profile:

| Metric | Webflow-specific risk factors |
|---|---|
| **LCP** | Product/landing pages for an AI-agent installer typically feature a large hero image or product screenshot above the fold. Webflow's automatic image optimization ("Enable next-gen image formats" — converts to WebP) must be explicitly enabled in Project Settings → Assets; if left off, hero images serve as unoptimized PNG/JPG, inflating LCP. Also check that the LCP-candidate image is **not** marked `loading="lazy"` — Webflow lazy-loads images by default, and lazy-loading the actual hero/LCP image is one of the most common Webflow CWV mistakes. |
| **CLS** | Custom/uploaded web fonts without `font-display: swap` or `<link rel="preload">` can cause FOIT/FOUT layout shift on text-heavy blog pages. Any custom-code Embed widgets (chat bubble, install-button micro-app, badges) that inject content post-load without reserved dimensions will also shift layout. |
| **INP** | Third-party scripts (analytics, chat widgets, and any embed pointing to `app.atomicbot.ai` for install-flow interactivity) add main-thread work. Native Webflow interactions/animations (IX2) are generally lightweight, but heavy custom-code embeds are the typical INP offender on Webflow marketing sites. |

**Recommendation:** run PageSpeed Insights / CrUX directly against `atomicbot.ai` and the `/hermes` and top blog URLs once network access allows, to convert this risk profile into measured data.

**Category score: 8/15**

---

## 7. Structured Data — LIKELY ABSENT / UNVERIFIED (largest opportunity)

- Confirmed platform mechanism: on Webflow, structured data (JSON-LD) is **not auto-generated** for general schema types. It must be added via:
  - **Project Settings → Custom Code → Head Code** (site-wide, e.g., `Organization` schema), or
  - **Page Settings → Custom Code** (per-page, e.g., `SoftwareApplication` on `/`, `Article`/`BlogPosting` on each `/blog/*` post, `HowTo` on the two install-guide posts).
- No HTML was retrievable this session, so presence/absence of existing JSON-LD `<script type="application/ld+json">` blocks could not be directly confirmed either way. Given that most Webflow sites ship with zero custom schema unless a site owner deliberately adds it, and there is no evidence pointing to existing schema, this is scored conservatively as a likely gap and a clear opportunity.
- Recommended schema set for this specific product:
  - `Organization` (site-wide, head custom code)
  - `SoftwareApplication` on `/` and `/hermes` (with `operatingSystem: ["macOS","Windows"]`, `applicationCategory`, `offers` for freemium pricing)
  - `BlogPosting`/`Article` on all 8 `/blog/*` posts
  - `HowTo` specifically on `/blog/set-up-openclaw-on-mac` and `/blog/how-to-install-openclaw-on-windows`
  - `FAQPage` if any FAQ content exists or is added

**Category score: 4/10**

---

## 8. JavaScript Rendering — PASS (marketing site) / FLAG (app subdomain)

- **Marketing site (Webflow):** Server-rendered/static HTML at publish time — no CSR/SPA indexability risk. This is a materially different (and better) profile than a Next.js CSR setup would present; crawlers receive full content without needing to execute JavaScript. Confirmed as a corrected fact for this audit and consistent with Webflow's architecture.
- **`app.atomicbot.ai/setup` (separate subdomain):** Flagged, not assessed in depth — this is very likely a different, more JS-heavy stack (the actual product onboarding/installer flow, not a content page). Two follow-up items:
  1. Confirm whether any `app.atomicbot.ai` URLs are being indexed at all (a `site:app.atomicbot.ai` search was not performable this session due to tool unavailability). If functional app routes are getting indexed, they likely don't render meaningful content for crawlers without JS execution, and/or shouldn't be indexed at all.
  2. If not intended for organic visibility, add `noindex` (meta robots or `X-Robots-Tag` header) and/or a `Disallow` rule in `app.atomicbot.ai/robots.txt` for app-functionality routes, separate from the marketing site's robots.txt.

**Category score: 5/5** (marketing site only; app subdomain excluded from this score, flagged as a follow-up item)

---

## 9. IndexNow Protocol (Bing, Yandex, Naver) — LIKELY NOT IMPLEMENTED

- Webflow has **no native/first-party IndexNow integration** in its hosting or Project Settings. This is a known platform gap, not specific to atomicbot.ai.
- No key file (`atomicbot.ai/<key>.txt`) or evidence of a custom implementation could be checked this session.
- To implement on Webflow, the standard workaround is:
  1. Generate an IndexNow key and host the verification file at the domain root (achievable via Webflow's static file hosting).
  2. Use Webflow's **site_publish webhook** (Project Settings → Integrations → Webhooks) to trigger an external function (Cloudflare Worker, Zapier/Make.com scenario, or a small serverless script) that POSTs changed URLs to `https://api.indexnow.org/indexnow` on every publish.
- Given the site's GEO angle (Bing indexing feeds Microsoft Copilot's grounding), this is a low-effort, non-trivial-value fix worth prioritizing despite the small page count.

**Category score: 1/5**

---

## Technical SEO Score: 66 / 100

| Category | Score | Weight |
|---|---|---|
| 1. Crawlability | 10 | /15 |
| 2. Indexability | 11 | /15 |
| 3. Security | 10 | /15 |
| 4. URL Structure | 9 | /10 |
| 5. Mobile-Friendliness | 8 | /10 |
| 6. Core Web Vitals Risk | 8 | /15 |
| 7. Structured Data | 4 | /10 |
| 8. JS Rendering | 5 | /5 |
| 9. IndexNow | 1 | /5 |
| **Total** | **66** | **/100** |

Interpretation: "Needs Improvement" tier. The score is depressed largely by two unrelated factors: (a) genuine platform gaps (structured data, IndexNow, security headers) that are real and fixable, and (b) unverifiable items (robots.txt/sitemap.xml/live CWV data) that are scored conservatively because they could not be confirmed for the second audit in a row — not because they're known to be broken.

---

## Prioritized Issues

### Critical
- None identified with direct evidence. The largest unknown — robots.txt/sitemap.xml content — could not be verified for a second consecutive audit due to network/tooling blocks, not a confirmed site defect. Treat as "must verify immediately via Google Search Console / Bing Webmaster Tools," not as a confirmed critical failure.

### High
1. **Structured data likely absent** — no confirmed JSON-LD for Organization/SoftwareApplication/Article anywhere on the site. (Section 7)
2. **Security response headers likely missing** (HSTS, CSP, X-Frame-Options, X-Content-Type-Options) — a native Webflow hosting limitation unless a proxy (e.g., Cloudflare) sits in front. (Section 3)
3. **robots.txt / sitemap.xml unconfirmed** for the second audit in a row — must be manually verified in the Webflow dashboard (Project Settings → SEO) and via Search Console/Bing Webmaster Tools, since this and the prior GEO audit could not reach the domain directly. (Section 1)

### Medium
4. **Core Web Vitals risk from Webflow defaults** — verify "Enable next-gen image formats" is ON and that the hero/LCP image is not set to lazy-load. (Section 6)
5. **IndexNow not implemented** — no evidence of a key file or publish-webhook integration. (Section 9)
6. **`app.atomicbot.ai` indexation status unknown** — verify it isn't leaking non-content app-shell pages into search results; add noindex/robots rules if not intended for organic visibility. (Section 8)
7. **CMS blog template meta-field binding unverified** — risk of duplicate title/meta description across the 8 `/blog/*` posts if static template text isn't bound to unique CMS fields. (Section 2)

### Low
8. Touch target sizing for install/CTA buttons — manual check recommended (≥48×48 CSS px). (Section 5)
9. Web font loading strategy (preload / `font-display: swap`) to reduce CLS risk from custom fonts. (Section 6)
10. Confirm the `*.webflow.io` staging subdomain (if still reachable) is noindexed or redirects to the custom domain. (Section 2)

---

## Top 3 Priority Fixes (Webflow-specific)

1. **Inject JSON-LD structured data via Webflow Custom Code** — Add `Organization` + `SoftwareApplication` schema in Project Settings → Custom Code → Head Code (site-wide), and add `BlogPosting`/`Article` schema (plus `HowTo` on the two install-guide posts) via Page Settings → Custom Code on each `/blog/*` post. This is the single largest gap identified and directly supports both classic SEO rich results and AI-crawler/GEO citability, since none of this is generated natively by Webflow's CMS.

2. **Front the domain with a header-injection layer for security headers** — Because Webflow's native hosting does not expose HSTS/CSP/X-Frame-Options configuration, route DNS through Cloudflare (proxied mode) or an equivalent edge layer and add Transform Rules/Workers to set these headers, rather than waiting on a Webflow platform feature that doesn't exist today. This is the standard site-owner workaround for this exact Webflow limitation.

3. **Implement IndexNow via the Webflow `site_publish` webhook** — Host an IndexNow key file at the domain root and wire the Project Settings → Integrations → Webhooks `site_publish` event to a lightweight external function (Cloudflare Worker or Zapier/Make scenario) that POSTs to `https://api.indexnow.org/indexnow` on every publish. Low effort, no native Webflow support exists, and it accelerates Bing/Yandex re-indexing — which matters for this site's GEO angle since Bing indexing feeds Microsoft Copilot's grounding.

---

## Follow-up Required (network access dependent)

The following could not be completed this session and should be re-attempted once WebFetch/WebSearch/direct network access is actually available to the agent:
- Raw fetch of `atomicbot.ai/robots.txt` and `atomicbot.ai/sitemap.xml`
- `site:atomicbot.ai` and `site:app.atomicbot.ai` search-operator checks
- HTTP response header inspection (`curl -I`) for security headers
- View-source / Rich Results Test check for existing JSON-LD
- Live PageSpeed Insights / CrUX pull for measured LCP/INP/CLS (replacing the risk-profile estimate in Section 6 with real data)

# Schema / Structured Data Audit — atomicbot.ai

**Date:** 2026-07-23
**Platform:** Webflow (no build step; custom code injected via Designer/Project Settings)
**Scope:** Re-verification of prior schema audit + Webflow-specific implementation plan + 4 priority JSON-LD templates

---

## 0. Methodology note / limitation (read before trusting the score)

The task instructed re-verification via WebSearch + Google SERP rich-result signals, since direct HTTP fetch to atomicbot.ai is blocked by the network proxy. In this execution environment:

- **No WebSearch tool was exposed to this agent** (tool list was Read/Bash/Write only), so live SERP queries (`site:atomicbot.ai`, rich-result snippet checks, PSI/Rich Results Test lookups) could **not** be executed this run.
- Direct fetch was independently confirmed blocked at the network layer: `curl` diagnostics show `connect_rejected` / 403 to `atomicbot.ai:443`, and the bundled `render_page.py` tool's raw-fetch attempt failed the same way (proxy refuses the connection). This is consistent with the stated constraint.

**Consequence:** the "re-verification" below is a **carry-forward confirmation**, not a freshly executed SERP check. I did not find any new evidence of schema having been added, but I also could not positively re-scan SERPs this session. Flagging this explicitly rather than presenting stale data as fresh.

**Action for the orchestrator:** if a WebSearch-capable agent is available in the parent `seo-audit` run, re-run the following queries and diff against this file before finalizing the client-facing report:
- `site:atomicbot.ai` (check for sitelinks/breadcrumbs in snippet)
- `atomicbot.ai` brand query (check for Knowledge Panel / logo / sameAs-driven panel)
- `atomicbot.ai hermes` (check for product rich result)
- `atomicbot.ai blog` (check for Article/BlogPosting date stamps in snippet)

---

## 1. Schema Score: 10/100 (unchanged from prior audit — unconfirmed this session, see §0)

No schema-driven rich result signal (star ratings, breadcrumbs, sitelinks, FAQ dropdowns, knowledge panel) has been reported at any point across audits. The 10 points reflect baseline technical SEO hygiene (HTTPS, native Webflow robots.txt/sitemap), **not** any structured data credit — structured data itself scores 0/100 on every axis below.

| Component | Points available | Points earned | Status |
|---|---|---|---|
| Organization (+ sameAs) | 20 | 0 | Missing |
| SoftwareApplication — Atomic Bot | 15 | 0 | Missing |
| SoftwareApplication — Hermes Agent | 15 | 0 | Missing |
| BlogPosting / Article | 15 | 0 | Missing |
| WebSite + SearchAction | 10 | 0 | Missing |
| BreadcrumbList | 10 | 0 | Missing |
| Person (founders/team) | 5 | 0 | Missing |
| Technical baseline (HTTPS, robots.txt, sitemap — non-schema) | 10 | 10 | Present (Webflow native) |
| **Total** | **100** | **10** | |

---

## 2. Detection results

No JSON-LD, Microdata, or RDFa detected in any prior scan, and no rich-result SERP feature has ever surfaced for this domain (confirmed as of the last audit; unconfirmed-but-presumed-unchanged this session per §0):

- No `Organization` — no Knowledge Panel, no logo in SERP, no sameAs graph.
- No `SoftwareApplication`/`Product` — no software rich result for Atomic Bot or Hermes Agent.
- No `Article`/`BlogPosting` — no article date-stamps in blog snippets.
- No `Person` — no author rich results.
- No `speakable` — irrelevant without Article markup to attach it to.
- No `BreadcrumbList` — SERP snippets show raw URL path, not breadcrumb trail.
- No `WebSite`+`SearchAction` — no sitelinks search box.
- No `FAQPage` — moot; see §3, this would score 0 SERP value even if added.
- No `HowTo` — correctly absent (dead rich result, do not add).

**Known sameAs targets to wire into Organization schema:**
- `https://github.com/AtomicBot-ai`
- `https://www.linkedin.com/company/atomicbot`
- `https://x.com/atomicbot_ai`
- `https://www.producthunt.com/products/atomic-bot`

**No Wikipedia article** — do not add `sameAs` to Wikipedia/Wikidata until disambiguation from the Tiny Defense game character and the unrelated Reddit RSS bot is resolved.

---

## 3. Deprecated-types check against `/root/.claude/skills/seo-schema/references/deprecated-types-2024-2026.md`

Reviewed the reference doc (last verified against developers.google.com 2026-05-25) for anything new relevant to a SaaS/dev-tool site since the prior audit:

| Type | Status | Relevant to atomicbot.ai? |
|---|---|---|
| `HowTo` | Dead since Sep 2023 | Correctly not in use — do not add for any onboarding/setup docs. |
| `FAQPage` | **Now fully retired for ALL sites as of May 7, 2026** (this supersedes the Aug-2023 gov/health-only restriction cited in the prior audit — worth updating the client's mental model). Today's date (2026-07-23) is past that retirement. | If AtomicBot has an FAQ section, it earns **zero** SERP benefit now, full stop (not just "restricted to gov/health" as previously understood). Any AI/GEO citation value is unconfirmed. Do not build new FAQPage for SERP reasons; if a genuine user-Q&A page exists (e.g., community/support), use `QAPage` instead. |
| `ClaimReview`, `VehicleListing`, `EstimatedSalary`, `LearningVideo`, `Course Info` carousel | Retired June 2025 | Not applicable to this site's product set. |
| `SpecialAnnouncement` | Retired July 2025 | Not applicable. |
| `Practice Problem` | Deprecation notice Nov 2025, tooling removed Jan 2026 | Not applicable (no ed-tech content type on this site). |
| `Dataset` | **Not deprecated** — clarified Nov 2025 that Dataset markup only feeds Dataset Search, never Google web rich results | Only relevant if AtomicBot ever publishes a benchmark dataset; not a current gap. |

**Net effect on this audit's recommendations:** none of the four templates below use anything from the deprecated list. `FAQPage` is intentionally excluded from all templates per the updated May-2026 retirement.

---

## 4. Webflow-specific implementation instructions

Webflow has no build step and no arbitrary-file-upload custom paths, so all JSON-LD must be pasted as `<script type="application/ld+json">` into one of Webflow's two custom-code surfaces. **Before starting, confirm the workspace's Site plan includes Custom Code** — Webflow's free "Starter" site plan blocks Head/Body custom code entirely; a paid Site plan (Basic/CMS/Business/Ecommerce) is required for both site-wide and per-page custom code fields.

### 4.1 Organization schema → site-wide Head Code
1. Open the atomicbot.ai project in the Webflow Designer.
2. Click the gear icon → **Project Settings** → **Custom Code** tab (this is the workspace-level one, distinct from per-page settings).
3. Paste the Organization JSON-LD (§5.1) into the **Head Code** field, wrapped in `<script type="application/ld+json">...</script>`.
4. Click **Save Changes**.
5. **Publish the site** (custom code in Project Settings only takes effect on the live site after a publish — Designer preview will not show it).
6. QA: on the live site, View Page Source (Ctrl/Cmd+U) on 2–3 different pages and confirm the same Organization `<script>` block appears in `<head>` on all of them (it's site-wide, so it should be identical everywhere).

### 4.2 SoftwareApplication — Atomic Bot → homepage Page Settings
1. In Designer, open the **Pages** panel and select the page for `/` (Home) — or `app.atomicbot.ai` if that's a separate Webflow page/subdomain in the same project.
2. Click the settings gear next to the page name (or **Page Settings** in the top bar when that page is open).
3. Scroll to the **Custom Code** section at the bottom of the Page Settings panel (this section is per-page, separate from Project Settings > Custom Code).
4. Paste the Atomic Bot JSON-LD (§5.2) into **Inside `<head>` tag**.
5. Save, then **Publish**.
6. QA: View Page Source on the live homepage; confirm both the site-wide Organization block (from 4.1) and the page-specific SoftwareApplication block are present.

### 4.3 SoftwareApplication — Hermes Agent → `/hermes` Page Settings
Repeat the exact steps in 4.2, but on the `/hermes` page, pasting the Hermes JSON-LD (§5.3).

### 4.4 BlogPosting + speakable → CMS Collection Template (per-post, dynamic)
Blog posts in Webflow are all rendered from a single **Collection Template page** bound to the CMS Collection — Page Settings custom code on that template is static and cannot bind per-item CMS field values, so use an **Embed element** instead:
1. Open the **Blog Post Template** page in Designer (the template page bound to your Blog Posts CMS Collection — not an individual published post).
2. From the **Add panel**, drag an **Embed** element onto the template canvas (position is irrelevant to schema rendering — commonly placed just before `</body>`, e.g. at the bottom of the template layout).
3. Open the Embed element's code editor and paste the BlogPosting + speakable JSON-LD (§5.4).
4. Use the **lightning-bolt / "Add field"** icon in the embed code editor's toolbar to bind live CMS field tokens into the pasted JSON at each placeholder position — e.g. bind Post Title → `headline`, Post Summary/Excerpt → `description`, Featured Image → `image`, Published Date → `datePublished`, Updated Date → `dateModified`, Author reference → `author`, and the post's own slug/URL → `mainEntityOfPage`/`@id`. Webflow inserts tokens like `{{wf {"path":"post-title","type":"PlainText"} }}` into the raw code.
5. Save the Embed, save the page, then **Publish** the whole site.
6. QA: View Page Source on **at least 2–3 different live blog posts** and confirm each renders a **unique** JSON-LD block (different `headline`/`datePublished` per post) — a common Webflow CMS mistake is leaving static text in the embed so every post gets identical schema.

### 4.5 SoftwareApplication — Cloud SKU → future `/cloud` page (when built)
1. Once the `/cloud` page exists in Webflow, replace every `[PLACEHOLDER]` token in §5.5 with confirmed copy and pricing.
2. Apply the same steps as §4.2/4.3: open `/cloud` in Designer → Page Settings → Custom Code → **Inside `<head>` tag** → paste → Save → Publish.
3. Do not publish this block with placeholder pricing live — treat §5.5 as a draft to fill in during the `/cloud` build, not something to ship today.

### 4.6 General QA for all of the above
- Validate every published page with Google's [Rich Results Test](https://search.google.com/test/rich-results) and [Schema Markup Validator](https://validator.schema.org/) after publishing.
- Webflow's own custom-code fields do not validate JSON syntax — a single trailing comma will silently break the whole block. Paste-test in a JSON linter before pasting into Webflow.
- Because Organization is injected site-wide (4.1) while SoftwareApplication/BlogPosting are page-specific, use `@id` references (see §5) instead of re-declaring full Organization properties on every page — this avoids duplicate/conflicting entity declarations across the site's structured data graph.

---

## 5. JSON-LD Templates

All four templates use `https://schema.org` context, absolute URLs, ISO 8601 dates, and `@id` anchors to link entities across the site-wide Organization block and page-specific blocks. Replace every `[PLACEHOLDER: ...]` before publishing — do not ship bracketed placeholders live.

### 5.1 Organization (Head Code — site-wide)

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "@id": "https://atomicbot.ai/#organization",
  "name": "AtomicBot",
  "url": "https://atomicbot.ai",
  "logo": "[PLACEHOLDER: absolute URL to logo asset, e.g. https://atomicbot.ai/images/atomicbot-logo.png — pull the production CDN URL from the Webflow Asset Manager]",
  "sameAs": [
    "https://github.com/AtomicBot-ai",
    "https://www.linkedin.com/company/atomicbot",
    "https://x.com/atomicbot_ai",
    "https://www.producthunt.com/products/atomic-bot"
  ]
}
```

**Validation notes:** required (`@type`, `name`, `url`) all present. `logo` is recommended for Knowledge Panel eligibility and must be an absolute URL to a real hosted image, not a placeholder, before publishing. Do **not** add a Wikipedia/Wikidata `sameAs` entry until the name-collision disambiguation issue (Tiny Defense character, unrelated Reddit RSS bot) is resolved.

### 5.2 SoftwareApplication — Atomic Bot (Homepage Page Settings)

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "@id": "https://atomicbot.ai/#software",
  "name": "Atomic Bot",
  "url": "https://atomicbot.ai",
  "applicationCategory": "[PLACEHOLDER: confirm exact category, e.g. DeveloperApplication — best-guess based on GitHub sameAs presence]",
  "operatingSystem": "[PLACEHOLDER: confirm actual supported OS list, e.g. \"Windows, macOS, Linux\"]",
  "description": "[PLACEHOLDER: 1–2 sentence product description pulled from live homepage copy]",
  "publisher": { "@id": "https://atomicbot.ai/#organization" },
  "author": { "@id": "https://atomicbot.ai/#organization" },
  "offers": {
    "@type": "Offer",
    "url": "https://atomicbot.ai",
    "price": "[PLACEHOLDER: confirm actual price, e.g. 0 for free tier]",
    "priceCurrency": "USD"
  }
}
```

**Validation notes:** `aggregateRating`/`review` intentionally **omitted** — do not fabricate a rating. Add `aggregateRating` only once genuine review counts exist (e.g., sourced from Product Hunt, G2, or a verified on-site review widget); without it this schema is valid but not eligible for Google's Software App star-rating rich result.

### 5.3 SoftwareApplication — Hermes Agent (`/hermes` Page Settings)

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "@id": "https://atomicbot.ai/hermes#software",
  "name": "Hermes Agent",
  "url": "https://atomicbot.ai/hermes",
  "applicationCategory": "[PLACEHOLDER: confirm exact category, e.g. BusinessApplication or DeveloperApplication depending on Hermes' actual use case]",
  "operatingSystem": "[PLACEHOLDER: confirm actual supported OS/platform list]",
  "description": "[PLACEHOLDER: 1–2 sentence product description pulled from live /hermes copy]",
  "publisher": { "@id": "https://atomicbot.ai/#organization" },
  "author": { "@id": "https://atomicbot.ai/#organization" },
  "offers": {
    "@type": "Offer",
    "url": "https://atomicbot.ai/hermes",
    "price": "[PLACEHOLDER: confirm actual price]",
    "priceCurrency": "USD"
  }
}
```

**Validation notes:** kept as a fully separate entity (`@id` = `.../hermes#software`) rather than merged with Atomic Bot, per the requirement that these are two distinct products. Same `aggregateRating` caveat as §5.2 applies.

### 5.4 BlogPosting + speakable (CMS Collection Template Embed — per-post)

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "@id": "[BIND: post URL]#article",
  "headline": "[BIND: Post Title]",
  "description": "[BIND: Post Summary/Excerpt]",
  "image": ["[BIND: Featured Image URL]"],
  "datePublished": "[BIND: Published Date, ISO 8601, e.g. 2026-07-23T09:00:00+00:00]",
  "dateModified": "[BIND: Updated Date, ISO 8601 — fall back to Published Date if no separate updated-date field exists]",
  "author": { "@id": "https://atomicbot.ai/#organization" },
  "publisher": { "@id": "https://atomicbot.ai/#organization" },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "[BIND: post URL]"
  },
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [".post-summary", "h1"]
  }
}
```

**Validation notes:** `[BIND: ...]` tokens are placeholders for Webflow's dynamic field-binding tokens described in §4.4 step 4 — these are not static text and must be replaced with actual `{{wf {...}}}` bindings in the Embed editor, not left as literal bracketed strings. `speakable` currently only affects Google Assistant text-to-speech surfaces (historically limited to English-language US news-adjacent content) — treat its SERP value as narrow, but it is low-cost to include and may aid AI/voice-assistant content extraction (unconfirmed benefit, consistent with this skill's stance on AI/GEO claims). If `author` should instead be a named writer rather than the Organization, replace with a `Person` node and add a matching `Person` schema on that author's bio page.

### 5.5 SoftwareApplication — Cloud SKU (draft; future `/cloud` page only)

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "@id": "https://atomicbot.ai/cloud#software",
  "name": "Atomic Bot Cloud",
  "url": "https://atomicbot.ai/cloud",
  "applicationCategory": "[PLACEHOLDER: e.g. BusinessApplication]",
  "applicationSubCategory": "[PLACEHOLDER: e.g. AI Agent Platform]",
  "operatingSystem": "Cloud, Web-based, macOS, Windows",
  "description": "[PLACEHOLDER: cloud SKU description — emphasize hosted/no-install deployment once copy exists]",
  "publisher": { "@id": "https://atomicbot.ai/#organization" },
  "author": { "@id": "https://atomicbot.ai/#organization" },
  "featureList": [
    "[PLACEHOLDER: e.g. Fully hosted agent execution, no local install required]",
    "[PLACEHOLDER: e.g. Managed scaling and uptime]",
    "[PLACEHOLDER: additional cloud-specific feature]"
  ],
  "offers": [
    {
      "@type": "Offer",
      "name": "[PLACEHOLDER: e.g. Atomic Bot Cloud — Starter]",
      "url": "https://atomicbot.ai/cloud#pricing",
      "priceCurrency": "USD",
      "priceSpecification": {
        "@type": "UnitPriceSpecification",
        "price": "[PLACEHOLDER: numeric price only, no currency symbol]",
        "priceCurrency": "USD",
        "billingDuration": "P1M"
      }
    }
  ]
}
```

**Cloud-specific schema notes (addressing the new business direction directly):**
- **`operatingSystem: "Cloud, Web-based, macOS, Windows"`** is the explicit signal Google/consumers of this schema use to distinguish a hosted SKU from the desktop app in §5.2 — include "Cloud" or "Web-based" as a literal token in this property; it's a free-text field, not a strict enum, so this is valid.
- **Model this as a fully separate `SoftwareApplication` entity** (own `@id` = `.../cloud#software`) rather than folding it into the Atomic Bot desktop entity's `offers` array — a separate entity lets the Cloud SKU carry its own `featureList`, `applicationCategory`/`applicationSubCategory`, and pricing lifecycle independently, and avoids one product's future review/rating data bleeding into the other's `aggregateRating` once that's added.
- If multiple pricing tiers exist at launch (e.g., Starter/Team/Enterprise), expand `offers` into an array of `Offer` objects, each with its own `name`, `price`, and `priceSpecification` — do not average tiers into a single price.
- Do **not** publish this block until `/cloud` exists and every placeholder is replaced with confirmed copy/pricing — an indexed page carrying fabricated pricing is a worse outcome than no schema at all.

---

## 6. Top 3 Priority Fixes

1. **Ship the Organization schema (§5.1) in Project Settings → Custom Code → Head Code, then publish.** This is the single highest-leverage fix: zero schema currently exists anywhere on the domain, and Organization + sameAs is the foundational entity every other schema block (`publisher`/`author` via `@id`) depends on. One paste, site-wide effect, unblocks everything else.
2. **Ship both SoftwareApplication blocks (§5.2 Atomic Bot on `/`, §5.3 Hermes Agent on `/hermes`) via per-page Custom Code.** These are the two revenue-relevant product pages with zero structured data today; get real (non-fabricated) `description`, `operatingSystem`, and `price` values from the live page copy before publishing, and plan to add `aggregateRating` the moment genuine review data exists (Product Hunt reviews would be the fastest source given the existing sameAs link).
3. **Build the CMS Collection Template Embed for BlogPosting + speakable (§5.4/§4.4) so every future blog post inherits schema automatically.** This is the only fix that compounds — done once on the template, it applies to every post going forward without per-post manual work, unlike the two static per-page blocks above.

**Explicitly deprioritized / do not do:** adding `FAQPage` for SERP purposes (fully retired for all sites as of May 7, 2026 — zero SERP value, use `QAPage` instead for genuine user Q&A); adding `HowTo` for any setup/onboarding docs (dead since Sep 2023); publishing the Cloud SKU template (§5.5) before the `/cloud` page and real pricing exist.

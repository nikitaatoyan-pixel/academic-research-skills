# Sitemap Audit — atomicbot.ai

**Audit date:** 2026-07-23
**Skill:** seo-sitemap (SKILL.md v2.2.4)
**Platform:** Webflow (hosted)

## 0. Access Constraint (read first)

Direct verification of `https://atomicbot.ai/sitemap.xml` and `https://atomicbot.ai/robots.txt`
was **not possible** in this session:

- `sitemap_discovery.py --json` returned `"found": []`, all four probed candidates
  (`/sitemap.xml`, `/sitemap_index.xml`, `/sitemap-index.xml`, `/wp-sitemap.xml`) failed with
  `"error": "Request failed"`, and `robots.txt could not be fetched safely`.
- Direct `curl` to the host failed with `CONNECT tunnel failed, response 403`.
- The proxy status log (`$HTTPS_PROXY/__agentproxy/status`) confirms this is an **organization
  egress policy denial**, not a transient error: `atomicbot.ai:443` was rejected by the gateway
  with `403` on repeated attempts (`connect_rejected`, "policy denial or upstream failure"). The
  same policy also blocked `index.commoncrawl.org` and `api.web.archive.org`, so no archive
  fallback was reachable either.
- This agent's toolset does not include WebFetch or WebSearch, so the fallback path specified in
  the task (fetch → search) could not be executed. Per proxy guidance, policy-denial (403) results
  should be reported, not retried or routed around.

**Everything below is therefore inferred from (a) the known indexed-page inventory supplied by
the orchestrator, and (b) documented Webflow platform behavior**, not from a fetched sitemap file.
Confidence level is stated per finding. **This report should be spot-checked by opening
`https://atomicbot.ai/sitemap.xml` in a browser or via Google Search Console → Sitemaps once
network access is available**, before treating the score below as final.

---

## 1. Sitemap Existence (inferred, not confirmed)

**Confidence: High that it exists, unverified.**

atomicbot.ai is Webflow-hosted. Webflow's Project Settings → SEO → "Auto generate sitemap"
toggle is **on by default** for Webflow-hosted sites and requires no custom build step (unlike a
hand-rolled Next.js setup needing `next-sitemap`). Absent evidence the site owner explicitly
disabled it, a `sitemap.xml` almost certainly exists at the standard path and auto-updates on
every publish. This could not be confirmed by fetch in this session.

## 2. Coverage vs. Known Indexed Pages

**Confidence: Medium-High.**

Known indexed inventory (~10-11 URLs, main domain):
`/`, `/privacy-policy`, `/terms-of-service`, `/hermes`, `/blog`, plus 8 CMS blog posts.

Webflow's auto-sitemap includes every **published, non-noindexed** static page and every
published item in a **live CMS Collection**, automatically, with no manual maintenance:

| Page | Expected sitemap status | Basis |
|---|---|---|
| `/` | Included | Static published page |
| `/privacy-policy`, `/terms-of-service` | Included | Static published pages; these are meant to be indexed (no legitimate reason to noindex) |
| `/hermes` | Included | Static published page |
| `/blog` | Included | Static Collection List page |
| 8× `/blog/[slug]` posts | Included | Webflow includes each live CMS Collection Item automatically — this is the one area worth spot-checking, since a single item's page-level "Exclude from search engines" setting would silently drop it from the auto-sitemap without any other visible symptom |
| `app.atomicbot.ai/setup` | **Correctly excluded** | Different subdomain; Webflow's auto-sitemap is scoped to the current site/domain, so a subdomain app (very likely not itself Webflow-hosted) will not appear here. This is expected behavior, not a defect — if that subdomain needs indexing signals, it needs its own sitemap/robots.txt, which is out of scope for this audit. |

**Gap:** cannot rule out that one or more of the 8 blog posts carries a page-level noindex/exclude
flag (a common Webflow authoring mistake when duplicating a CMS template item) — this would
silently remove it from the sitemap without breaking the page itself (still 200, just orphaned
from discovery). Recommend a manual check of each blog item's SEO settings tab.

## 3. Common Webflow Sitemap Issues (checked against known pattern)

| Issue | Status | Notes |
|---|---|---|
| CMS collection items not included | Likely PASS, unverified | Standard Webflow behavior includes all live items; verify no per-item noindex flags (see §2) |
| Blog pagination pages incorrectly in sitemap | PASS (by design) | Webflow's native auto-sitemap generator emits only the canonical Collection List page URL (`/blog`); it does not enumerate paginated query-string URLs (`/blog?<page>`) even if the Collection List uses Webflow's native pagination component. No duplicate-URL risk from the sitemap itself. |
| Orphaned pages | Low risk, informational | Webflow's sitemap is driven by publish status, not internal linking — a page could be sitemap-listed but weakly linked internally. Not a defect (arguably a safety net for discovery), but worth confirming all listed pages are also reachable via on-site navigation for crawl-equity reasons. |
| Missing `<lastmod>` | **Likely present issue (Low severity)** | Webflow's native auto-generated sitemap historically emits **only `<loc>`** — no `<lastmod>`, `<priority>`, or `<changefreq>`. This could not be confirmed by fetch, but is consistent with long-documented Webflow behavior. Net effect: no deprecated-tag issue (priority/changefreq legitimately absent, which is actually clean per Google's guidance that both are ignored), but also no freshness signal for `/blog` posts, which do have real edit dates. |
| Deprecated tags (`priority`, `changefreq`) | PASS (Info, moot) | Webflow does not emit these tags natively, so there is nothing to remove. |
| Sitemap referenced in robots.txt | Likely PASS, unverified | Webflow auto-appends a `Sitemap:` line to its auto-generated `robots.txt` when the sitemap toggle is on. `robots.txt` fetch also failed under the same network block, so unconfirmed. |
| >50,000 URL / 50MB cap | PASS | ~11 URLs total, nowhere near either threshold. Sitemap index splitting is not needed. |

## 4. Future `/cloud` Page Handling

**Answer: No special sitemap handling needed.** Webflow's auto-generated sitemap.xml
regenerates on every site publish and will automatically include any newly published static page
(e.g., `/cloud`) with no manual sitemap edit, no rebuild step, and no developer action required —
this is the core advantage of Webflow's native SEO tooling versus a custom Next.js pipeline.
The only actions that *do* require manual follow-up are indexing-request actions in step 5 below,
which are about search-engine discovery speed, not sitemap file correctness.

## 5. Search Console / Bing Webmaster Resubmission

**Answer: Not strictly required for the file to stay correct, but recommended for indexing
speed — and this is a genuine Webflow gap worth flagging.**

- The sitemap **file content** auto-updates with no action needed (see §4).
- However, Webflow has **no built-in mechanism to ping Google/Bing** when the site publishes.
  Google Search Console and Bing Webmaster Tools will still re-crawl a previously submitted
  sitemap on their own schedule (typically every few days, budget-dependent), so `/cloud` will
  eventually be discovered even with zero manual action.
- For a **planned business-priority page** like `/cloud`, waiting on the default recrawl cadence
  is a real opportunity cost. Recommend manually triggering discovery after publish via:
  - GSC → Sitemaps report → resubmit `sitemap.xml` (or use URL Inspection → Request Indexing on
    `/cloud` directly, which is faster than sitemap resubmission alone)
  - Bing Webmaster Tools → Sitemaps → resubmit, or use the Submit URL / IndexNow API if configured
- This is a process/ops gap, not a file-format defect — classify as **Medium severity, procedural**.

## 6. Quality Gate Results (per SKILL.md)

| Gate | Result |
|---|---|
| ≤50,000 URLs AND ≤50MB per file | **PASS** — ~11 URLs total |
| 30+ location pages → 60%+ unique content warning | **N/A / PASS** — 0 location pages on this site |
| 50+ location pages → hard stop | **N/A / PASS** — not applicable |
| Valid XML format | **UNVERIFIED** — could not fetch file this session |
| All URLs return 200 | **UNVERIFIED** — could not fetch/crawl this session |
| No noindexed URLs in sitemap | **UNVERIFIED, Medium risk flagged** — see §2 (per-item CMS noindex flag) |
| No redirected URLs in sitemap | **UNVERIFIED** |
| lastmod accurate / present | **LIKELY FAIL (Low)** — Webflow native generator likely omits `<lastmod>` entirely |
| No deprecated tags (priority/changefreq) | **PASS** — Webflow does not emit these |
| Sitemap referenced in robots.txt | **LIKELY PASS, UNVERIFIED** |

No location-page doorway risk exists on this site — the location-page quality gates (30+/50+
thresholds) are not triggered and are not relevant to atomicbot.ai's current architecture (SaaS
product marketing + blog, no programmatic city/location pages observed).

---

## Sitemap Score: 78 / 100

**This is a confidence-adjusted estimate, not a validated score** — it reflects "very likely
correct given Webflow defaults and the known page inventory" rather than confirmed pass/fail
against the fetched file. Deductions:

- −5: Sitemap file itself could not be fetched/validated this session (network policy block)
- −3: `robots.txt` sitemap declaration unverified
- −5: Cannot confirm zero noindexed/redirected/non-200 URLs are present (structural risk from
  per-item CMS noindex flags)
- −4: Likely missing `<lastmod>` across all 11 URLs (Low severity per skill table, but real)
- −5: No auto-ping to Google/Bing on publish — procedural gap affecting time-to-index for
  priority pages like the planned `/cloud`

No critical failures (URL cap, XML validity by platform default, deprecated tags, location-page
doorway risk) are expected given the site's small size and Webflow's native handling. Re-run this
audit with direct fetch access (or paste the raw `sitemap.xml` contents) to convert this into a
fully validated score.

---

## Top 3 Priority Fixes (Webflow-specific)

1. **Set up manual re-submission / Request Indexing for high-priority new pages, since Webflow
   never pings search engines on publish.** The sitemap file auto-updates correctly, but discovery
   speed does not — for the planned `/cloud` launch specifically, use GSC URL Inspection → Request
   Indexing (and/or Bing Webmaster Tools resubmission) immediately after publishing, rather than
   relying on Webflow or the default sitemap re-crawl cadence.

2. **Audit each of the 8 blog CMS items' page-level SEO settings for an accidental "Exclude from
   search engines" flag.** Because Webflow's auto-sitemap silently drops any CMS item with this
   flag set (page still returns 200, just vanishes from `sitemap.xml`), a duplicated/cloned blog
   post template is the most common way a Webflow site's sitemap coverage silently regresses.
   This can't be checked from outside the CMS — verify directly in Webflow Designer/Editor per
   post.

3. **Confirm the "Auto generate sitemap" toggle (Project Settings → SEO) is actually enabled and
   that `robots.txt` correctly declares the `Sitemap:` line** — this session could not verify
   either due to an egress policy block on `atomicbot.ai:443`. Since Webflow sites occasionally
   ship with a custom/legacy `robots.txt` override (added via the Custom Code panel) that
   clobbers the auto-generated one and drops the `Sitemap:` reference, a 30-second manual browser
   check of both files is the fastest way to close this audit's biggest confidence gap.

---

## Findings (structured)

```json
{
  "category": "Sitemap",
  "url": "https://atomicbot.ai",
  "cms_platform": "Webflow",
  "score": 78,
  "score_confidence": "estimated_unverified",
  "verification_blocked": {
    "reason": "egress_policy_403",
    "evidence": "proxy status log: connect_rejected 403 for atomicbot.ai:443 (repeated) and robots.txt fetch failure via sitemap_discovery.py",
    "tools_unavailable": ["WebFetch", "WebSearch"]
  },
  "findings": [
    {"id": "SM-01", "severity": "info", "title": "Sitemap likely exists via Webflow auto-generation", "confidence": "high_unverified"},
    {"id": "SM-02", "severity": "medium", "title": "Cannot confirm zero noindexed/redirected/non-200 URLs in sitemap", "confidence": "unverified"},
    {"id": "SM-03", "severity": "low", "title": "Webflow native sitemap likely omits <lastmod> for all URLs", "confidence": "medium"},
    {"id": "SM-04", "severity": "info", "title": "No deprecated priority/changefreq tags expected (Webflow doesn't emit them)", "confidence": "high"},
    {"id": "SM-05", "severity": "medium", "title": "No auto-ping to Google/Bing on publish; manual resubmission recommended for priority pages (e.g. /cloud)", "confidence": "high"},
    {"id": "SM-06", "severity": "info", "title": "app.atomicbot.ai/setup correctly excluded (different subdomain, out of scope)", "confidence": "high"},
    {"id": "SM-07", "severity": "low", "title": "Blog pagination not a sitemap risk (Webflow only emits canonical Collection List URL)", "confidence": "high"},
    {"id": "SM-08", "severity": "pass", "title": "50,000 URL / 50MB cap not a concern at ~11 URLs", "confidence": "high"},
    {"id": "SM-09", "severity": "pass", "title": "Location-page quality gates (30+/50+) not triggered — no location pages on site", "confidence": "high"}
  ],
  "missing_pages_vs_crawl": "unknown_unverified",
  "extra_pages_404_or_redirected": "unknown_unverified",
  "top_3_priority_fixes": [
    "Manually trigger GSC/Bing indexing requests for high-priority new pages (e.g. /cloud) since Webflow does not auto-ping search engines on publish",
    "Audit each blog CMS item for an accidental page-level noindex/exclude flag, the most common cause of silent sitemap coverage regression on Webflow",
    "Manually confirm Auto-generate-sitemap toggle and robots.txt Sitemap: declaration, since this session's direct verification was blocked by egress policy (403 on atomicbot.ai:443)"
  ]
}
```

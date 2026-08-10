# AI4L - Change Log


### v1.3.6 — 2026-08-10

* Clarified item 20.6: populations to avoid must appear under a fixed bolded label, or "None identified" when there are none


### v1.3.5 — 2026-08-09

* Improved URL handling

* A failing link is now repaired by the fixer in exactly one of two ways — replace the URL with one that retrieves the cited source, or remove the link — so an unreachable source can no longer send the audit into repeated rounds

* Every repaired or removed link is recorded in the audit's `## Fixes` section under a `LINK REPLACED` / `LINK REMOVED` label, one entry per link, opening with a parseable line that gives the old URL, the new URL where there is one, the tools tried, and the outcome that failed it — so a dropped source leaves a trace and a pattern of failures from one host stays visible

* A citation that never had a URL and got one is recorded as `LINK ADDED`, keeping it out of the failure counts

* Bright Data is now explicitly optional in the URL verification chain; its absence is no longer stated as a reason to fail a link

* A paywalled page now counts as loaded and fails on content instead of failing as a load error

* The `Loaded` column uses 🟢/🔴 instead of ✅/❌


### v1.3.4 — 2026-08-07

* Tightened Recommended Reading: database, registry and directory entries are now excluded, an item from a priority expert must independently qualify on its own merits, and an item admitted via mechanism or therapeutic category must name that shared mechanism or target in its annotation (new items 9.11, 9.12, 9.24)


### v1.3.3 — 2026-08-06

* Added Lifespan.io (lifespan.io) as a trusted source for the Recommended Reading section


### v1.3.2 — 2026-08-04

* Removed the `model: sonnet` pin from `er-combiner` — sonnet was a floating alias that could silently drift to a newer Sonnet release; the agent now inherits the host session's model instead


### v1.3.1 — 2026-08-02

* ClinicalTrials.gov links are now verified directly via `d-clinicaltrialsgov` (`clinicaltrials_get_study_record`) instead of going through the URL fallback chain

* The `Verified By` column now takes one of a fixed set of values, so the same tool is always recorded under the same name


### v1.3.0 — 2026-08-01

* Added Bright Data as a third-tier URL verification fallback (`scrape_as_markdown`) for pages protected by bot detection, after `d-browser` and `d-fetch`

* Replaced the `HTTP Status Code` column in the URL Verification table with `Loaded` and `Verified By`, since a bot-wall page can still return a 200

* PubMed links are now verified directly via `pubmed_fetch_articles` instead of going through the URL fallback chain


### v1.2.0 — 2026-07-17

* Added Grok CLI support with an @agent translation layer that maps AI4L's `@er-*` agent calls to Grok's `spawn_subagent` tool


### v1.1.0 — 2026-06-16

* Improved URL verification processes to better handle bot detection / rejection scenarios

* Switched primary URL verification from `d-fetch` to `d-browser` with `d-fetch` as a fallback

* Disallowed PDFs as viable link targets, for better verification


### v1.0.0 — 2026-05-15

* 1st public release

* Initial ER/QA definition with >= 390 checklist items

* ER generation process produces repeatable, consistent results (Opus 4.7, GPT 5.5)

* QA process to audit ERs is working consistently

* Full product cycle working: Creation > Audit > Fix > Audit > Fix > ... (Opus 4.7, GPT 5.5)

* CLI / agent-based workflow for Claude Code, OpenAI Codex, and OpenCode

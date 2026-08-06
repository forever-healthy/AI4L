# AI4L - Change Log


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

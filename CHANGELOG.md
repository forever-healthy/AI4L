# AI4L - Change Log


### v1.3.13 — 2026-08-22

* A url the fixer adds, removes or replaces is now recorded as a header-only table of three pipe-separated fields on its own line under the fix's label — one uniform shape for `LINK REPLACED`, `LINK REMOVED` and `LINK ADDED`, and an edit that leaves the url untouched is no longer reported as a url change


### v1.3.12 — 2026-08-19

* Added item 5.6: no link points to PubMed Central — an article available on PMC is linked by its PubMed ID (former items 5.6–5.11 renumbered to 5.7–5.12)


### v1.3.11 — 2026-08-19

* When a conversation stays on an intervention Evipedia has not reviewed, PERSONA.md now offers to propose it to the Evipedia editorial team, or to generate an Evipedia-style review immediately with AI4L


### v1.3.10 — 2026-08-17

* Rewrote PERSONA.md as a system prompt aligned with AI4L.md, covering ER-style intervention analysis and live URL verification before any link is shown

* PERSONA.md now looks up existing evidence reviews on evipedia.ai before answering from memory


### v1.3.9 — 2026-08-14

* Added item 5.11: link text describing a specific study, trial, or finding resolves to that study, not to a reference work, monograph, database entry, or review that merely mentions it


### v1.3.8 — 2026-08-13

* Changed items 16.23/18.23: "Not quantified in available studies." now costs a sentence stating why the literature gives none, instead of standing alone

* Added items 16.31 and 18.31: each graded item except Speculative cites at least one PubMed link

* The URL verification chain now names retrieval tiers instead of a provider — `d-browser`, `d-fetch`, `d-proxy-1`, `d-proxy-2` — so a provider can be swapped in the MCP config without changing the prompt


### v1.3.7 — 2026-08-10

* Added length caps across the checklist to curb model wordiness: annotation and item limits in sections 9–13 and 17–28, and word ranges for the Mechanism of Action, Historical Context and Monitoring narratives (new items 9.28, 10.10, 11.12, 12.12, 13.22, 14.6, 15.7, 17.8, 19.8, 20.11, 21.7, 22.13, 23.6, 24.6, 25.6, 26.7, 27.14, 28.15)

* Changed items 16.17/18.17: the annotation cap now scales with evidence level — at most 90 words for High and Medium, 50 for Low, 35 for Speculative


### v1.3.6 — 2026-08-10

* Added item 2.12: a finding attributed to a specific paper, or to a body of reviews, carries a link to that paper or to one of those reviews

* Changed items 9.12/9.13: up to 5 items are listed, each qualifying on its own merits, and where fewer qualify the shortfall is explained to the user

* Added item 13.9: where the intervention involves a trade-off, the listed papers must cover both sides of it, or the section states which side is unrepresented

* Added items 16.2 and 18.2: each benefit and each risk is a distinct outcome, not the same one restated at another granularity, split by population or endpoint, or repeated under another evidence level

* Clarified items 16.17 and 18.17: the annotation is typically 60–80 words and never more than 90

* Clarified items 16.22/16.23 and 18.22/18.23: the magnitude line takes an outcome figure where the literature has one, otherwise the direction plus its conditions, and only then "Not quantified in available studies."

* Clarified item 20.6: populations to avoid must appear under a fixed bolded label, or "None identified" when there are none

* Added item 27.6: each Optimal Functional Range cell states a value or range, or says no established target exists and gives what to track instead


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

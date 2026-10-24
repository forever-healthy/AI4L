# AI4L - Change Log


### v2.0.2 — 2026-10-24

#### Support for Combination Treatments

* AI4L can now create evidence reviews for two or more interventions given together. Following the pattern `Combining A, B & C [for goal]`.

Already live in production on [evipedia.ai](https://evipedia.ai/)

* [Combining Dasatinib & Quercetin as a Senolytic Therapy](https://evipedia.ai/dasatinib_quercetin_senolytic)

* [Combining Statins & Ezetimibe to Lower LDL](https://evipedia.ai/statins_ezetimibe_ldl)

* [Combining Pravastatin & Ezetimibe to Lower LDL](https://evipedia.ai/pravastatin_ezetimibe_ldl)

* [Combining PDE5 Inhibitors & Statins to Treat Cancer](https://evipedia.ai/pde5_inhibitors_statins_cancer)

* [Combining Ivermectin, Mebendazole & Fenbendazole to Treat Cancer](https://evipedia.ai/ivermectin_mebendazole_fenbendazole_cancer)

* [Combining Daraxonrasib, Afatinib & SD-36 to Treat Cancer](https://evipedia.ai/daraxonrasib_afatinib_sd_36_cancer)

* [Combining Spirulina, Chlorella, MCP & Modified Alginate Complex for Heavy Metal Detoxification](https://evipedia.ai/spirulina_chlorella_mcp_modified_alginate_complex_detox)


#### Improved Evidence Handling

The ER prompt was reviewed against multiple leading frontier models (Opus 5.5, GPT-6, Grok 4.7, Gemini 4 Argon & Muse Wonder Full) on about 100 evidence reviews of the same topics, and refined where the comparison showed systematic strenghts/weaknesses

* Research results: contrary trials and reviews are searched for, cited in the item, flagged ⚠️ Conflicted and weighed; evidence from a different dose, form, route, or compound is graded as indirect; High now needs more than one independent research group; retracted papers are excluded, and errata are applied

* Weighing contrary evidence: a systematic review or meta-analysis outranks single trials, a larger trial a smaller one, and several consistent trials a single one; evidence outweighed by stronger contrary evidence is graded Low; a trial too small to detect the effect is noted, not weighed

* Clinical trial results: effect sizes are the difference between groups with their confidence interval, uncontrolled figures are labeled, side-effect rates come with the placebo rate, and absolute figures are reported only from the source, never calculated; finished trials are no longer described as ongoing, and each ongoing trial states what its result would change

* Interactions: published interaction studies are searched for first, and a study that found no interaction is reported as such; an interaction inferred only from mechanism is marked "(theoretical)", or the section says so once when that applies to all of them; "Caution" and "Monitor" labels are dropped, since every listed interaction warrants them, and a severity label remains only for an absolute contraindication or an "Avoid"; thresholds for populations to avoid are sourced

* Reference values: an optimal functional range is given only with a named source; otherwise, the standard lab range or "No established target"; each biomarker is a safety check or a marker the intervention changes; thresholds, doses, and protocol parameters are cited or stated as common practice

* Sources: regulatory and legal facts carry a link to a source that states them (a news report, labeled as such, may stand in until the regulator publishes the fact), and no cited source is overstated or stretched to a comparison it did not make; where a paper's abstract and full text disagree, the review follows the full text and notes the discrepancy

* Completeness: an omitted benefit fails the audit only if it would be graded High, is an approved indication, is found by a systematic review or meta-analysis, or bears directly on a specific goal; an omitted risk fails if it would be graded Medium or High, is on the label, or is serious. An item that already cites contrary evidence fails only for contrary evidence that would change its grade; other findings are noted without failing, so audits can reach 100%

* Checklist consolidated: contradictions between items resolved (1.4, 1.9/1.17, 2.8/2.10/29.5, 20.10/20.11, 28.6), several duplicate items merged, hints now apply to authors as well

* Sections 16 (Benefits) and 18 (Risks) reordered: items and grading scale first, then grade checks, completeness, contrary evidence, layout, magnitude, and sources.

* Empty evidence levels: every level without items, Low and Speculative included, now carries one sentence naming the class of evidence that falls short, instead of a bare heading


#### New Models successfully tested

* Google Gemini 4 Aragon - using Antigravity

* Meta Muse Wonder Full - using Muse Code


#### New Environments Supported

* **Google Antigravity:** (`agy`), with the agents and MCP servers configured as the `ai4l` plugin in `.agents/plugins/ai4l/` and the skills registered in `.agents/skills.json`. Its permissions live only in the global settings, so merge the provided template once — see [Using CLI Environments](docs/Using-CLI-Environments.md#antigravity-setup)

* **OpenCode 2:** (`opencode`), with its own installer; the agents run as subagents and web search works out of the box — see [Using CLI Environments](docs/Using-CLI-Environments.md#opencode-setup)


#### Updated Environments

* **Muse Code** now runs the agents as its own typed subagents from `.agents/agents/`. Muse keeps no project-level permissions, so start interactive sessions with `muse --approval-mode never` — see [Using CLI Environments](docs/Using-CLI-Environments.md#muse-code-setup)

* **Codex** now spawns the agents with an underscore task name (`er-creator` → `er_creator`), the only form it accepts, and no longer asks for approval on every MCP tool call, as each MCP server sets `default_tools_approval_mode = "approve"`

* **Claude Desktop** now runs the four local MCP servers (`d-browser`, `d-fetch`, `d-pubmed`, `d-clinicaltrialsgov`) via `npx` and `uvx`, no Docker needed, under the names ER.md records in `Verified By`; the built-in Clinical Trials and PubMed connectors stay off, as in the CLIs — see [Using Claude Desktop](docs/Using-Claude-Desktop.md#install-the-mcp-servers)

* **Gemini CLI** is no longer supported, as Google no longer serves it to individual accounts; `GEMINI.md` and `.gemini/` are removed. Use Antigravity for Gemini models


#### Other Changes

* The main prompt is renamed from `prompts/AI4L.md` to `prompts/ER.md`; update any local copies or links

* Project instructions now live in `AGENTS.md` for all environments; `CLAUDE.md` imports it, so Codex and OpenCode now also get the shared globals and rules

* Examples now link to the live reviews and audits on evipedia, which stay current with the prompt; the outdated examples folder is removed

* The creator, fixer, and auditor use web search and the research MCPs as the prompt requires; their read limits apply to local files only

* Model names: the creator and auditor take their model name and version from their system prompt instead of guessing it, without product, effort, or pricing names (e.g., `Opus 5.5` / `Opus`, `Gemini 4 Argon` / `Gemini-Argon`); variants now also differ in the filename. A model whose system prompt names no version is recorded by its bare name (e.g., `Grok` for Grok Build subagents); one that names no model at all is recorded as `Unknown`

* Obstacle Protocol: a run stops only when a sub-agent itself cannot continue (it crashes, hits a hard limit, or a required agent, tool, or file is missing); a failed call such as an unreachable website, a 403, a bot wall, or a timeout is recorded and retried or passed to the next retrieval tier, never a reason to stop

* Frontmatter: `initial_topic` and `topic` are always wrapped in double quotes (6.2–6.5, 6.34); other values are quoted only where YAML requires it


### v1.3.29 — 2026-09-18

* Muse Code is documented as a tested environment, running Muse Spark 1.3 with the Codex agents and instructions


### v1.3.28 — 2026-09-17

* The motivation no longer states or hints at how results turned out; a headline finding says what was studied, not what it showed


### v1.3.27 — 2026-09-13

* The short goal is taken from the wording of the goal itself and never a synonym, so the same topic always yields the same filename and address


### v1.3.26 — 2026-09-12

* A combination review lists each agent's own names and abbreviations among the alternate names again, so a search for one agent finds the review


### v1.3.25 — 2026-09-12

* The Grokipedia, Examine and ConsumerLab sections now search through every retrieval tier before calling an article absent, and an absence is challenged only when the auditor actually retrieves the article — a site that answers inconsistently can no longer send the audit loop back and forth

* Alternate names are plain names, one entry each, so every brand name and abbreviation reaches the search index


### v1.3.24 — 2026-09-11

* Two checklist items now lead with the condition they test, so an audit marks them not-applicable instead of passing them unexamined

* Dropped the item asking whether a url looked guessed — intent cannot be observed, and every link is already verified by retrieving it


### v1.3.23 — 2026-09-11

* A combination of two or more agents is now one intervention: the title reads "Combining ...", the reason the agents are given together is stated, and every benefit and risk says whether the evidence tested the combination or a single named agent
* A benefit that does not bear on the review's goal is kept but flagged "⭕️ Not Central to ...", so an off-goal effect no longer leads the evidence list


### v1.3.22 — 2026-09-09

* Route of administration stays clinical on every surface, including Motivation and Conclusion — "oral" / "administered orally", never "taken by mouth"


### v1.3.21 — 2026-08-27

* All four ER agents now state an explicit read scope, so an agent reads only the review it was given, that review's QA files and the prompt — by any means of access, shell commands, and globs included


### v1.3.20 — 2026-08-25

* The persona proposes an intervention to the Evipedia editorial team with the renamed `suggest_review` MCP tool


### v1.3.19 — 2026-08-25

* The persona now renders every Evipedia review title as a markdown link to the review's permalink, never as plain text


### v1.3.18 — 2026-08-24

* Every url in the fixer's URL-change record is now an inline markdown link, so the record's urls render as clickable hyperlinks instead of plain text


### v1.3.17 — 2026-08-23

* The fixer's URL-change record is now a headerless single-row table, so it renders as a table instead of a line of pipes


### v1.3.16 — 2026-08-23

* Added item 5.13: no link points to a web archive or cache, and an archive copy is never a valid replacement for a url that will not load


### v1.3.15 — 2026-08-22

* Defined the four evidence levels (new items 16.32/18.32), required all four level headings even when empty (16.33/18.33), the net reading of conflicted evidence (16.21/18.21), and the search statement in an HTML comment (16.4/18.4)


### v1.3.14 — 2026-08-22

* PERSONA.md opens with a summary of the features it adds to a conversation and links the AI4L repository


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

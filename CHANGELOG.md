# AI4L - Change Log


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

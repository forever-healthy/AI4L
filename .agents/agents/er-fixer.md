---
name: er-fixer
description: AI4L - audit & fix ER
---

# AI4L - Agent to Audit & Fix Evidence Reviews according to ER.md

* Set [evidence_review] to the Evidence Review defined in your instructions

* Set [audit_ai_fullname] to the model name, version number and variant (if any) as stated in your system prompt or developer instructions, written as "[name] [version]" or "[name] [version] [variant]". [name] is the word or words directly before the version number; if there is more than one, leave out the first (the vendor or model family) (e.g., Claude Opus 5.5 → Opus 5.5, gemini-3.8-flash → Gemini 3.8 Flash, acme-nova-max-2.0 → Nova Max 2.0). Leave out product, effort and pricing names, also when attached by a hyphen (e.g., Codex, Build, high, free, preview), and capitalize every word. Never use a model name or version from memory. If your system prompt or developer instructions name a model without a version number, [audit_ai_fullname] is that name alone, without a version (e.g., "a Grok Build subagent" → Grok). If they name no model at all, set [audit_ai_fullname] to "Unknown"; a name you believe to be yours does not count.

* Set [audit_ai_nickname] to [audit_ai_fullname] without the version number, with all remaining words joined by hyphens (e.g., Opus, Gemini-Flash, Nova-Max). If [audit_ai_fullname] has no version number or is "Unknown", [audit_ai_nickname] equals [audit_ai_fullname].

* Report `FIXER: [audit_ai_fullname] / [audit_ai_nickname]`

* If you resume from a summary of earlier work, take [audit_ai_fullname] and [audit_ai_nickname] from the values reported there; never decide them again.

* READ SCOPE — you may read ONLY:
  - [evidence_review]
  - the audit file you create during this run
  - [er_prompt]
  - files you yourself created in [tmp_dir] during this run

* Never read any OTHER evidence review, and never read any _ER_QA.md file other than the one you create in this run — prior audits of this same review included, regardless of where they live or what they are named. If [creation_dir] holds other reviews alongside the one you are working on, they are out of scope too.

* Never glob, list or search for evidence reviews or _ER_QA.md files.

* Applies to ANY means of access, not just the Read tool: `cat`, `head`, `tail`, `sed`, `awk`, `grep`, `diff`, `less`, `ls` and any glob or file search count the same.

* An audit or a fix is derived from [evidence_review] and [er_prompt] — never from another review.

* READ SCOPE covers local files only. Web search and the MCP research tools (PubMed, ClinicalTrials.gov, fetch, browser) are NOT restricted by it — use them as [er_prompt] requires.

* Report `FIXER: evidence_review: [evidence_review]`

* Read [er_prompt] in chunks of 250 lines, from the top to its end

* Read [evidence_review] in chunks of 250 lines, from the top to its end

* Perform a QA audit on [evidence_review] by following the instructions in [er_prompt]

* If the [pass_rate] of the audit is not 100% {

  * Report `FIXER: pass_rate: [pass_rate]. Will fix the issues found in the audit.`
  
  * Fix the issues found in [evidence_review] during the audit

  * When fixing a link that failed 5.9, 5.10 or 5.12, repair it in exactly one of two ways: replace the URL with one that retrieves the cited source and itself satisfies Section 5, or remove the link and its annotation when no such URL exists. Never leave a failing link in place, and never rewrite the annotation to match a different page.

  * Save the fixed version of the [evidence_review]

  * Report `FIXER: ER fixed: [evidence_review]`
  * Report `FIXER: Documenting the fixes in a markdown file...`
  
  * Create a [temporary_file] in [tmp_dir], named like [audit_filename] with `.md` replaced by `_fixes.md`, with the following content:

  * Start with `## Fixes [audit_date reformatted as '%m/%d/%Y %H:%M']`

  * Then note all the fixed issues in this format:
  
    - Analog to the `## Issues` section, but with the following changes/similarities:

    - Numbered list

    - Each entry: `1. **{item# of the issue} — {short label, a 3-6 word condensed summary}:** {What was changed, from-to if helpful. 1-2 sentences max.}`

    - `{item# of the issue}` MUST be the exact section.item reference from the `## Issues` section (e.g., `6.1`, `17.2`), NOT the sequential list position number

    - Bold covers only the item number(s) and the {short label} , followed by a colon

    - If one fix covers multiple items, combine: `**4.1 / 4.4 — {short label}:**`

    - Example: `3. **5.5 — PMC link format:** Replaced PMC link for Johnston et al. 2006 with s format.` (here `3` is the list position, `5.5` is the issue reference — they are different)

  **URL Changes**
    
    - When a fix adds, removes or replaces a url, what follows the bold `**{item#} — {short label}:**` prefix is, after a blank line and indented to the list item's text column, a machine-parseable record as a single-row headerless table with three pipe-separated fields: `| <MARKER>: <urls> | tried: <tools> | outcome: <why> |`

    - The record row stands alone: no header row, and no `| --- | --- | --- |` after it — a trailing delimiter row stops it from rendering as a table.

    - Every url in the `<urls>` field is written as an inline markdown link whose link text is the url itself — `[https://example.org/page](https://example.org/page)` — so it renders as a clickable hyperlink. Never leave a bare url; it renders as plain text. When the url itself contains parentheses, wrap the destination in angle brackets: `[https://example.org/a_(b)](<https://example.org/a_(b)>)`.

    The ER's URL table lists only what it still cites, so once a source is dropped or swapped this record is the only evidence it was ever there.

    - One table per fix — never batch several fixes into one entry. Prose may follow the record, at the same indent.

    The cases are:

    - `LINK REPLACED` — `<urls>` is `<failing-url> -> <new-url>`. Both urls occupy that single field, so a replacement stays three fields like the others.

    - `LINK REMOVED` — `<urls>` is the dropped url. Record it even when the removal was obviously correct (dead URL).

    - `LINK ADDED` — `<urls>` is the new url and `tried:` is `—`. This is not a repair and must not be counted as one.

    - The short label of the entry describes the fix and must not restate the marker: write `**5.11 — link text named a secondary source:**`, not `**5.11 — LINK REPLACED:**`.

    - A change that leaves the url untouched is not a reportable url change. Editing only the link text or the annotation is an ordinary fix entry, not recording it as a `LINK REPLACED`.

  * Report `FIXER: Fixes documented. Combining QA audit results and fixes...`
  * Append the content of [temporary_file] to the end of [audit_filename]
  * Move the [temporary_file] to [trash_dir]
}

* Run: `sed -i 's|</content>||g; s|</invoke>||g' [audit_filename]`
* This command strips every `</content>` and `</invoke>` token from the [audit_filename] — including ones glued to the next line like `</invoke>## Fixes ...` — which are artifacts from the audit process and should not be present in the final document. (Opus BUG)

* CRITICAL: Do not assume a 100% pass rate only because you fixed the issues found in this pass. Due to the heuristic nature of AI, other issues may still exist that were not detected in this audit.

* CRITICAL: Never claim, imply, mention, or add any text about "100%", "post-fix", "now meets all", "effective pass rate 100%", or success after fixing. Report ONLY the exact pre-fix audit pass_rate. Narrative must match the reported number.

* Report `FIXER: Done with [evidence_review]`
* Report `FIXER: audit_filename: [audit_filename]`
* Report `FIXER: pass_rate: [pass_rate]` of the audit (the one before the fixing step)

* Return `audit_filename: [audit_filename]`
* Return `pass_rate: [pass_rate]` of the audit (the one before the fixing step)

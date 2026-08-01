---
name: er-fixer
description: AI4L - audit & fix ER
model: opus
color: green
version: 26.8.1
---

# AI4L - Agent to Audit & Fix Evidence Reviews according to AI4L.md

* Set [evidence_review] to the Evidence Review defined in your instructions

* Report `FIXER: evidence_review: [evidence_review]`

* Read [ai4l_prompt] lines 000 - 249
* Read [ai4l_prompt] lines 250 - 499
* Read [ai4l_prompt] lines 500 - 749
* Read [ai4l_prompt] lines 750 - 999
* Read [ai4l_prompt] lines 1000 - 1249
* Read [ai4l_prompt] lines 1250 - 1499

* Read [evidence_review] lines 000 - 249
* Read [evidence_review] lines 250 - 499
* Read [evidence_review] lines 500 - 749
* Read [evidence_review] lines 750 - 999

* Perform a QA audit on [evidence_review] by following the instructions in [ai4l_prompt]

* If the [pass_rate] of the audit is not 100% {

  * Report `FIXER: pass_rate: [pass_rate]. WIll fix the issues found in the audit.`
  
  * Fix the issues found in [evidence_review] during the audit
  * Save the fixed version of the [evidence_review]

  * Report `FIXER: ER fixed: [evidence_review]`
  * Report `FIXER: Documenting the fixes in a markdown file...`
  
  * Create a [temporary_file] `[audit_filename]_fixes.md` with the following content:

  * Start with `## Fixes [audit_date reformatted as '%d/%m/%Y %H:%M']`

  * Then note all the fixed issues in this format:
    - Analog to the `## Issues` section, but with the following changes/similarities:
    - Numbered list
    - Each entry: `1. **{item# of the issue} — {short label, a 3-6 word condensed summary}:** {What was changed, from-to if helpful. 1-2 sentences max.}`
    - `{item# of the issue}` MUST be the exact section.item reference from the `## Issues` section (e.g., `6.1`, `17.2`), NOT the sequential list position number
    - Bold covers only the item number(s) and the {short label} , followed by a colon
    - If one fix covers multiple items, combine: `**6.1 / 6.10 — {short label}:**`
    - Example: `3. **7.5 — PMC link format:** Replaced PMC link for Johnston et al. 2006 with s format.` (here `3` is the list position, `7.5` is the issue reference — they are different)

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

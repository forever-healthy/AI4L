---
name: er-creator
description: AI4L - create ER
---

# AI4L - Agent to Create Evidence Reviews according to ER.md

* Set [topic] to the topic defined in your instructions

* Report `CREATOR: Create ER for [topic]`

* Run `date +%s` and save the result as [start_time]

* Set [creator_ai_fullname] to the model name, version number and variant (if any) as stated in your system prompt or developer instructions, written as "[name] [version]" or "[name] [version] [variant]". [name] is the word or words directly before the version number; if there is more than one, leave out the first (the vendor or model family) (e.g., Claude Opus 5.5 → Opus 5.5, gemini-3.8-flash → Gemini 3.8 Flash, acme-nova-max-2.0 → Nova Max 2.0). Leave out product, effort and pricing names, also when attached by a hyphen (e.g., Codex, Build, high, free, preview), and capitalize every word. Never use a model name or version from memory. If your system prompt or developer instructions name a model without a version number, [creator_ai_fullname] is that name alone, without a version (e.g., "a Grok Build subagent" → Grok). If they name no model at all, set [creator_ai_fullname] to "Unknown"; a name you believe to be yours does not count.

* Set [creator_ai_nickname] to [creator_ai_fullname] without the version number, with all remaining words joined by hyphens (e.g., Opus, Gemini-Flash, Nova-Max). If [creator_ai_fullname] has no version number or is "Unknown", [creator_ai_nickname] equals [creator_ai_fullname].

* Report `CREATOR: [creator_ai_fullname] / [creator_ai_nickname]`

* If you resume from a summary of earlier work, take [creator_ai_fullname] and [creator_ai_nickname] from the values reported there; never decide them again.

* Read [er_prompt] in chunks of 250 lines, from the top to its end

* READ SCOPE — you may read ONLY:
  - [er_prompt]
  - files you yourself created in [tmp_dir] during this run

* Never read any existing evidence review or _ER_QA.md file, and never read any other existing file — not in [creation_dir], not in any other directory. Create the ER from [er_prompt], your knowledge, and the research [er_prompt] requires.

* READ SCOPE covers local files only. Web search and the MCP research tools (PubMed, ClinicalTrials.gov, fetch, browser) are NOT restricted by it — use them as [er_prompt] requires.

* Never glob, list or search for evidence reviews or _ER_QA.md files.

* Applies to ANY means of access, not just the Read tool: `cat`, `head`, `tail`, `sed`, `awk`, `grep`, `diff`, `less`, `ls` and any glob or file search count the same.

* Create an evidence review for [topic] that can pass a QA audit as described in [er_prompt]

* Run `date +%s`, subtract [start_time], convert the elapsed seconds to HH:MM format, and add it as `duration: \"HH:MM\"` in the frontmatter of the result

* Save the result as an .md file in [creation_dir] using the [filename] given in the result

* Report `CREATOR: Done with [topic]`
* Report `CREATOR: Created [filename]`

* Return `filename: [filename]`

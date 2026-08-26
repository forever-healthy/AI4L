---
name: er-creator
description: AI4L - create ER
model: opus
color: purple
version: 1.3.21
---

# AI4L - Agent to Create Evidence Reviews according to AI4L.md

* Set [topic] to the topic defined in your instructions

* Report `CREATOR: Create ER for [topic]`

* Run `date +%s` and save the result as [start_time]

* Read [ai4l_prompt] lines 000 - 249
* Read [ai4l_prompt] lines 250 - 499
* Read [ai4l_prompt] lines 500 - 749
* Read [ai4l_prompt] lines 750 - 999
* Read [ai4l_prompt] lines 1000 - 1249
* Read [ai4l_prompt] lines 1250 - 1499

* READ SCOPE — you may read ONLY:
  - [ai4l_prompt]
  - files you yourself created in [tmp_dir] during this run
* Never read any existing evidence review or QA file, and never read any other existing file — not in [creation_dir], not in any other directory. Create the ER solely from [ai4l_prompt] and your knowledge.
* Never glob, list or search for evidence reviews or QA files.
* Applies to ANY means of access, not just the Read tool: `cat`, `head`, `tail`, `sed`, `awk`, `grep`, `diff`, `less`, `ls` and any glob or file search count the same.

* Create an evidence review for [topic] that can pass a QA audit as described in [ai4l_prompt]

* Run `date +%s`, subtract [start_time], convert the elapsed seconds to HH:MM format, and add it as `duration: \"HH:MM\"` in the frontmatter of the result

* Save the result as an .md file in [creation_dir] using the [filename] given in the result

* Report `CREATOR: Done with [topic]`
* Report `CREATOR: Created [filename]`

* Return `filename: [filename]`

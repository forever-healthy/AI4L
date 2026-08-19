---
name: er-auditor
description: AI4L - audit ER
model: opus
color: red
version: 1.3.11
---

# AI4L - Agent to Audit Evidence Reviews according to AI4L.md

* Set [evidence_review] to the Evidence Review defined in your instructions

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

* Perform an QA audit on [evidence_review] by following the instructions in [ai4l_prompt]

* Return `audit_filename: [audit_filename]`
* Return `pass_rate: actual [pass_rate] of the audit`

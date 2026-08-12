![Version 1.3.8](https://img.shields.io/badge/Version-1.3.8-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Our Experience: Challenges and Solutions

* We found tremendous variability in the quality of reviews, audits, and fixes across models. [Discussion of individual model performance](https://github.com/forever-healthy/AI4L/discussions/categories/models).

* Models got confused when doing the whole audit in one pass. To counter this, we split the process into multiple steps.

* Models had issues converting the long checklist from a bulleted list into a table. They hallucinated new items, dropped others, or reformulated them. We "fixed" that by individually numbering all checklist items and adding many explicit "DO NOT" instructions to the review process.

* All models are currently horrible at simple math. They cannot reliably count items in a list, which is needed to calculate the pass/fail rate for a QA review. We "fixed" that by allowing the model to create and run a script that counts the items in a list, rather than doing it itself.

* All models hallucinate when it comes to URLs. They often make up URLs or link to completely unrelated documents. We "fixed" that by asking the auditor to both verify each URL and the content the URL links to.

* Models might mistakenly assume truthfulness of facts in prior conversations in the same session, even though they might be hallucinations (session bias). Therefore, we always perform audits/fixes using @agents. @agents get a clean session with no conversation history and a controlled context. It also prevents an ever-growing, bloated session context.

* When doing audits, the auditing agent must not use sub-agents itself (e.g., for table generation in parallel). Parallel sub-agents would not have the master agent's full context and would miss information known to the agent.

* Due to the heuristic nature of AI, models can miss issues during an audit. Even after fixing all issues found during the audit, some may remain. After fixing, we perform another audit to verify the corrections and check for overlooked issues. We repeat this process until at least one audit shows a 100% pass rate.

* A 100% clean pass on a first audit is suspicious (actually only happened to us when testing smaller models) and should be taken with a grain of salt.

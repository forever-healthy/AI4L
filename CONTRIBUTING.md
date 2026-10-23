![Version 2.0.2](https://img.shields.io/badge/Version-2.0.2-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# How to Contribute to AI4L

First off, thanks for taking the time to contribute!

Your [feedback & discussions](https://github.com/forever-healthy/AI4L/discussions) are always welcome!

Just reach out to us via GitHub discussions, and we will be happy to chat about your ideas, suggestions, or questions.

Below, you will find a brief guide to evolving the ER prompt ([ER.md](prompts/ER.md)) and customizing it for your needs.


## Customizing the Expert List

The list of experts prioritized for the "Recommended Reading" section can be modified by changing [ER.md (9.5)](prompts/ER.md#9-recommended-reading)


## Developing ER.md

* Use a [CLI Environment](docs/Using-CLI-Environments.md)

* Agents & skills are located in `.claude/`; the copies for the other environments (`.opencode/`, `.codex/`, `.agents/`) are generated from it

* After modifying files, use "/check" to check all AI4L files for inconsistencies or errors. The skill should be auto-loaded by the CLI.

* The CLI might need a `/clear` or even an exit from the CLI to reload the changed agent or skill code


### Step #1: Make sure the QA process is working flawlessly

  * Use `/er create <topic>` to create an evidence review

  * Use `/er audit` to audit the review

  * Inspect the resulting _ER_QA.md for any errors, inconsistencies, or misbehavior of the auditing AI.

  * Fix issues by improving the audit instruction at the beginning of "ER.md" and throughout the document as `inline comments for the auditor`.

  * This can be done interactively, in dialogue with the AI itself, by asking it how to improve the prompt to avoid a particular mistake or misunderstanding in the future.

  * Repeat until satisfied

  * Proceed to step 2 **ONLY WHEN** repeatedly happy with the behavior of the QA process itself.


### Step #2: Focus on the actual content of the ER

  * When unhappy with the actual content of the ER, interactively work with the AI on how to improve / change / add checklist items to steer the generating AI in the desired direction.
  * Remember the reverse, QA based approach as described in [README.md](README.md)

  * QA items work best when they are not only negative (DOES NOT ...) but also clearly express what is expected and give a precise example.

  * Repeat the cycle " Create > Audit > Improve audit checklist items > ... " until satisfied

  
## Lessons Learned

Check out our [Lessons Learned](docs/Lessons-Learned.md) document for insights and best practices.

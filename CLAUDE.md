![Version 1.3.5](https://img.shields.io/badge/Version-1.3.5-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Project Instructions


### Session related

* For interactive conversations, follow "prompts/PERSONA.md"


### Glossary

* "ER" stands for evidence review
* "ER.md" refers to the .md file of an ER

* "QA" stands for quality assurance audit
* "QA.md" refers to the .md file of a QA


### Timestamps

* When generating timestamps (for filenames, headers, etc.), always obtain the current date/time by running `date` in the shell. Never use the model's internal clock, as it may differ from the local timezone.


### Globals for Skills and @Agents

* [ai4l_prompt]  = "prompts/AI4L.md"

* [creation_dir] = "creation/"
* [tmp_dir] =      "creation/tmp/"
* [trash_dir] =    "creation/trash/"

* [max_audits] = 20
* [needed_passes] = 1


### Hard Rule

* NEVER modify any files outside of [creation_dir], [tmp_dir], or [trash_dir] without explicit user permission. This includes, but is not limited to, files in the root directory, configuration files, and any other files not in the listed directories.


### When Calling @Agents

* ALWAYS call the @agent exactly as specified in the prompt
* DO NOT modify the prompts for the @agent or the way to call them; pass the prompt as it is.
* DO NOT modify the arguments you pass to the @agent; pass them as they are.


## MCP Servers

When calling `d-pubmed` (`pubmed_fetch_articles`), always pass 'pmids' as a proper JSON array of strings, never as a comma-separated string or single value

When verifying ClinicalTrials.gov URLs, use `mcp__d-clinicaltrialsgov__clinicaltrials_get_study_record` first. If the NCT ID resolves, that is the verification; only if it does not resolve, fall back to the chain below.

When verifying URLs, use the `mcp__d-browser__*` tools (`browser_navigate` + `browser_snapshot`). If `d-browser` is blocked or unavailable, fall back to `mcp__d-fetch__fetch`. If that also fails, fall back to `mcp__d-brightdata__scrape_as_markdown`, which can retrieve pages protected by bot detection. Do not use the built-in `WebFetch` tool.


### Temporary Files

* When creating temporary files (scripts, working copies, etc), always create them in the [tmp_dir]


### Obstacle Protocol

* If a sub-agent fails, reaches a limit, or otherwise runs into an error that prevents the completion of a directive, you MUST immediately stop execution and report the specific problem to the user. Do not attempt to bypass errors or perform manual recovery (such as manual file edits) unless explicitly directed by the user.

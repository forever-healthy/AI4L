![Version 2.0.2](https://img.shields.io/badge/Version-2.0.2-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Project Instructions


### Session related

* For interactive conversations, follow "prompts/PERSONA.md"


### Glossary

* "ER" stands for evidence review
* "_ER.md" refers to the .md file of an ER
* "ER.md" refers to the ER prompt ([er_prompt]), never to an ER file

* "QA" stands for quality assurance; a "QA audit" is the audit of an ER against ER.md
* "_ER_QA.md" refers to the .md file of a QA of an ER


### Timestamps

* When generating timestamps (for filenames, headers, etc.), always obtain the current date/time by running `date` in the shell. Never use the model's internal clock, as it may differ from the local timezone.


### Globals for Skills and @Agents

* [er_prompt]    = "prompts/ER.md"

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

When verifying PubMed URLs, use `mcp__d-pubmed__pubmed_fetch_articles` first. If the PMID resolves, that is the verification; only if it does not resolve, fall back to the chain below.

When verifying ClinicalTrials.gov URLs, use `mcp__d-clinicaltrialsgov__clinicaltrials_get_study_record` first. If the NCT ID resolves, that is the verification; only if it does not resolve, fall back to the chain below.

When verifying URLs, use the `mcp__d-browser__*` tools (`browser_navigate` + `browser_snapshot`). If `d-browser` is blocked or unavailable, fall back to `mcp__d-fetch__fetch`. If that also fails, fall back to `d-proxy-1` and then `d-proxy-2` — bot-wall-defeating retrieval tiers; use whichever page-retrieval tool that server offers. Do not use the built-in `WebFetch` tool.


### Temporary Files

* When creating temporary files (scripts, working copies, etc), always create them in the [tmp_dir]


### Obstacle Protocol

* If a sub-agent fails, reaches a limit, or otherwise runs into an error that prevents the completion of a directive, you MUST immediately stop execution and report the specific problem to the user. Do not attempt to bypass errors or perform manual recovery (such as manual file edits) unless explicitly directed by the user.

* The Obstacle Protocol applies only when a sub-agent itself cannot continue: it crashes, hits a hard limit, or a required agent, tool, or file does not exist. A single failed call is never an obstacle: an error page, 403, bot wall, timeout, empty response, an unconfigured tier, or a tool rejecting its arguments. Fix the arguments, retry, or move to the next tier; record the failed attempt where ER.md asks for it; and continue with the directive. Never stop, and never ask the user, for a failed call.


## OpenAI Codex & Muse Code

### Canonical Paths

When running in Codex, use `.codex/skills` and `.codex/agents` as the canonical repo-local skill and agent paths. When running in Muse Code, use `.codex/skills` for skills and `.agents/agents` for agents.

These paths may be symlinks. When inspecting them from the shell, follow symlinks, for example with `find -L`.

Do not use `.claude/skills` or `.claude/agents` as the primary path unless explicitly asked or unless resolving the `.codex` symlink target.


### Sub-Agent Defaults

Never set `model` or `reasoning_effort` when spawning sub-agents unless the user explicitly asks for a specific model or reasoning effort in the current turn.

Do not set `fork_context: true` when spawning sub-agents. Sub-agents must start without inherited conversation history and receive only the explicit task instructions, relevant repo paths, globals, and target filenames needed for their assigned work.

When spawning a sub-agent, set `task_name` to the agent name with hyphens replaced by underscores (er-creator → er_creator); Codex accepts only lowercase letters, digits, and underscores there.

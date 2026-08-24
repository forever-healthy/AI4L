![Version 1.3.18](https://img.shields.io/badge/Version-1.3.18-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Grok-Specific Instructions

This file is under `.grok/rules/` so Grok auto-loads it at session start (every `*.md` in that directory).

## @Agent Translation Layer (Critical for AI4L compatibility)

Whenever the instructions, skills, or prompts contain any of the following patterns:

* `@er-creator: [something]`
* `@er-auditor: [something]`
* `@er-fixer: [something]`
* `@er-combiner: [something]`

**Automatically translate and execute them as follows:**

Use the `spawn_subagent` tool with these exact parameters:

- `prompt`: the original argument (e.g. the topic or target ER filename)
- `description`: "Execute [agent name] for [argument]"
- `subagent_type`: "er-creator", "er-auditor", "er-fixer", or "er-combiner" (matching the called agent)
- `capability_mode`: "all"
- `isolation`: "none"

Always wait for the subagent to complete and return its result (including filename or pass_rate) before continuing.

This translation must happen transparently so that the existing `.claude/skills/er/SKILL.md` (which uses the `@er-xxx:` syntax) continues to work unmodified across all environments (Claude, ChatGPT, Gemini, etc.).

This ensures the agents write directly into `creation/` as required by the AI4L skill, while still giving them full capability. (Changed from worktree isolation because it was breaking the "all results must go in creation/" rule.)

![Version 1.3.1](https://img.shields.io/badge/Version-1.3.1-green.svg)[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# OpenAI Codex Instructions


## Canonical Paths

When running in Codex, use `.codex/skills` and `.codex/agents` as the canonical repo-local skill and agent paths.

These paths may be symlinks. When inspecting them from the shell, follow symlinks, for example with `find -L`.

Do not use `.claude/skills` or `.claude/agents` as the primary path unless explicitly asked or unless resolving the `.codex` symlink target.


## Sub-Agent Defaults

Never set `model` or `reasoning_effort` when spawning Codex sub-agents unless the user explicitly asks for a specific model or reasoning effort in the current turn.

Do not set `fork_context: true` when spawning Codex sub-agents. Sub-agents must start without inherited conversation history and receive only the explicit task instructions, relevant repo paths, globals, and target filenames needed for their assigned work.

![Version 2.0.2](https://img.shields.io/badge/Version-2.0.2-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Using CLI Environments

Although CLI environments are primarily designed for software and website development, they are also great environments for creating ERs.

They offer several advantages for our use case:

* Automatic loading of skills 
* Automatic loading of settings/permissions
* Automatic loading of MCP servers

* Proper handling of @agents, avoiding session bias and context bloat
* Multiple, parallel sessions

Particularly for security reasons in regard to malicious mcp code, prompt injection, and potential errors in the CLI itself, we have two main security measures in place

* Sandboxing of all MCPs in Docker containers
* Sandboxing of the CLI itself using Docker SBX


## Tested Environments

We tested six environments for ER creation and auditing, as well as model evaluation. The repository provides the configuration for all of them.

* **Claude Code** - Opus, Fable

* **Grok Build** - Grok

* **OpenAI Codex** - GPT

* **Muse Code** - Muse Wonder, Muse Wonder Full

* **Google Antigravity** - Gemini

* **OpenCode** - any other model that can use MCP servers


## Installing AI4L

* Check out this project

``` bash
git clone https://github.com/forever-healthy/AI4L
```

* Or download and expand [AI4L-main.zip](https://github.com/forever-healthy/AI4L/archive/refs/heads/main.zip) if you don't want to use git.


> [!TIP]
> On macOS, use `CMD-SHIFT-.` to show hidden files and access `.claude`, `.opencode`, `.agents`, or `.mcp.json`


## Project Structure

* `prompts/`   - prompt files used by the agents and skills, including the ER prompt (ER.md)
* `creation/`  - created ERs & QA audits, tmp files & trash
* `docs/`      - documentation files, including this one


## Configuration Files

* `AGENTS.md`    - project global instructions for all environments (read directly by Codex, OpenCode, Grok Build, Muse Code & Antigravity)
* `CLAUDE.md`    - one-line `@AGENTS.md` import for Claude Code

* `.claude/`     - related to Claude Code, including agents, skills, and settings. The skills are the single source for all environments: Grok Build reads them directly, `.opencode/skills` and `.codex/skills` link here, and Antigravity registers them via `.agents/skills.json`
* `.mcp.json`    - configuration for the local MCP servers (used by Claude Code & Muse Code)

* `.grok/`       - related to Grok Build, including settings & /rules/rules.md on how to call agents

* `.codex/`      - related to OpenAI Codex, including settings; agents and skills link to `.opencode/` (skills also used by Muse Code)

* `.opencode/`   - related to OpenCode, including agents, skills, and settings

* `.agents/`     - related to Google Antigravity (agents inside the `ai4l` plugin, the MCP servers, the skill registration `skills.json`, and the permission template) and Muse Code (agents)


## Docker Sandbox for the AI CLI Environments

For security reasons, we only run our CLIs in a dedicated [Docker Sandbox](https://docs.docker.com/ai/sandboxes/)

Docker SBX:

* Protects against prompt injection by nefarious sites or MCP servers
* Protects the local environment from Claude Code/Codex/OpenCode errors
* Provides the containers for the MCP servers, no need to install `Docker Desktop` separately

It installs using brew and needs a one-time login with a Docker account

```bash
brew install docker/tap/sbx
sbx login
```

Claude Code, Codex, and OpenCode can be run in the Docker SBX with the following commands:

```bash
cd .../AI4L
sbx run claude
sbx run codex
sbx run opencode
```

Grok Build, Muse Code, and Antigravity are currently not supported in the Docker SBX, but they can be run in shell mode:

```bash
cd .../AI4L
sbx run shell
curl -fsSL https://x.ai/cli/install.sh | bash
grok
```

```bash
cd .../AI4L
sbx run shell
curl -fsSL https://dev.meta.ai/install.sh | bash
muse --approval-mode never
```

```bash
cd .../AI4L
sbx run shell
curl -fsSL https://antigravity.google/cli/install.sh | bash
agy
```

> [!IMPORTANT]
> **We strongly recommend running the CLIs only within the sandbox!!**


## OpenCode Setup

* AI4L requires OpenCode 2 (`opencode --version`). Install or upgrade with `curl -fsSL https://opencode.ai/v2/install | bash`

* Logins from OpenCode 1 carry over. If `opencode auth list` shows none, run `opencode auth login`

* Web search is set to OpenCode's keyless hosted provider (`websearch.provider: random` in `.opencode/opencode.json`). Without a provider, OpenCode 2 cancels every web search

* A bare `opencode` command starts a shared background service that keeps running after you quit the TUI


## Muse Code Setup

Muse Code reads the agents from `.agents/agents/` and the MCP servers from `.mcp.json`; nothing else needs to be configured. Muse has no project-level permission file, so start an interactive session with

```bash
muse --approval-mode never
```

to turn the tool prompts off while its sandbox stays on (`--yolo` would also disable the sandbox).


## Antigravity Setup


### Permissions

Antigravity reads permissions only from the global `~/.gemini/antigravity-cli/settings.json`; there is no project-level permission file. Without allow rules, it asks for approval every few seconds, so merge the provided template once, from the AI4L folder:

```bash
f=~/.gemini/antigravity-cli/settings.json; [ -f "$f" ] || echo '{}' > "$f"
jq -s '.[0] * {permissions: {allow: (((.[0].permissions.allow // []) + .[1].permissions.allow) | unique)}}' \
  "$f" .agents/antigravity-permissions.json > "$f.new" && mv "$f.new" "$f"
```

Then restart `agy`.

* In the sandbox, run the command inside `sbx run shell` — the settings live in the sandbox, not on your machine — and again after `sbx rm`

* The template allows all tools of the AI4L MCP servers and all shell commands (`command(*)`) — the same trust level as the Claude Code settings. Per-command rules do not work in practice: Antigravity requires an exact match for any command line containing `$(…)` or a heredoc, which the agents use constantly

* This is one more reason to run Antigravity only in the sandbox


### MCP Server Names

Antigravity prefixes the MCP servers with the plugin name: `d-pubmed` becomes `ai4l_d-pubmed`. The prompts use the plain names, which the models map without problems.


## MCP Servers

ER.md names six local MCP servers with tools that significantly improve the quality of review creation and audit; five are configured here, and `d-proxy-2` is an unconfigured placeholder (see below).

The MCPs are configured in:

* Claude Code & Muse Code: [.mcp.json](../.mcp.json)
* Grok Build: [.grok/config.toml](../.grok/config.toml)
* Codex: [.codex/config.toml](../.codex/config.toml)
* OpenCode: [.opencode/opencode.json](../.opencode/opencode.json) 
* Antigravity: [.agents/plugins/ai4l/mcp_config.json](../.agents/plugins/ai4l/mcp_config.json)

They are automatically loaded at startup and then run in isolated Docker containers to protect against malicious code.

There is no need to install Docker Desktop separately, as the Docker SBX will handle the containers for the MCP servers.


### Local Browser (`d-browser`)

[playwright/mcp](https://github.com/microsoft/playwright-mcp)

* Runs a headless Playwright browser locally for URL verification
* Handles JavaScript-rendered pages and sites with bot detection that block simple fetches
* Used by the auditor via `browser_navigate` + `browser_snapshot` to verify URL targets match cited content


### Local URL Fetch (`d-fetch`, fallback)

[mcp-server-fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)

* Lightweight HTTP fetcher used as a fallback when the browser MCP is blocked or unavailable
* Set to ignore robots.txt and use a Chrome user agent
* Faster than the browser MCP for sites without bot detection or JS-rendered content


### Stealth Fallback Tiers (`d-proxy-1`, `d-proxy-2`)

* Third- and fourth-tier fallbacks used when both the local browser and local fetch MCPs are blocked by bot detection
* The prompt names only the tier, never the provider or a specific tool, so a provider can be swapped here without editing the prompt — which would otherwise bump the prompt version
* `d-proxy-1` currently runs [@brightdata/mcp](https://github.com/brightdata/brightdata-mcp), whose free Web Unlocker tier retrieves pages that would otherwise return a bot wall or CAPTCHA. Requires a `BRIGHTDATA_API_TOKEN` — get a free account and API token at [brightdata.com](https://brightdata.com) and save it in `.env`
* `d-proxy-2` is unconfigured by default. Add a second provider when one tier refuses a whole class of sites by policy rather than by bot detection; a different failure surface is the point
* Optional: if a tier is unconfigured or its token is missing, the auditor skips it, and a URL that no remaining tier could load is marked unverified with a comment saying what happened


### Clinical Trials MCP Server

[cyanheads/clinicaltrialsgov-mcp-server](https://ghcr.io/cyanheads/clinicaltrialsgov-mcp-server:latest)

* Local requests to clinicaltrials.gov are less likely to be blocked


### PubMed MCP Server

[cyanheads/pubmed-mcp-server](https://ghcr.io/cyanheads/pubmed-mcp-server:latest)

* Local requests to PubMed are less likely to be blocked
* To increase rate limits: Get a [free account with NCBI](https://account.ncbi.nlm.nih.gov/signup/) > get a free API key
* Save the `NCBI_API_KEY` & `NCBI_ADMIN_EMAIL` in `.env` to use the API key for increased rate limits


## Disabling the built-in Anthropic MCP Servers

Claude Code includes built-in MCP servers for PubMed & Clinical Trials. However, they need credentials and make requests from Anthropic's Data Center, which are often blocked by target websites. We recommend disabling them and using the local versions instead.

```
/mcp > `claude.ai Clinical Trials` & `claude.ai PubMed` > `disabled`
```


## First Startup

First startup might take a while:

* On first startup, sbx needs to build the container for the CLI and the containers for individual MCP servers.
* Some MCPs might be displayed as failed on the first run
* You can check the status of MCP servers with `/mcp`
* Usually, containers are fully instantiated after a short while
* On next startup, all MCP containers are cached and will launch and connect instantly


## Current Model / CLI of Choice

We are currently using `Opus` / `Claude Code` for all creation/audit/fixing agents and `Sonnet` for workflow processing, particularly for [evipedia.ai](https://evipedia.ai).

`Sonnet` is more cost-effective and faster for processing tasks that don't require the advanced capabilities of `Opus`.


## Your first ER Creation & Audit

You can use this simple prompt to create your first ER and iteratively audit it with the /er skill:

Claude Code / Grok Build / Muse Code / OpenCode / Antigravity

```bash
/er full Tadalafil
```

Codex

```bash
$er full Tadalafil
```

It will create an [ER for Tadalafil (Cialis)](https://evipedia.ai/tadalafil_er), then audit it, then fix the findings, then audit it again, and so on until it gets a 100% pass rate or reaches the maximum number of audits defined.

Real-world protocols sometimes combine interventions — PDE5 inhibitors with statins to treat cancer, a statin with ezetimibe to lower LDL. AI4L can review such a combination as one intervention:

```bash
/er full Combining PDE5 Inhibitors & Statins to Treat Cancer
```


## Using the /er Skill ($er for Codex)

We have included our /er skill, which allows for easy creation and auditing of evidence reviews. The skill is automatically loaded by the CLIs on startup.

We have implemented the following /er commands in the skill:

* **/er create** \<intervention> \[ for\|to\|as\|\:  \<goal> ] - Create evidence review for \<intervention> to achieve \<goal>. The default goal is "for Health & Longevity". Results are saved in [creation_dir] with filenames constructed as defined in ER.md

* **/er audit** \[ \<filename> | \<intervention> ] - Audit a specific ER, the latest ER for \<intervention>, or the latest ER if nothing is specified. Can also be used to audit ERs generated with other environments/models and saved in [creation_dir].

* **/er fix** \[ \<filename> | \<intervention> ] - Audit a specific ER, the latest ER for \<intervention>, or the latest ER if nothing is specified, and fix the findings.

* **/er iterate** \[ \<filename> | \<intervention> ] - Loop (audit → fix) on a specific ER, the latest ER for \<intervention>, or the latest ER if nothing is specified, until an audit shows 100% pass rate (up to [max_audits])

* **/er combine** \[ \<filename> | \<intervention> ] - Merge all timestamped audits of an ER into one _ER_QA.md file (newest audit first), and move the originals to the trash. Runs automatically at the end of /er iterate

* **/er full** \<intervention> \[ for\|to\|as\|\:  \<goal> ] -  Create ER for \<intervention> → Loop (audit → fix)

* **/er compare** \[ \<intervention> ] - Compare all ERs for \<intervention>, or use the latest \<intervention> worked on, if none specified. Allows comparison of ERs across different models for the same intervention, taking their latest audits into account.


## Helpful Tools

* [**ZED**](https://zed.dev/) as a lightweight editor
* [**MacDown**](https://macdown.uranusjr.com) for visually editing .md files
* [**Markdown Reader Chrome Extension**](https://chromewebstore.google.com/detail/medapdbncneneejhbgcjceippjlfkmkg) as an easy-to-use markdown viewer


## Lessons Learned

Check out our [Lessons Learned](Lessons-Learned.md) document for insights and best practices.

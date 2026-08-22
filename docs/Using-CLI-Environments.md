![Version 1.3.15](https://img.shields.io/badge/Version-1.3.15-green.svg)
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


## Claude Code, OpenCode, Codex, Grok Build

We tested four environments for ER creation and auditing, as well as model evaluation. The repository provides the configuration for all of them.

* **Claude Code** - Opus 

* **OpenAI Codex** - GPT

* **OpenCode** - Gemini (or any other model that can use MCP servers)

* **Grok Build** - Grok


## Installing AI4L

* Check out this project

``` bash
git clone https://github.com/forever-healthy/AI4L
```

* Or download and expand [AI4L-main.zip](https://github.com/forever-healthy/AI4L/archive/refs/heads/main.zip) if you don't want to use git.


> [!TIP]
> On macOS, use `CMD-SHIFT-.` to show hidden files and access `.claude`, `.opencode`, or `.mcp.json`


## Project Structure

* `prompts/`   - prompt files used by the agents and skills, including the main AI4L.md prompt
* `creation/`  - created ERs & QA audits, tmp files & trash
* `docs/`      - documentation files, including this one
* `examples/`  - example ERs and audits created with the CLI environments


## Configuration Files

* `CLAUDE.md`    - project global instructions, also read by OpenCode, Codex & Grok Build

* `.claude/`     - related to Claude Code, including agents & skills (also used by Grok Build), and settings
* `.mcp.json`    - configuration for the local MCP servers (used by Claude Code)

* `.opencode/`   - related to OpenCode, including agents, skills, and settings

* `.codex/`      - related to OpenAI Codex, including agents, skills, and settings
* `AGENTS.md`    - project global instructions for Codex

* `.grok/`       - related to Grok Build, including settings & /rules/rules.md on how to call agents


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
sbx run opencode
sbx run codex
```

Grok Build is currently not supported in the Docker SBX, but it can be run in shell mode:

```bash
cd .../AI4L
sbx run shell
curl -fsSL https://x.ai/cli/install.sh | bash
grok
```

> [!IMPORTANT]
> **We strongly recommend running the CLIs only within the sandbox!!**


## MCP Servers

We are using five local MCP servers with tools that significantly improve the quality of review creation and audit.

The MCPs are configured in:

* Claude Code: [.mcp.json](../.mcp.json)
* OpenCode: [.opencode/opencode.json](../.opencode/opencode.json) 
* Codex: [.codex/config.toml](../.codex/config.toml)
* Grok Build: [.grok/config.toml](../.grok/config.toml)

They are automatically loaded at startup and then run in isolated Docker containers to protect against malicious code.

There is no need to install Docker Desktop separately, as the Docker SBX will handle the containers for the MCP servers.


### Local Browser

[playwright/mcp](https://github.com/microsoft/playwright-mcp)

* Runs a headless Playwright browser locally for URL verification
* Handles JavaScript-rendered pages and sites with bot detection that block simple fetches
* Used by the auditor via `browser_navigate` + `browser_snapshot` to verify URL targets match cited content


### Local URL Fetch (fallback)

[mcp-server-fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)

* Lightweight HTTP fetcher used as a fallback when the browser MCP is blocked or unavailable
* Set to ignore robots.txt and use a Chrome user agent
* Faster than the browser MCP for sites without bot detection or JS-rendered content


### Stealth Fallback Tiers (`d-proxy-1`, `d-proxy-2`)

* Third- and fourth-tier fallbacks used when both the local browser and local fetch MCPs are blocked by bot detection
* The prompt names only the tier, never the provider or a specific tool, so a provider can be swapped here without editing the prompt — which would otherwise bump the prompt version
* `d-proxy-1` currently runs [@brightdata/mcp](https://github.com/brightdata/brightdata-mcp), whose free Web Unlocker tier retrieves pages that would otherwise return a bot wall or CAPTCHA. Requires a `BRIGHTDATA_API_TOKEN` — get a free account and API token at [brightdata.com](https://brightdata.com) and save it in `.env`
* `d-proxy-2` is unconfigured by default. Add a second provider when one tier refuses a whole class of sites by policy rather than by bot detection; a different failure surface is the point
* Optional: if a tier is unconfigured or its token is missing, the auditor still tries it, fails, and honestly marks the URL as unverified rather than silently skipping it


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
/mcp > `claude.ai Clinical Trials` & `claude.ai Clinical PubMed` > `disabled`
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

Claude Code / OpenCode / Grok Build

```bash
/er full Tadalafil
```

Codex

```bash
$er full Tadalafil
```

It will create an [ER for Tadalafil (Cialis)](../examples/tadalafil_2026-0421-2331_Opus_ER.md), then audit it, then fix the findings, then audit it again, and so on until it gets a 100% pass rate or reaches the maximum number of audits defined.


## Using the /er Skill ($er for Codex)

We have included our /er skill, which allows for easy creation and auditing of evidence reviews. The skill is automatically loaded by the CLIs on startup.

We have implemented the following /er commands (not case sensitive) in the skill:

* **/er create** \[ \<intervention> \[ for\|to\|as\|\:  \<goal> ] ] - Create evidence review for \<intervention> to achieve \<goal>. The default goal is "for Health & Longevity". Results are saved in [creation_dir] with filenames constructed as defined in AI4L.md

* **/er audit** \[ \<filename> | \<intervention> | all ] - Review a specific ER, all unaudited ERs, or the latest ER if nothing is specified. Can also be used to create audits for all ERs generated with other environments/models and saved in [creation_dir].

* **/er fix** \[ \<filename>  | \<intervention> ] - Review a specific ER, or the latest ER if nothing is specified, and fix the findings.

* **/er iterate** \[ \<filename> | \<intervention> ] - Loop (audit → fix) until an audit shows 100% pass rate (up to [max_audits])

* **/er full** \[ \<intervention> ] -  Create ER for \<intervention> → Loop (audit → fix)

* **/er compare** \[ \<intervention> ] - Compare all ERs for \<intervention>, or use the latest \<intervention> worked on, if none specified. Allows comparison of ERs across different models for the same intervention. Very helpful when combined with the "/er audit all" command to gain a comprehensive understanding of the models' relative performance.


## Helpful Tools

* [**ZED**](https://zed.dev/) as a lightweight editor
* [**MacDown**](https://macdown.uranusjr.com) for visually editing .md files
* [**Markdown Reader Chrome Extension**](https://chromewebstore.google.com/detail/medapdbncneneejhbgcjceippjlfkmkg) as an easy-to-use markdown viewer


## Lessons Learned

Check out our [Lessons Learned](Lessons-Learned.md) document for insights and best practices.

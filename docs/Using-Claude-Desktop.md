![Version 1.3.6](https://img.shields.io/badge/Version-1.3.6-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Using Claude Desktop

[**Claude Desktop**](https://code.claude.com/docs/en/desktop-quickstart) is an easy-to-use, local environment for non-coding applications.

Compared to a pure Chat-UI, Claude Desktop offers:

* Automatic loading of session instructions from CLAUDE.md
* Direct saving of files to the local filesystem
* Uncomplicated auditing of reviews in a new, clean session, without the risk of session bias
* Interactive conversations based on PERSONA.md

We strongly recommend using **Claude Opus**

## Setup

* Download and expand [AI4L-main.zip](https://github.com/forever-healthy/AI4L/archive/refs/heads/main.zip). It contains the full project, including AI4L.md, PERSONA.md & CLAUDE.md with instructions for the workspace

* Activate connectors for Clinical Trials, PubMed, and bioRxiv. Use: `Customize > Connectors`


## Working with AI4L

* Create a new session
* Set the session folder to `AI4L-main`

Use the same prompts as for the [basic Chat UI](Using-Chat-UI.md)

Results will be filed in `/creation/`


## Helpful Tools

The prompt will generate markdown files for the evidence review and the audit, which can be easily edited and viewed with various tools:

* [**Markdown Reader Chrome Extension**](https://chromewebstore.google.com/detail/medapdbncneneejhbgcjceippjlfkmkg) as an easy-to-use markdown viewer
* [**MacDown**](https://macdown.uranusjr.com) for visually editing .md files


### Audit Quality and Session Bias

> [!IMPORTANT]
> An AI **might mistakenly assume truthfulness** of prior conversations or results of prior actions in a session, even though they might have been hallucinations (**session bias**). Therefore, it is recommended to always perform audits in a **new, clean session** without any previous conversation history.

> [!IMPORTANT]
> Due to the heuristic nature of AI models, there is a good chance that an **AI misses issues during an audit**. To mitigate this risk, we recommend **conducting multiple audits** until at least one clean audit shows a 100% pass rate. To avoid session bias, each audit should be conducted in a clean new session.


### Interactive Conversations

For interactive conversations about generated ERs, prompts, or health & longevity in general, we have distilled the core principles of creating ERs into a "PERSONA.md" file that is automatically loaded by "CLAUDE.md".

It defines role, audience, tone, focus, acronym expansion, and the handling of scientific evidence, equivalent to what an evidence review does.


### Optional: Install Playwright MCP for URL validation

* [playwright/mcp](https://github.com/microsoft/playwright-mcp)

Runs a headless browser locally for URL verification. Useful for URL validation, since provider-based, server-side fetching is often blocked by websites, and many sites require JavaScript rendering.

MCPs are installed by modifying `claude_desktop_config.json`.

Use: `Settings > Developer > Edit Config`

```
  "mcpServers": {
    "Playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    }
  }
```

> [!IMPORTANT]
> Make sure to allow **network egress** and add all domains access for the MCP to work properly:
**Settings** > Capabilities > Code Execution > Allow Network egress: on & Domain Allowlist: All Domains

> [!IMPORTANT]
> Make sure to set **tool permission** to always allow: **Settings** > Connectors > Playwright > Configure > Tool permissions > Always allow


### Optional: Install Fetch MCP as a fallback

* [Fetch URL](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)

A lightweight HTTP fetcher used as a fallback when Playwright is blocked or unavailable. Faster for simple URLs without bot detection.

```
  "mcpServers": {
    "Fetch URL": {
      "command": "uvx",
      "args": ["mcp-server-fetch", "--ignore-robots-txt", "--user-agent=Chrome/145.0.7632.160"]
    }
  }
```

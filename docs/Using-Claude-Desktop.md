![Version 2.0.2](https://img.shields.io/badge/Version-2.0.2-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Using Claude Desktop

[**Claude Desktop**](https://code.claude.com/docs/en/desktop-quickstart) is an easy-to-use, local environment for non-coding applications.

Compared to a pure Chat-UI, Claude Desktop offers:

* Automatic loading of session instructions from AGENTS.md (via CLAUDE.md)
* Direct saving of files to the local filesystem
* Uncomplicated auditing of reviews in a new, clean session, without the risk of session bias
* Interactive conversations based on PERSONA.md

We strongly recommend using **Claude Opus**

## Setup

* Download and expand [AI4L-main.zip](https://github.com/forever-healthy/AI4L/archive/refs/heads/main.zip). It contains the full project, including ER.md, PERSONA.md & AGENTS.md with instructions for the workspace

* Leave the built-in Clinical Trials and PubMed connectors off: they need credentials and make requests from Anthropic's data center, which target websites often block. The local MCP servers below replace them — see [Install the MCP Servers](#install-the-mcp-servers)


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

For interactive conversations about generated ERs, prompts, or health & longevity in general, use PERSONA.md as the system prompt. Claude Desktop loads it automatically via AGENTS.md.

It tells the model to behave, in conversation, the same way evidence reviews are generated from ER.md: same audience, tone, evidence standards, and analytical habits — without the document structure or audit checklist of an evidence review.


### Install the MCP Servers

ER.md names six local MCP servers for literature lookup and URL verification and records them under these exact names in the `Verified By` column of an audit. The two optional proxy tiers (`d-proxy-1`, `d-proxy-2`) are skipped when not configured, so Claude Desktop needs only these four:

* `d-browser` — [playwright/mcp](https://github.com/microsoft/playwright-mcp): runs a headless browser locally for URL verification. Provider-based, server-side fetching is often blocked by websites, and many sites require JavaScript rendering
* `d-fetch` — [mcp-server-fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch): a lightweight HTTP fetcher used as a fallback when the browser is blocked or unavailable. Faster for simple URLs without bot detection
* `d-pubmed` — [pubmed-mcp-server](https://github.com/cyanheads/pubmed-mcp-server): searches PubMed and fetches article metadata and full text via NCBI's E-utilities
* `d-clinicaltrialsgov` — [clinicaltrialsgov-mcp-server](https://github.com/cyanheads/clinicaltrialsgov-mcp-server): looks up trial records and results on ClinicalTrials.gov

They run with `npx` (Node 24 or later) and `uvx`, no Docker needed. MCPs are installed by modifying `claude_desktop_config.json`.

Use: `Settings > Developer > Edit Config`

```
  "mcpServers": {
    "d-browser": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headless"]
    },
    "d-fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch", "--ignore-robots-txt", "--user-agent=Chrome/145.0.7632.160"]
    },
    "d-pubmed": {
      "command": "npx",
      "args": ["-y", "@cyanheads/pubmed-mcp-server"],
      "env": { "MCP_TRANSPORT_TYPE": "stdio", "NCBI_API_KEY": "", "NCBI_ADMIN_EMAIL": "" }
    },
    "d-clinicaltrialsgov": {
      "command": "npx",
      "args": ["-y", "clinicaltrialsgov-mcp-server"],
      "env": { "MCP_TRANSPORT_TYPE": "stdio" }
    }
  }
```

* To increase PubMed rate limits: get a [free account with NCBI](https://account.ncbi.nlm.nih.gov/signup/) > get a free API key, and enter `NCBI_API_KEY` & `NCBI_ADMIN_EMAIL` above. Both may stay empty

> [!IMPORTANT]
> Make sure to allow **network egress** and add all domains access for the MCPs to work properly:
**Settings** > Capabilities > Code Execution > Allow Network egress: on & Domain Allowlist: All Domains

> [!IMPORTANT]
> Make sure to set the **tool permission** of each of the four servers to always allow: **Settings** > Connectors > d-browser (and the other three) > Configure > Tool permissions > Always allow

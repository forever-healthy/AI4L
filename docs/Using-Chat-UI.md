![Version 1.3.20](https://img.shields.io/badge/Version-1.3.20-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# Using a Web-Based Chat UI


## Setup

Download [AI4L.md](../prompts/AI4L.md), drop it in your chat box, and use the prompts below.

We strongly recommend using **Claude Opus**


## Prompts for Creating Evidence Reviews

Schema: "Create an ER for \<intervention> [for|as|to \<goal>] that can pass an audit as described in AI4L.md"

``` text
Create an ER for Tadalafil that can pass an audit as described in AI4L.md
```

``` text
Create an ER for Low-Dose Naltrexone to Improve Immune Function
that can pass an audit as described in AI4L.md
```

``` text
Create an ER for Low-Level Light Therapy for Skin Rejuvenation
that can pass an audit as described in AI4L.md
```


## Prompts for Quality Assurance Audit and Self-Refinement

``` text
Perform an audit of the previously generated ER as described in AI4L.md
```

``` text
Use the results of the audit to improve the ER
```

``` text
Redo the audit and explain any remaining issues
```


## Helpful Tools

The prompt will generate markdown files for the evidence review and the audit, which can be easily edited and viewed with various tools:

* [**Markdown Reader Chrome Extension**](https://chromewebstore.google.com/detail/medapdbncneneejhbgcjceippjlfkmkg) as an easy-to-use markdown viewer
* [**MacDown**](https://macdown.uranusjr.com) for visually editing .md files


## Audit Quality and Session Bias

> [!IMPORTANT]
> An AI **might mistakenly assume truthfulness** of prior conversations or results of prior actions in a session, even though they might have been hallucinations (**session bias**). Therefore, it is recommended to always perform audits in a **new, clean session** without any previous conversation history.

> [!IMPORTANT]
> Due to the heuristic nature of AI models, there is a good chance that an **AI misses issues during an audit**. To mitigate this risk, we recommend **conducting multiple audits** until at least one clean audit shows a 100% pass rate. To avoid session bias, each audit should be conducted in a clean new session.


## Interactive Conversations

For interactive conversations about generated ERs, prompts, or health & longevity in general, load [PERSONA.md](../prompts/PERSONA.md) as the system prompt.

It tells the model to behave, in conversation, the same way evidence reviews are generated from AI4L.md: same audience, tone, evidence standards, and analytical habits — without the document structure or audit checklist of an evidence review.

![Version 1.3.7](https://img.shields.io/badge/Version-1.3.7-green.svg)
![License MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

![AI4L – AI for Practical Longevity](docs/AI4L-Header.png)

**AI4L** - Enabling anyone to use AI to generate high-quality, evidence-based reviews of health & longevity interventions

[Getting Started](#getting-started) - [Examples](examples/) - [Feedback & Discussions](https://github.com/forever-healthy/AI4L/discussions) - [Contributing](CONTRIBUTING.md)

_AI4L's purpose is to support informed discussion and not to provide medical advice, diagnosis, or treatment. See [Limitations](docs/Limitations.md)_

# A Wonderful Opportunity

Senolytics, NAD+ restoration, lipid replacement, decalcification, mTOR modulation, geroprotectors, peptides, … – the first generation of human rejuvenation therapies is available today. However, the field is still young, and information is often spotty. New therapies are emerging, and existing ones are updated or replaced.

Additionally, there is already a vast body of cutting-edge medical knowledge available now to maximize our health and well-being. Unfortunately, most of that knowledge is scattered across various experts, specialized communities, blogs, books, and websites, or buried deep in scientific research, making it quite hard to make informed decisions about one's personal health and longevity.

Ultimately, to optimize our health and longevity, we need to make very personal decisions about which interventions to apply and when. Arming ourselves with the best knowledge about therapeutic options is vital.


## The Need for a Trusted 2nd Opinion

When considering an intervention, naturally, questions arise such as:

* What does the science say about the intervention?
* What do the experts say?
* What are the potential benefits? At what magnitude?
* What potential risks are involved?
* Are there risk mitigation strategies?
* What does a good therapeutic protocol look like?
* How do we monitor success?

In the past, we have answered these questions by creating reviews on health and longevity-related interventions with a dedicated team of researchers (see: [Legacy Publications](https://forever-healthy.atlassian.net/wiki/spaces/ENG/pages/1171128358/Legacy+Publications)).

However, each review took us more than two months with a team of two. This neither scaled to all potential interventions we would love to review, nor to keeping past reviews up to date at all times. We knew what we wanted, identified and mastered the challenges. But it turned out impractical.


## AI to the Rescue

We are now entering the age of modern AIs that are trained on vast corpora of scientific literature, expert commentary, and multimedia content.

That could solve the scalability problem, but unfortunately, conventional AI-based reviews often sound equally confident whether they're right or wrong. Due to the heuristic nature of AI, models often hallucinate studies and URLs, misrepresent evidence, miss critical nuances, and restructure results on every request. A new approach is needed.


## AI4L Design Goals

To maximize trustworthiness, quality, and utility of the generated reviews, we have focused on the following goals:

**Trusted Knowledge** - To build trust, the AI needs to base its answers only on evidence from scientific sources, peer-reviewed journals, clinical trials, and reputable experts' opinions.

**Reproducible Structure** - Reviews should always follow the same format, making it easier to learn from them, judge their quality, and compare reviews from different models.

**Measurable Quality** - There should be a way to objectively evaluate the quality of a review. Depth, breadth, source quality, analytic quality, completeness, and the elimination of hallucinations are important factors.

**Self-Auditing** - The system should be able to meticulously audit the quality of its reviews — without requiring human review as a prerequisite for improvement.

**Self-Refinement** - The AI should be able to correct its mistakes by using the result of a self-audit.

**Simplicity** - Ideally, our tool would be a single prompt that anyone could easily download and use. It should work with all major models so we can compare reviews, easily identify, and use the best AI at any time.


## A QA Centered Approach

Given the above goals, reviews need to follow a lifecycle that allows for iterative improvement:

Creation > Audit > Correction > Audit > ...

Here we face two main challenges:

* Due to the heuristic nature of AI models, answers to a given prompt will be delivered in a more or less different structure and with varying content for every request.

* Additionally, there is the issue of hallucinations, AI inventing answers, particularly when the honest answer would just be "I don't know".

If we were to go the conventional way and present an AI with a prompt simply asking it to create a review on a given topic, it would produce a fuzzy result with varying structure, depth, source selection, evidence quality, etc. The review would potentially also include hallucinations. If we were to ask it to perform a QA audit on that review, it would generate a QA process on the fly, with the same fuzziness and potential for hallucinations as when creating the review itself. The loss of quality and repeatability would be potentiated.

To avoid this, we logically move our prompt to the very end of our product lifecycle.

## Audit-Driven Prompting

The prompt "only" describes a very thorough, 400+ item QA audit process for an evidence review for a given topic, including all the hints and instructions one would also give to a human QA auditor. It does not, in any way, directly instruct an AI how to create a review.

We use this "QA" prompt and task the AI with generating a review on a given topic that can pass an audit as described in the prompt. Leading frontier models understand this indirection and will try to generate a review that can pass the QA audit.

We now use the same prompt to audit the review by asking the AI to perform a QA audit, as described in the prompt. Afterward, with full context knowledge of the audit, the AI is asked to correct the review based on the audit findings.

**Optimizing Review and Audit Quality**

We apply strict role separation — the creator and each loop's auditor are independent agents with enforced isolation, and a clean, history-free context. Thus, we can avoid context bias and context-based hallucinations.

Audit quality is improved by a multi-step audit process that minimizes auditor hallucinations and requires auditors to perform rigorous external verification, including actively fetching URLs, retrieving metadata, and verifying citations against live sources.

Combined with a zero-tolerance pass/fail approach, the process is repeated until we reach 100% pass across all QA criteria. In testing, reviews typically reach 100% pass only after multiple audit-fix cycles (see: [examples](examples/)).

We refer to this approach as "Audit-Driven Prompting".




## Getting Started

There are two general modes of using AI4L, each with its own advantages:

**Basic Mode**: best for quick exploration and testing

* [Using a Web-Based Chat UI](docs/Using-Chat-UI.md)
* [Using Claude Desktop](docs/Using-Claude-Desktop.md)

**Workflow Mode**: best for repeatability, automated workflows, and higher audit quality

* [Using CLI Environments](docs/Using-CLI-Environments.md)


## What to expect from AI4L

Here are some [samples of evidence reviews and audits](examples/) created with AI4L.

> [!IMPORTANT]
> We found tremendous variability in the quality of reviews, audits, and fixes across models. [Discussion of individual model performance](https://github.com/forever-healthy/AI4L/discussions/categories/models).


## Further Reading

* [Limitations of AI4L](docs/Limitations.md) - What AI4L cannot do, and what to be cautious about
* [Lessons Learned](docs/Lessons-Learned.md)- Our general findings working with models
* [Contributing to AI4L](CONTRIBUTING.md) - How to improve the AI4L prompt

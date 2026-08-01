![Version 1.3.0](https://img.shields.io/badge/Version-1.3.0-green.svg)
[![Forever Healthy](https://img.shields.io/badge/(c)_2026-Forever_Healthy-573D7D.svg)](https://forever-healthy.org)

# AI4L Quality Assurance Guideline for Evidence Reviews

Full lines encapsulated in `   ` are treated as comments for the auditor and are not executed or included in any resulting document. They are hints and instructions for the auditor only.

`The entire Introduction, Globals & Audit Instructions block is not part of the final audit document.`

## Introduction

Evidence reviews are well-structured, high-quality, evidence-based reviews of health and longevity interventions

This document defines the quality assurance (QA) audit process for an "Evidence Review" (ER). It contains both instructions for an auditor and a structured list of the items to be audited.

It not only allows an auditor to evaluate the quality of an ER but also guides authors in creating ERs, since the QA implicitly defines the requirements for such documents.


## Globals

* Set [total_items] to 404 (which is the number of all checklist items)

* Set [review_filename] to the filename of the review to be audited
* Set [review_canonical_topic] to the canonical_topic as stated in the frontmatter of the review to be audited

* Set [prompt_version] to the version stated at the start of this document

* Set [audit_date] to the date of the audit with a format of "YYYY-MMDD-HHMM" (e.g., 2026-0501-1430)
* Set [audit_ai_fullname] to the full name of the model that is running in this session, including its version, but no additional qualifiers. (e.g., Opus 4.7, Sonnet 4, Grok 4.1, Gemini 3, ChatGPT 5.5)
* Set [audit_ai_nickname] to the name of the AI model reduced to a single word without version, etc. (e.g., Opus, Sonnet, Grok, Gemini, GPT)

* Set [audit_filename] by performing a literal string substitution on [review_filename]. Replace "_ER.md" with "_ER_QA_[audit_date].md".
  - E.g., `Intervention_2026-0303-1455_MODEL_ER.md` becomes `Intervention_2026-0303-1455_MODEL_ER_QA_2026-0303-1455.md`
  - DO NOT forget the ".md" extension
  - DO NOT change anything else in the filename except the specified substitution


## Auditor Instructions

The following rules govern how this checklist must be used when performing an audit:


### Start Fresh

* DO NOT use or take into account any prior audit's results in any way in relation to this audit. Every audit must be performed independently. The prior audit may have missed issues or made mistakes. Each iteration must be a fully independent, item-by-item audit.

* DO NOT use or access any other documents besides this one and the ER in question

* DO NOT take any shortcuts to optimize token usage or cut processing time. The goal is thoroughness, not token optimization.

### Handling limitations

* Read all files that you have to access in chunks of 250 lines or fewer, starting from the top. Do not read a file in one go if it exceeds 250 lines.

### No Sub-agents

An auditing AI should not use sub-agents for tasks in the audit process (e.g., the 4-step process). Often, not all knowledge is passed to or available to sub-agents; they can lose context and become confused.

* DO NOT use sub-agents for any tasks in the audit process.
* DO everything yourself.
* DO things sequentially.

### Temporary files

* All temporary files created during the audit process must be stored in [tmp_dir]

### Do not fix any issues during the audit

* DO NOT fix any issues found during the audit.
* Auditing and fixing are separate operations.
* Your mode is audit-only.

### Use Unicode for emojis

When inserting emojis, always use the corresponding Unicode, not the short code
E.g., 🟢 (Unicode) instead of ":green_circle:", 🔴 (Unicode) instead of ":red_circle:"


### Formatting of checklist items and sections

In the document resulting from this audit, the individual sections of the checklist should be structured as tables with the following format.

| # | Description | Result | Comments |
|---|-------------|:------:|----------|

Each section gets its own table. The table and the column titles should be exactly as given above.


### Processing the checklist

Process the checklist section by section, in order.

For each section:

1. Identify every item (looking like "* x.y \<description>") in the section. Count them.
2. Create one table row for each item. Do not skip any, even short conditional items like "If none of the above..." or "If the section is not applicable...".
3. After completing the table for that section, verify that the number of rows in your table must equal the number of items you counted in step 1. If it does not, find and add the missing item(s) before moving to the next section.

Every item in a checklist section must produce exactly one row. No more, no less.

Do not add, invent, or infer checklist items that are not explicitly listed in this document. Only the items written below as "* x.y \<description> " are checklist items. If something is not listed, it is not checked.


#### "#" column

Items are pre-numbered in this document with the format "<section#>.<item#>" (e.g., 2.1, 2.2, ...). Use these exact numbers in the "#" column of the audit table.


#### Description column

Do not abbreviate, shorten, or reformulate any checklist items when creating the audit document. Use them as they are for the description column.


#### Result column

Each result is either "pass", "fail", or "N/A". There is no "partial pass" or "borderline." If an item is not fully satisfied, it is "fail".

Use 🟢 for "pass" and 🔴 for "fail".
DO NOT use text for these two.
For "N/A", use the actual text "N/A".


#### N/A definition

Mark an item N/A only when the checklist item contains an explicit condition that does not apply to the document under audit. Specifically, N/A is permitted only for items phrased as "If \<condition>..." where the condition is factually not met. Examples:

* "If an article exists, a direct link is provided" → N/A when no article exists
* "If no article exists, clearly states not found" → N/A when an article does exist
* "If the section does not apply, briefly mention this" → N/A when the section does apply
* "For supplements, addresses third-party testing" → N/A when the intervention is not a supplement

N/A must never be used for items that the document failed to address, did poorly, or where the auditor is uncertain. Those are fails.


#### Comment column

Every item marked as failed must include a specific comment stating what is wrong and where in the review (line number or section name).


### 5-Step audit process

To avoid hallucinations and calculation errors, break down the audit process into 5 steps:

#### 1. Table generation

* First, only expand the numbered sections and items into the table format
* Copy each item description verbatim. DO NOT abbreviate, rephrase, or summarize items

* DO NOTHING else
* DO NOT audit items
* DO NOT fill in results
* DO NOT calculate the pass rate
* DO NOT generate comments

* Count the total number of items in the tables

* If it differs from [total_items]
  - Report the fact and the reason
  - STOP the audit process

* Save the result as a new file named "[audit_filename]" in [creation_dir]
* If possible, display a download link for the user to save the file; otherwise,
  - Display the full result (with all tables included, not a partial file), so it can be copied


#### 2. URL Verification

* Scan the whole document and list all links of the ER in the `## URL Verification` table
* Ignore internal links and links to the AI4L repository
* Use the standard markdown notation for the `Link` column
* When creating the list copy the link title verbatim, excactly as it appears in the document

* After creating the list, for each entry:

  - For PubMed links, use `pubmed_fetch_articles` first. If the PMID resolves and the returned title matches the ER's claim, mark `Loaded` ✅ and `Verified By` `pubmed_fetch_articles`. Only if the PMID does not resolve, fall back to the chain below.

  - try to retrieve the url using `d-browser`. If `d-browser` fails, try `d-fetch`. If `d-fetch` also fails, try `d-brightdata` (`scrape_as_markdown`), which can retrieve pages protected by bot detection.

  - A retrieval has FAILED unless you are holding the genuine target page. A transport error, an error page (404, 500, …), a paywall, or a bot-detection interstitial (CAPTCHA, "Security Checkpoint", "Verify your browser") are all failures. A page you could not read is not verified.

  - Mark the `Loaded` column ✅ or ❌. In `Verified By` note which tool retrieved it: `d-browser`, `d-fetch`, `d-brightdata`, or — for PubMed links — `pubmed_fetch_articles`.

  - If all three fail, mark `Loaded` ❌, `Verified By` `none`, `Match` 🔴, and comment that the url could not be retrieved. If `d-brightdata` is not configured, say so in the `Comment` column — an unavailable tool is still a failure, NEVER a pass.

  - If the page was retrieved, confirm that the content matches the link's title. If it does, mark the item in the `Match` column as passed with 🟢. If it does not, mark it as failed with 🔴 in the `Match` column and add a comment in the `Comment` column about the mismatch.


#### 3. Audit

* Now do the actual audit
* Carefully examine each section and item, ensuring all requirements are met, and any potential issues are addressed

* DO NOT add any checklist items
* DO NOT add any table rows or columns
* DO NOT add any extra tables
* DO NOT count items or calculate the pass rate

* Save the result by overwriting "[audit_filename]" in [creation_dir]
* If possible, display a download link for the user to save the file; otherwise,
  - Display the full result (with all tables included, not a partial file), so it can be copied


#### 4. Counting

* Store your scripts in [tmp_dir]
* Write and run a script that parses the result of step 3 and counts "Items", 🟢, 🔴, and "N/A".

* Calculate [pass_rate] = [passed] / ([passed] + [failed]) × 100, always formatted to exactly 2 decimal places followed by % (e.g. `100.00%`, `85.71%`)
* N/A items are excluded from both the numerator and the denominator.

* DO NOT attempt to count or do arithmetic manually
* DO NOT add any extra tables
* DO NOT remove, summarize, or replace the individual items or tables; they must all remain in the final file

* Use the Step 3 result
* Fill out the summary table at the beginning of the audit

* If "Items" differs from [total_items]
  - Report the fact and the reason
  - STOP the audit process

* Save the result by overwriting "[audit_filename]";
* If possible, display a download link for the user to save the file; otherwise,
  - Display the full result (with all tables included, not a partial file), so it can be copied


#### 5. Documentation

* Document all the issues you found during the audit in the `## Issues` section at the end of the document.

* If the [pass_rate] is 100.00% {
  * Write in the `## Issues` section `Pass rate 100.00%. No issues found.`
}

* Issue entry format: Use a numbered list
* Each entry starts with the checklist item number and a short label (3-6 words) in bold, followed by a colon, then the description in normal weight.
* Format:  "1. **{item#} — {short label}:** {What is wrong and where, with specific line numbers or quotes. 1-2 sentences max.}"
* If one issue covers multiple checklist items, combine the numbers: "**6.1 / 6.10 — {short label}:**"

* Example: "1. **7.5 — PMC link instead of PubMed:** The Johnston et al. 2006 paper in Recommended Reading uses a PMC link instead of the required PubMed format."
* Example: "2. **6.1 / 6.10 — RCT not expanded at first use:** "RCT" first appears at line 106 without expansion; it is only expanded later at line 183."

* Always end the whole document with two blank lines


### Source-of-truth fields are immutable

The frontmatter of the ER contains a one-way derivation chain rooted in the topic:

[topic] → [intervention] → [canonical_name] → [canonical_topic] / [short_topic] /
[short_topic_lc]

* [intervention] is derived solely from [topic] (rule 6.7) and is the source of truth for the
intervention identity. It MUST NOT be modified to resolve any finding.

* When a finding involves a mismatch between [intervention] and a downstream field — e.g.,
[canonical_name] expanding [intervention] in violation of rule 6.10 — the only valid remedy is to
rewrite the downstream field. Rewriting [intervention] to match a corrupted downstream field
corrupts the input chain and is never a valid fix.


### Hints for the auditor

If necessary, hints for the auditor in the actual checklist are formatted `like this` and must not be part of the resulting document.


### End of process description

The actual audit form starts below this line

-------------------------------------------------------

`Insert appropriate data here.`

---
review_canonical_topic: [review_canonical_topic]
review_filename: [review_filename]
prompt_version: [prompt_version]
audit_date: [audit_date]
audit_ai: [audit_ai_nickname] / [audit_ai_fullname]
audit_filename: [audit_filename]
audit_duration: "[Actual time the audit took in HH:MM format]"
---

# Audit: [review_canonical_topic]

Audit conducted on [audit_date reformatted as %d/%m/%Y %H:%M] using [AI4L](https://github.com/forever-healthy/AI4L) / [audit_ai_fullname]

[Iterations](#iterations)

## Summary

`Fill out the summary table in step 4 of the audit only`

| Items         | Count            |
| ------------- | ---------------- |
| Total         | [total_items]    |
| Passed        | [passed]         |
| Failed        | [failed]         |
| N/A           | [n/a]            |
| **Pass Rate** | **[pass_rate]**  |

* Total = Passed + Failed + N/A
* Pass Rate = Passed / (Passed + Failed) × 100
* N/A items are excluded from the pass rate calculation


## URL Verification

| Link                     | Loaded | Verified By | Match | Comment          |
| ------------------------ | ------ | ----------- | ----- | ---------------- |
| [link_title](<link_url>) |        |             |       |                  |



`List of checklist items grouped in sections starts here.`


## 1. Focus, Tone & Audience

* 1.1 The focus of the document is to present the evidence for and against applying the intervention in question, with the stated goal

* 1.2 The focus of the document is primarily on the effectiveness of the intervention, not cost. Cost is secondary at most

* 1.3 The document does not jump to conclusions prematurely; it builds its case through evidence

* 1.4 The tone of the document is simultaneously expert, accessible, objective, and data-driven, but also empowering and encouraging

* 1.5 The document reads as a trusted, knowledgeable guide rather than a prescriptive doctor

* 1.6 The document avoids language that implies medical or clinical advice

* 1.7 The document "presents information" instead of "providing guidance", "recommending", or "advising"

* 1.8 The document never addresses "the reader" directly — it presents evidence, not guidance

* 1.9 Content is written in plain language, avoiding unnecessary medical jargon

`A term is "unnecessary" when a plain-language word would carry the same precision; where a specialist term is the only precise option it is kept and explained at first use. Entry and summary surfaces hold to a stricter plain-language bar — see (Motivation) and (Conclusion).`

* 1.10 Information is presented in a concise and very compact manner

* 1.11 When presenting multiple aspects, bullet points are used to enhance clarity and readability

* 1.12 The target audience is health- and longevity-oriented adults who are risk-aware, proactive, and actively seeking to optimize health or apply the intervention under review.

* 1.13 The target audience is willing to employ lifestyle and behavioral changes as well as follow protocols that may be inconvenient, costly, or require effort.

* 1.14 The document is NOT written for the general population, who are unwilling to employ lifestyle and behavioral changes or follow protocols that may be inconvenient, costly, or require effort.

* 1.15 Framing, takeaways, and risk/benefit weighting throughout the document reflect this audience, including where an intervention's signal for the average person differs from its signal for this audience.

* 1.16 The document's own voice frames usage in **longevity** terms, not "anti-aging" (e.g., "anti-aging clinics", "anti-aging community", "anti-aging medicine"). Proper names that contain "anti-aging" (e.g., "American Academy of Anti-Aging Medicine") are quoted verbatim.

* 1.17 The document's own voice uses formal clinical and scientific terminology, not colloquial or consumer-grade language (e.g., "oral medication" not "pill(s)"; "injection" not "shot"; "adverse event" not "bad reaction"). Direct quotes from sources are exempt.


## 2. Handling of Scientific Evidence

* 2.1 When available, conclusions are based on high-quality human clinical trials, such as randomized controlled trials (RCTs) and meta-analyses.

* 2.2 Observational studies, mechanistic data, and expert opinion from researchers and reputable physicians with practical experience, utilizing their books, blog posts, podcasts, and presentations, are used to provide context and fill in gaps where clinical data is lacking.

* 2.3 It is stated clearly when high-quality evidence is conflicting or inconclusive (e.g., two large RCTs show different outcomes).

* 2.4 If evidence is conflicting or inconclusive, different viewpoints or study outcomes are presented, and the potential reasons for the discrepancy, if known (e.g., differences in study design, population, dosage, or duration), are explained

* 2.5 When a researcher's work is central to the topic, their primary sources (original publications, books, data) are examined directly — not solely through the lens of later critiques or dismissals

* 2.6 Claims that prior research has been "debunked," "discredited," or "disproven" are treated as assertions that themselves require evidence. The critique is evaluated, not repeated as fact

* 2.7 The document does not adopt any professional establishment's consensus position as the default truth. Mainstream and dissenting positions are both presented as claims supported (or not) by evidence.

* 2.8 When the body of available evidence is produced primarily by parties with a direct financial interest in the intervention's adoption or rejection (e.g., professional associations whose members perform the procedure, pharmaceutical manufacturers, device makers), this conflict of interest is identified and named at first citation and again in the Conclusion.

* 2.9 When competing interventions differ substantially in cost, the ER identifies whether institutional payers (insurers, national health systems) have a systematic financial incentive to favor one over the other, and notes this as a potential source of structural bias in guideline formation and research funding.

* 2.10 When a professional or advocacy organization is cited as a primary source of evidence or guidelines, the ER notes whether that organization's membership derives direct revenue from the conclusions it endorses — immediately adjacent to where the organization's position or advocacy is stated (not merely at its introduction or first mention), and again in the Conclusion. This applies symmetrically to all cited parties on all sides of the debate.

* 2.11 Content is framed for the target audience (see 1.12), not as population-level outcomes.


## 3. General Formatting

* 3.1 The document is output as a Markdown file (.md)

* 3.2 All section headings use H2 formatting

* 3.3 An extra blank line (two spaces + carriage return) appears before each section heading

* 3.4 Every list (bulleted or numbered) is preceded by a blank line

* 3.5 New lines are created using two spaces and a carriage return

* 3.6 Emojis are always encoded using the corresponding Unicode, not the short code. e.g., 🟢 (Unicode) instead of ":green_circle:", 🔴 (Unicode) instead of ":red_circle:"

* 3.7 The document contains no unrendered AI-internal markup (e.g., citation tokens, reference IDs, source tags, or debug metadata)

* 3.8 Scientific nomenclature and technical symbols follow their field conventions everywhere in the document — in frontmatter fields, headings, body text, tables, and captions.

* 3.9 These conventions override "Chicago Manual of Style" (CMS) where they conflict

* 3.10 Binomial species names: genus is capitalized, species epithet is lowercase. Examples: "Lactobacillus acidophilus" not "Lactobacillus Acidophilus"; "Ginkgo biloba"; "Rhodiola rosea"; "Tribulus terrestris"; "Bifidobacterium lactis"; "Curcuma longa". This applies inside longer phrases too ("Ginkgo biloba extract", "Withania somnifera root"). Binomials SHOULD be italicized in body text when context allows (*Lactobacillus acidophilus*); frontmatter and headings stay plain.

* 3.11 Trinomial/subspecies names: genus capitalized; species and subspecies epithets lowercase. Example: "Bifidobacterium animalis subsp. lactis".

* 3.12 Stereochemistry prefixes: `L-`, `D-`, `l-`, `d-`, `cis-`, `trans-`, `R-`, `S-` keep their conventional case and stay hyphenated to the next token. The token following a capitalized prefix is title-cased per CMS ("L-Theanine", "D-Ribose", "cis-9, trans-11 CLA").

* 3.13 Gene and protein symbols: follow field convention, not CMS. Examples: "FOXO4", "mTOR", "p53", "HIF-1α", "NF-κB", "IGF-1". Human gene symbols are all-caps; mouse gene symbols are title-case with the rest lowercase ("Foxo4").

* 3.14 Chemical compound names and IUPAC forms: keep hyphens, numeric locants, and lowercase elements as written ("N-acetylcysteine" in body text, "N-Acetylcysteine" only if it is the canonical_name/short_topic where CMS title-case applies to the word after the prefix).



## 4. Glossary Enforcement

* 4.1 Every medical acronym (e.g., DASH, SGLT2, mTOR, AMPK, BUN, CMP, eGFR) includes a plain-language explanation at first use

* 4.2 Every drug class name (e.g., ARB, ACE inhibitor, SGLT2 inhibitor) includes a plain-language explanation at first use

* 4.3 Every biological pathway name (e.g., RAAS, PPAR-γ, mTOR, AMPK) includes a plain-language explanation at first use

* 4.4 Every statistical term (e.g., CI, HR, RR, OR, NNT) includes a plain-language explanation at first use

* 4.5 Every exercise and nutrition term (e.g., Zone 2, DASH, HIIT) includes a plain-language explanation at first use

* 4.6 Every uncommon medical condition or symptom name (e.g., angioedema, rhabdomyolysis, hyperkalemia, orthostatic hypotension) includes a plain-language explanation at first use

* 4.7 Every enzyme and genetic polymorphism (e.g., CYP2C9, UGT1A3, ABCB1, APOE4, MTHFR, and COMT) includes a very short explanation of what the enzyme or gene does at first use

* 4.8 Clinical trial names and brand/product names are NOT given glossary explanations (they are exempt)

* 4.9 Plain-language explanations are brief and in parentheses

* 4.10 First use is determined by reading order (top to bottom); the explanation appears where the term is first encountered, starting at the motivation section.

* 4.11 Frontmatter, title, and "Also known as:" line are exempt from glossary rules

* 4.12 If an acronym first appears inside a table cell, the expansion is provided in a "Context/Notes" column, not crowding the table cell itself


## 5. Hyperlinks

* 5.1 All links are syntactically valid (proper URL format)
* 5.2 All links use standard Markdown syntax: "[text](URL)"
* 5.3 No URLs appear to have been constructed by guessing or interpolating from memory
* 5.4 All pipe characters "\|" in markdown link text are replaced with an en-dash "–"

* 5.5 PubMed links use the format "https://pubmed.ncbi.nlm.nih.gov/PMID/" — not publisher sites (e.g., not ahajournals.org, sciencedirect.com, wiley.com)
* 5.6 ClinicalTrials.gov links use the format "https://clinicaltrials.gov/study/<NCT ID>"
* 5.7 DOI links use the format "https://doi.org/<DOI>"

* 5.8 No link points directly to a PDF file. A URL whose path ends in ".pdf" (optionally followed by a query string or fragment) or that serves "application/pdf" is a FAIL. Links must resolve to an HTML landing or abstract page (e.g., the PubMed, DOI, or publisher article page), never a raw PDF download.

`URL verification: Each URL must pass two checks:`

* 5.9 Each URL retrieves the genuine target page

`For PubMed links, use "pubmed_fetch_articles" first: a resolving PMID satisfies this item. If it does not resolve, fall through to the chain below.`

`Use "d-browser" to load the URL. If it fails, try "d-fetch". If that also fails, try "d-brightdata" ("scrape_as_markdown"). Record the tool that succeeded in "Verified By". A FAIL is any outcome that is not the genuine target page — a transport error, an error page (404, 403, 500, …), or a bot wall / CAPTCHA / "security checkpoint" interstitial. A page you could not read is not verified. If all three fail — including when "d-brightdata" is not available in this environment — the item is a FAIL.`

* 5.10 The page at each URL contains content matching the link's annotation in the ER (the page is about the cited topic; the article/resource title matches)

`Use "d-browser" or, if it fails, "d-fetch", or, if that also fails, "d-brightdata" to read the page, then confirm the content matches the link's description in the ER (e.g., the page is about the cited topic, the article title matches what the ER claims). A generic landing page, paywall, bot wall, or unrelated content fails this check.`

`For PubMed links, the auditor must instead use "pubmed_fetch_articles" to retrieve the article metadata and verify the returned title matches the title claimed in the ER. A mismatch means the PMID was fabricated or assigned to the wrong paper.`


## 6. Frontmatter - Metadata

* 6.1 The document starts with a frontmatter section containing metadata about the document

`Stating what the author received as input for the review creation`

* 6.2 The initial topic used in the prompt to create the review is stated as 'initial_topic: "[initial_topic]"'

`Topic Expansion`

* 6.3 If the [initial_topic] of the evidence review is just a name of a single intervention or a list of interventions (e.g., "Metformin", "Resveratrol & NMN"), the [topic] is stated as "topic: "[initial_topic] for Health & Longevity""

* 6.4 If the [initial_topic] of the evidence review is of the form "[intervention(s)] : [goal]", the [topic] is stated as "topic: "[intervention(s)] to/for/as [goal]" using a fitting preposition (e.g., "Metformin : Skin Rejuvenation" → "Metformin for Skin Rejuvenation")

* 6.5 Otherwise [topic] is stated as 'topic: "[initial_topic]"'

`Normalization & extraction of intervention and goal`

* 6.6 The [topic] of the evidence review is normalized to the form of "[intervention] to/for/as [goal]"

* 6.7 The [intervention] part of the [topic] is stated as "intervention: [intervention]"

* 6.8 The [goal] part of the [topic] is stripped of any preposition and wordiness and stated as "goal: [goal]", e.g., "to Optimize Immune Function" → "Optimize Immune Function", "for Skin Rejuvenation" → "Skin Rejuvenation", "as a Longevity Intervention" → "Longevity"

`Cleaning the intervention and determining variations.`

* 6.9 The [canonical_name] is stated as "canonical_name: [intervention]"
* 6.10 [canonical_name] must not expand [intervention] with synonyms, INN names, or alternate identifiers — even if known. Expansion belongs in alternate_names
* 6.11 The [canonical_name] is simplified and compressed in a way that "and" is replaced by "&", prepositions and wordiness are removed where possible, but the essence is not altered (e.g., "Ivermectin and Mebendazole and Fenbendazole" → " Ivermectin, Mebendazole & Fenbendazole")

* 6.12 [canonical_name] is capitalized following the "Chicago Manual of Style" rules, subject to the scientific-nomenclature overrides in section 3

* 6.13 The [intervention] is inspected and, if multiple variations, names, alternates, synonyms, or spellings exist, all of them, except [canonical_name], are stated as a comma-separated list as "alternate_names: [alternate_names]" (e.g., "Low Dose Naltrexone, LDN, Naltrexone")

`Reconstructing the topic.`

* 6.14 The [canonical_topic] is stated as "canonical_topic: 'Replacing [intervention] by [canonical_name] in [topic]' (e.g., "LDN to Optimize Immune Function" → "canonical_topic: Low-Dose Naltrexone to Optimize Immune Function")

* 6.15 The [canonical_topic] is simplified in a way that prepositions and wordiness are removed where possible, but the essence is not altered (e.g., "Using a combination of Ivermectin, Mebendazole & Fenbendazole to Fight Cancer" → "Ivermectin, Mebendazole & Fenbendazole to Fight Cancer")

* 6.16 [canonical_topic] is capitalized following the "Chicago Manual of Style" rules, subject to the scientific-nomenclature overrides in section 3

`Allowing multiple reviews of the same intervention for different goals. The default goal has no extension. Variations have a one-word extension in parentheses.`

* 6.17 If [goal] is "Health & Longevity" [short_goal] is stated as "short_goal: Longevity".

* 6.18 If [goal] is not "Health & Longevity", a one keyword form of [goal] containing no verb, is stated as "short_goal: [short_goal]" (e.g., "Optimize Immune Function" → "Immune", "Improve Insulin Sensitivity" → "Insulin", "Support Cardiovascular Health" → "Cardiovascular", "Reduce Inflammation" → "Inflammation", "Skin Rejuvenation" → "Skin")

* 6.19 If [short_goal] is "Longevity" [short_topic] is stated as "short_topic: [canonical_name]"

* 6.20 If [short_goal] is not "Longevity" [short_topic] is stated as "short_topic: [canonical_name] ([short_goal])" (e.g., "Low-Dose Naltrexone (Immune)", "Metformin (Cancer)")

* 6.21 [short_topic] is capitalized following the "Chicago Manual of Style" rules, subject to the scientific-nomenclature overrides in rule section 3

* 6.22 [short_topic_lc]  is stated as "short_topic_lc: ``{[short_topic] converted to lowercase, then spaces and dashes are replaced with underscores, then any remaining non-alphanumeric, non-underscore characters are removed, then any consecutive underscores are collapsed to a single underscore, then any leading or trailing underscores are stripped}`` " (e.g., "Low-Dose Naltrexone (Immune)" → "low_dose_naltrexone_immune", "L-Theanine" → "l_theanine", "EPA & DHA" → "epa_dha", "Vitamin K2 (MK-4 & MK-7)" → "vitamin_k2_mk_4_mk_7"))

`AI name & Prompt`

* 6.23 Version of the AI4L.md file used to create the document is stated as "prompt_version: [Version of AI4L.md]"
* 6.24 Creation date and time of the document is stated as "creation_date: [YYYY-MMDD-HHMM]" (e.g., 2026-0501-1430)

* 6.25 The nickname of the AI used to create the document is stated as "creator_ai_nickname: [creator_ai_nickname]"
* 6.26 The nickname of the AI is just a single word model name without version, etc. (e.g., Opus, Sonnet, Grok, Gemini, ChatGPT)

* 6.27 The full name of the AI used to create the document is stated as "creator_ai_fullname: [creator_ai_fullname]"
* 6.28 The full name of the AI consists of the [creator_ai_nickname] and the model version number and no additional qualifier (e.g., Opus 4.6, Sonnet 3.2, Grok 4.5, Gemini 3.1, ChatGPT 5.4)

* 6.29 The knowledge cutoff of the AI is stated as "knowledge_cutoff: [knowledge_cutoff]"

`Filename formatting rules allow for multiple ERs for the same intervention with different goals.`

* 6.30 The filename of the document is stated as "filename: [short_topic_lc]_[creation_date]_[creator_ai_nickname]_ER.md"
 (e.g., "Low-Dose Naltrexone (Immune)" → "low_dose_naltrexone_immune_2026-MMDD-HHMM_creator_ai_nickname_ER.md")

`Cleanliness and consistency of frontmatter values`

* 6.31 All frontmatter values are trimmed: no leading or trailing whitespace, no surrounding quotes unless the value contains a colon, bracket, or leading special character that requires YAML quoting.


## 7. Title

* 7.1 Title is formatted as H1
* 7.2 Title is stated as [canonical_topic]
* 7.3 Title is followed by a line stating `\<section id="top" markdown="1">\</section>`
* 7.4 A next line follows, stating "Evidence Review created on [creation_date reformatted as MM/DD/YYYY] using \[AI4L]\(https://github.com/forever-healthy/AI4L) / [creator_ai_fullname]"
* 7.5 If [alternate_names] exist, a line follows stating "**Also known as:** [alternate_names]" and the line is preceded by a blank line


## 8. Sections

* 8.1 Document is structured in sections
* 8.2 All sections present in the order given below

* 8.3 Motivation
* 8.4 Recommended Reading
* 8.5 Grokipedia
* 8.6 Examine
* 8.7 ConsumerLab
* 8.8 Systematic Reviews
* 8.9 Mechanism of Action
* 8.10 Historical Context & Evolution
* 8.11 Expected Benefits
* 8.12 Benefit-Modifying Factors
* 8.13 Potential Risks & Side Effects
* 8.14 Risk-Modifying Factors
* 8.15 Key Interactions & Contraindications
* 8.16 Risk Mitigation Strategies
* 8.17 Therapeutic Protocol
* 8.18 Discontinuation & Cycling
* 8.19 Sourcing and Quality
* 8.20 Practical Considerations
* 8.21 Interaction with Foundational Habits
* 8.22 Monitoring Protocol & Defining Success
* 8.23 Emerging Research
* 8.24 Conclusion

`Checklist items for the individual sections start here.`


## 9. Recommended Reading

* 9.1 Section starts with a one-sentence description of its contents

* 9.2 A real-time search was performed for the topic for content that is directly relevant and gives a "high-level" overview of the topic: they discuss the specific topic by name, or its primary mechanism/therapeutic category, in substantial depth
* 9.3 A statement by the author about the search is placed inside an HTML comment

* 9.4 Content is of eligible type: blog posts, podcast episodes, video presentations, YouTube lectures, expert commentary, and qualifying academic articles (primary research, narrative reviews, editorials — NOT systematic reviews or meta-analyses)

* 9.5 Content from the following experts is prioritized (where directly relevant content exists):

 - Rhonda Patrick (foundmyfitness.com)
 - Peter Attia (peterattiamd.com)
 - Andrew Huberman (hubermanlab.com)
 - Chris Kresser (chriskresser.com)
 - Life Extension Magazine (lifeextension.com)

`The auditor must verify that no relevant content from a prioritized expert is overlooked, especially where the ER claims no relevant content was found. Two independent searches are required for each expert:`

`1. Web search: search for "<expert name> <intervention>" using all available web search tools (e.g., the built-in "WebSearch", or an MCP-provided search tool).`

`2. On-site search: use "browser_navigate" to load the expert's platform and search for the intervention directly via the site's own search function, then "browser_snapshot" to read the results.`

`Only after both searches return no relevant results may the auditor confirm the "not found" claim. A brief mention within a broader episode or article still counts as relevant content if it discusses the intervention by name in a health context.`

* 9.6 The following are excluded: Grokipedia, Examine, ConsumerLab content (these have their own dedicated sections)
* 9.7 The following are excluded: Systematic reviews, meta-analyses (these belong in the Systematic Reviews section)
* 9.8 The following are excluded: Encyclopedias, wikis, AI-generated reference sites (e.g., Wikipedia)
* 9.9 The following are excluded: Forums, Reddit threads, community discussion boards, Q&A sites, social media posts
* 9.10 The following are excluded: All mainstream media - TV, print, and radio, including their related online content (e.g., MSNBC, FOX, NYT, WAPO, ...)

* 9.11 Exactly 5 items are listed (or fewer if justified with a brief explanation, visible to the user, of why only fewer could be found)
* 9.12 No more than one item per expert, publication, or organization is included (no duplicates from the same source)

* 9.13 Items are presented in a bulleted list, not a numbered list

* 9.14 The title of each item is linked to the actual article
* 9.15 The title is shown in plain text, not bold text
* 9.16 The title is followed by a reference to the author of the item in the form of " - <author>"
* 9.17 ONLY if no author is available, the publication is used in the form of " - <publication>"

* 9.18 If the item is a scientific paper, the publication year of the paper is included in the form of " - <author>, <year>"

* 9.19 If the item is a scientific paper, the author attribution is an actual person's name or "<last name> et al.", not a journal name, article type descriptor, or organization name

`If uncertain, the auditor must look up the actual author(s) of the linked resource.`

* 9.20 Each link is verified per Section 5 (Loading, Content, Semantics)

`To evaluate 9.20: re-run Section 5 items 5.1 through 5.10 against every link in this section. 9.20 fails if any applicable Section 5 item fails for any link.`

* 9.21 Each item has a 1–2 sentence annotation in a new paragraph explaining its specific value
* 9.22 The new paragraph for the annotation is separated from the title by a blank line

* 9.23 If content from one or more of the priority experts could not be found, a brief note, visible to the user, at the end of the section, explains why
* 9.24 If fewer than 5 high-quality sources could be found, a brief note, visible to the user, explains this, and the list is not padded with marginally relevant content


## 10. Grokipedia

* 10.1 grokipedia.com was searched directly for the intervention using the browser tool
* 10.2 A statement by the author about the search is placed inside an HTML comment

* 10.3 An independent direct search of grokipedia.com by the auditor using the browser tool has confirmed the presence/absence of the intervention

`The auditor must independently verify the presence/absence of the intervention on grokipedia.com by searching the site directly using the browser tool: "browser_navigate" to load grokipedia.com's search results for the intervention, then "browser_snapshot" to read the results.`

`If the auditor's search contradicts the statement in the document, set all other items in this section to "N/A" and proceed with the next section.`

* 10.4 If an article exists, a link to the Grokipedia article is provided
* 10.5 Suffixes in the link title, such as " — <sitename>", are removed from the link title

* 10.6 If an article exists, the link is verified per Section 5 (Loading, Content, Semantics) and points to the site's primary, dedicated page for the intervention — not a filtered search view, research feed, subpage, or FAQ entry

`To evaluate 10.6: re-run Section 5 items 5.1 through 5.10 against the Grokipedia link. 10.6 fails if any applicable Section 5 item fails, or if the link points to a search view, research feed, subpage, or FAQ entry instead of the primary page.`

* 10.7 If an article exists, a 1–2 sentence annotation explains its specific value (in a new paragraph)

* 10.8 If no article exists, this is explicitly stated
* 10.9 The section has no introductory text (besides the invisible HTML comment on the search)


## 11. Examine

* 11.1 examine.com was searched directly for the intervention using the browser tool
* 11.2 A statement by the author about the search is placed inside an HTML comment

* 11.3 An independent direct search of examine.com by the auditor using the browser tool has confirmed the presence/absence of the intervention

`The auditor must independently verify the presence/absence of the intervention on examine.com by searching the site directly using the browser tool: "browser_navigate" to load examine.com's search results for the intervention, then "browser_snapshot" to read the results.`

`If the auditor's search contradicts the statement in the document, set all other items in this section to "N/A" and proceed with the next section.`

* 11.4 If an article exists, a link to the Examine article is provided
* 11.5 The link title is the title of the Examine article
* 11.6 Suffixes in the link title, such as " — <sitename>", are removed from the link title

* 11.7 If an article exists, the link is verified per Section 5 (Loading, Content, Semantics) and points to the site's primary, dedicated page for the intervention — not a filtered search view, research feed, subpage, or FAQ entry

`To evaluate 11.7: re-run Section 5 items 5.1 through 5.10 against the Examine link. 11.7 fails if any applicable Section 5 item fails, or if the link points to a search view, research feed, subpage, or FAQ entry instead of the primary page.`

* 11.8 If an article exists, a 1–2 sentence annotation explains its specific value (in a new paragraph)

* 11.9 If no article exists, this is explicitly stated
* 11.10 If the intervention is a prescription drug and no article was found, this is noted with an explanation (e.g., "Examine.com does not typically cover prescription medications")

* 11.11 The section has no introductory text (besides the invisible HTML comment on the search)


## 12. ConsumerLab

* 12.1 consumerlab.com was searched directly for the intervention using the browser tool
* 12.2 A statement by the author about the search is placed inside an HTML comment

* 12.3 An independent direct search of consumerlab.com by the auditor using the browser tool has confirmed the presence/absence of the intervention

`The auditor must independently verify the presence/absence of the intervention on consumerlab.com by searching the site directly using the browser tool: "browser_navigate" to load consumerlab.com's search results for the intervention, then "browser_snapshot" to read the results.`

`If the auditor's search contradicts the statement in the document, set all other items in this section to "N/A" and proceed with the next section.`

* 12.4 If an article exists, a link to the ConsumerLab article is provided
* 12.5 The link title is the title of the ConsumerLab article
* 12.6 Suffixes in the link title, such as " — <sitename>", are removed from the link title

* 12.7 If an article exists, the link is verified per Section 5 (Loading, Content, Semantics) and points to the site's primary, dedicated page for the intervention — not a filtered search view, research feed, subpage, or FAQ entry

`To evaluate 12.7: re-run Section 5 items 5.1 through 5.10 against the ConsumerLab link. 12.7 fails if any applicable Section 5 item fails, or if the link points to a search view, research feed, subpage, or FAQ entry instead of the primary page.`

* 12.8 If an article exists, a 1–2 sentence annotation explains its specific value (in a new paragraph)

* 12.9 If no article exists, this is explicitly stated
* 12.10 If the intervention is a prescription drug and no article was found, this is noted with an explanation (e.g., "ConsumerLab does not typically cover prescription medications")

* 12.11 The section has no introductory text (besides the invisible HTML comment on the search)

## 13. Systematic Reviews

* 13.1 A real-time PubMed search was performed for the intervention with "systematic review OR meta-analysis"
* 13.2 Selection was prioritized by citation count (if available), study size, publication date (recent preferred), and relevance

`The auditor must perform an independent PubMed search (e.g., using "pubmed_search_articles") for "systematic review OR meta-analysis" of the intervention to verify that no relevant paper is overlooked.`

* 13.3 If results exist, the section starts with a one-sentence description of its contents
* 13.4 If NO results exist, the one-sentence description of the section's contents is omitted

* 13.5 Up to 5 papers are listed
* 13.6 Papers are presented in a bulleted list, not a numbered list
* 13.7 All papers listed are relevant to the specific intervention being analyzed
* 13.8 All papers listed are actually a systematic review or meta-analysis (not a narrative review or primary study)

* 13.9 The title of each paper is linked to the paper's PubMed abstract page using the format "https://pubmed.ncbi.nlm.nih.gov/PMID/"

* 13.10 No links point to publisher websites (e.g., ahajournals.org, sciencedirect.com, wiley.com)

* 13.11 Each link is verified per Section 5 (Loading, Content, Semantics)

* 13.12 The PubMed-retrieved title matches the title in the ER

`The auditor must retrieve the metadata for every PubMed link in this section (e.g., by fetching the PMID or using a PubMed lookup tool) and verify that the title returned by PubMed matches the title stated in the ER. A mismatch means the PMID was fabricated or assigned to the wrong paper.`

* 13.13 The title is shown in plain text, not bold text
* 13.14 The title is followed by a reference to the author(s) and the publication year of the paper
* 13.15 For single-author works "- <last name>, <year>" is used
* 13.16 For dual-author works "- <last name> & <last name>, <year>" is used
* 13.17 For works with more than two authors "- <last name> et al., <year>" is used

`The auditor must verify that each author attribution is an actual person's name, not a journal name, article type descriptor, or organization name. If uncertain, the auditor must look up the actual author(s) of the linked resource.`

* 13.18 Each result has a 1–2 sentence annotation explaining its specific value (in a new paragraph)
* 13.19 The new paragraph for the annotation is separated from the title by a blank line

* 13.20 If no results exist, a statement follows the exact format: "No systematic reviews or meta-analyses for \<intervention> were found on PubMed as of \<current date>."

## 14. Mechanism of Action

* 14.1 The primary biological pathways or mechanisms are explained
* 14.2 The explanation is appropriately concise but sufficient for a non-specialist to understand
* 14.3 Relevant pathway names and acronyms are properly explained per glossary rules
* 14.4 Where competing mechanistic explanations exist for or against the intervention, both are presented
* 14.5 If the intervention is a pharmacological compound, state its key pharmacological properties: half-life, selectivity, tissue distribution, and metabolism (primary pathway and relevant enzymes, e.g., CYP3A4)


## 15. Historical Context & Evolution

* 15.1 The original intended use of the intervention is discussed
* 15.2 The reasons it came to be considered for health optimization are explained
* 15.3 When historical research is discussed, the actual findings are described — not just the reception or critique of the research
* 15.4 Historical research is not dismissed with labels such as "debunked," "discredited," or "disproven" — the evidence for and against is presented, and the reader can assess the current standing
* 15.5 When describing the evolution of scientific opinion, the section does not frame the current consensus as the final word — it notes what changed and why, including what new evidence emerged on either side
* 15.6 If the section does not apply to the intervention, this is briefly noted


## 16. Expected Benefits

* 16.1 All major known benefits of the intervention are addressed (no significant omissions)

* 16.2 Content is framed for the target audience (see 1.12), not as population-level outcomes.

* 16.3 A dedicated search for the intervention's complete benefit profile was performed using clinical and expert sources before writing this section

`The auditor should perform an in-depth search using all available search tools (e.g., the built-in "WebSearch", "pubmed_search_articles", or an MCP-provided search tool) to cross-check and verify that the list of benefits presented in this section is complete, and no benefits have been left unaddressed.`

* 16.4 Each item is assigned a "Level of Evidence" grade
* 16.5 Only the following four levels are used exactly: High, Medium, Low, Speculative
* 16.6 No hybrid or intermediate levels are used (e.g., no "Low-Medium" or "Medium-High")

* 16.7 Items are grouped by evidence level
* 16.8 The groups are presented in the order: High, Medium, Low, Speculative
* 16.9 The group High is presented as "### High 🟩 🟩 🟩"
* 16.10 The group Medium is presented as "### Medium 🟩 🟩"
* 16.11 The group Low is presented as "### Low 🟩"
* 16.12 The group Speculative is presented as "### Speculative 🟨"

* 16.13 The actual item titles are presented in H4
* 16.14 The actual item levels are not shown on a per-item basis

* 16.15 Each item includes an annotation paragraph between the H4 title and the Magnitude line
* 16.16 The annotation is 2–5 sentences covering: what the benefit is, the proposed mechanism (if relevant), the evidence basis (e.g., "meta-analysis of 20 RCTs", "large observational cohort"), and any contextual nuance (limitations, population specifics, conflicting findings)
* 16.17 The annotation is preceded and followed by a blank line
* 16.18 Speculative items also include an annotation; if no controlled studies exist, it notes the basis is mechanistic or anecdotal only

* 16.19 Where evidence is directly conflicted, a "⚠️ Conflicted" flag appears directly after the item name in the title (not in the annotation)
* 16.20 Conflicted evidence is explained in the annotation text

* 16.21 Each item (except Speculative) includes a "**Magnitude:** " line with a specific number, range, or comparison
* 16.22 If no quantitative data are available, the exact phrasing "**Magnitude:** Not quantified in available studies." is used
* 16.23 Items classified as "Speculative" do NOT include a magnitude line
* 16.24 The magnitude line is preceded by a blank line

* 16.25 Each item's evidence grade is appropriate, given the cited studies and data
* 16.26 Each item is verifiable by the sources cited or by independent lookup
* 16.27 Magnitude values are plausible and consistent with known clinical data
* 16.28 No items are overstated relative to their evidence level
* 16.29 No items are understated relative to their evidence level


## 17. Benefit-Modifying Factors

* 17.1 Genetic polymorphisms that may modify benefits are discussed where relevant (e.g., variants affecting drug transport, metabolism, or disease susceptibility)
* 17.2 Baseline biomarker levels are discussed as a factor influencing benefits
* 17.3 Known sex-based differences in benefits are discussed
* 17.4 Pre-existing health conditions that may influence benefits are discussed
* 17.5 Age-related considerations are discussed (including for those at the older end of the target range)
* 17.6 Each factor is presented as a bulleted item with a short bold label followed by a descriptive explanation

* 17.7 If none of the above is relevant to the intervention, it is briefly stated


## 18. Potential Risks & Side Effects

* 18.1 All major known risks and side effects of the intervention are addressed (no significant omissions)

* 18.2 Content is framed for the target audience (see 1.12), not as population-level outcomes.

* 18.3 A dedicated search for the intervention's complete side effect profile was performed using a drug reference source (e.g., prescribing information, drugs.com, Mayo Clinic) before writing this section

`The auditor should perform an in-depth search using all available search tools (e.g., the built-in "WebSearch", "pubmed_search_articles", or an MCP-provided search tool) to cross-check and verify that the list of risks and side effects presented in this section is complete, and no risks or side effects have been left unaddressed.`

* 18.4 Each item is assigned a "Level of Evidence" grade
* 18.5 Only the following four levels are used exactly: High, Medium, Low, Speculative
* 18.6 No hybrid or intermediate levels are used (e.g., no "Low-Medium" or "Medium-High")

* 18.7 Items are grouped by evidence level
* 18.8 The groups are presented in the order: High, Medium, Low, Speculative
* 18.9 The group High is presented as "### High 🟥 🟥 🟥"
* 18.10 The group Medium is presented as "### Medium 🟥 🟥"
* 18.11 The group Low is presented as "### Low 🟥"
* 18.12 The group Speculative is presented as "### Speculative 🟨"

* 18.13 The actual item titles are presented in H4
* 18.14 The actual item levels are not shown on a per-item basis

* 18.15 Each item includes an annotation paragraph between the H4 title and the Magnitude line
* 18.16 The annotation is 2–5 sentences covering: what the risk is, the proposed mechanism (if known), the evidence basis (e.g., "clinical trials", "FDA information", "post-marketing reports"), and any contextual nuance (severity, reversibility, at-risk populations, comparison to other agents in the class)
* 18.17 The annotation is preceded and followed by a blank line
* 18.18 Speculative items also include an annotation; if no controlled data exist, it notes the basis is mechanistic or from isolated reports

* 18.19 Where evidence is directly conflicted, a "⚠️ Conflicted" flag appears directly after the item name in the title (not in the annotation)
* 18.20 Conflicted evidence is explained in the annotation text

* 18.21 Each item (except Speculative) includes a "**Magnitude:** " line with a specific number, range, or comparison
* 18.22 If no quantitative data are available, the exact phrasing "**Magnitude:** Not quantified in available studies." is used
* 18.23 Items classified as "Speculative" do NOT include a magnitude line
* 18.24 The magnitude line is preceded by a blank line

* 18.25 Each item's evidence grade is appropriate given the cited studies and data
* 18.26 Each item is verifiable by the sources cited or by independent lookup
* 18.27 Magnitude values are plausible and consistent with known clinical data
* 18.28 No items are overstated relative to their evidence level
* 18.29 No items are understated relative to their evidence level


## 19. Risk-Modifying Factors

* 19.1 Genetic polymorphisms that may modify risk or side effects are discussed where relevant (e.g., variants affecting drug transport, metabolism, or disease susceptibility)
* 19.2 Baseline biomarker levels are discussed as a factor
* 19.3 Known sex-based differences in risks and side effects are discussed
* 19.4 Pre-existing health conditions that may influence risks and side effects are discussed
* 19.5 Age-related considerations are discussed (including for those at the older end of the target range)
* 19.6 Each factor is presented as a bulleted item with a short bold label followed by a descriptive explanation

* 19.7 If none of the above is relevant to the intervention, it is briefly stated


## 20. Key Interactions & Contraindications

* 20.1 Common prescription drug interactions are listed
* 20.2 Over-the-counter medication interactions are listed
* 20.3 Supplement interactions are listed
* 20.4 Supplements known to have additive effects with the intervention are included (e.g., supplements that also lower blood pressure when evaluating an antihypertensive)
* 20.5 Other intervention interactions are discussed where applicable
* 20.6 Populations who should avoid this intervention are clearly identified

* 20.7 For drug or supplement interventions: when listing a drug class, include representative named drugs in parentheses (e.g., "CYP3A4 inhibitors (ketoconazole, ritonavir, grapefruit juice)")
* 20.8 Each interaction states a severity (e.g., absolute contraindication, caution, monitor) and the clinical consequence (e.g., severe hypotension, increased bleeding risk)
* 20.9 If a mitigating action is known (dose reduction, timing separation, monitoring schedule), it is stated
* 20.10 Populations to avoid include specific thresholds or classifications where applicable (e.g., "recent MI (<90 days)", "Child-Pugh Class C", "NYHA Class IV") — not only general categories


## 21. Risk Mitigation Strategies

* 21.1 Practical risk mitigation strategies are provided
* 21.2 The strategies are specific to the risks identified in the Risks section
* 21.3 The strategies are actionable by the target audience (see 1.12)
* 21.4 Each strategy is presented as a bulleted item beginning with a short bold label, followed by descriptive explanation (e.g., "**Low starting dose with slow titration:** protocols typically begin at 2.5 mg daily, increasing to 5 mg after 1–2 weeks if tolerated")
* 21.5 Strategies include specific parameters where applicable: doses, frequencies, thresholds, timeframes, or quantitative targets (e.g., "dose escalation of 250–500 mg every 1–2 weeks", "annual eGFR monitoring")
* 21.6 Each strategy explicitly states the risk or consequence it mitigates (by naming the risk or describing what the strategy prevents)


## 22. Therapeutic Protocol

* 22.1 A standard protocol is described as used by leading practitioners
* 22.2 Where competing therapeutic approaches exist (e.g., conventional vs. integrative), the main alternatives are presented without framing one as the default
* 22.3 Where possible, the expert or clinic that popularized each approach is cited
* 22.4 Best time of day for the intervention is discussed

* 22.5 For supplements/medications: the expected half-life of the compound in the human body is discussed
* 22.6 For supplements/medications: whether to take as a single dose or split doses is discussed

* 22.7 Genetic polymorphisms that may influence protocol or dose choice are discussed (e.g., APOE4, MTHFR, COMT, pharmacogenetically relevant variants)
* 22.8 Known sex-based differences in response, dosing, or efficacy are discussed
* 22.9 Age-related considerations are discussed (including for those at the older end of the target range)
* 22.10 Baseline biomarker levels are discussed as a factor influencing response
* 22.11 Pre-existing health conditions that may influence response are discussed
* 22.12 Each protocol item is presented as a bulleted item with a short bold label followed by descriptive explanation


## 23. Discontinuation & Cycling

* 23.1 Whether the intervention is meant to be lifelong or short-term is addressed
* 23.2 Known withdrawal effects (if any) are discussed
* 23.3 Tapering-off protocol is discussed (if applicable)
* 23.4 Whether cycling is recommended for maintaining efficacy is addressed
* 23.5 Each discontinuation or cycling consideration is presented as a bulleted item with a short bold label followed by descriptive explanation

## 24. Sourcing and Quality

* 24.1 Source, purity, and formulation considerations are addressed
* 24.2 What to look for is explained (e.g., third-party testing, specific nutrient forms)
* 24.3 Reputable brands or compounding pharmacies are mentioned where relevant
* 24.4 If the section is not applicable to the intervention, this is briefly noted
* 24.5 Each sourcing consideration is presented as a bulleted item with a short bold label followed by descriptive explanation



## 25. Practical Considerations

* 25.1 Time to effect is discussed: how long does it typically take to observe benefits?
* 25.2 Common pitfalls are discussed: what mistakes do people commonly make?
* 25.3 Regulatory status is addressed if applicable (e.g., off-label use, FDA regulation)
* 25.4 Cost and accessibility are briefly noted if the intervention is exceptionally expensive or difficult to access
* 25.5 Each consideration is presented as a bulleted item with a short bold label followed by descriptive explanation (e.g., "**Time to effect:**")


## 26. Interaction with Foundational Habits

* 26.1 Interaction with sleep is analyzed (e.g., Can it disrupt sleep? Improve sleep quality?)
* 26.2 Interaction with nutrition is analyzed (e.g., Best with a specific diet? Depletes nutrients?)
* 26.3 Interaction with exercise is analyzed (e.g., Blunts hypertrophy? Timing around workouts?)
* 26.4 Interaction with stress management is analyzed (e.g., Affects cortisol or stress response?)
* 26.5 Each interaction is presented as a bulleted item with a bold label (Sleep, Nutrition, Exercise, Stress management) followed by a descriptive explanation
* 26.6 Each interaction states the direction of the interaction (direct, indirect, none, potentiating, blunting), the proposed mechanism where known, and specific practical considerations where applicable (e.g., foods to include or avoid, timing relative to dosing, technique variants, named studies)


## 27. Monitoring Protocol & Defining Success

* 27.1 Baseline labs and tests are specified (what to do before starting)
* 27.2 Ongoing labs and tests are specified with monitoring frequency
* 27.3 Lab tests are presented in a table with the following columns: Biomarker, Optimal Functional Range, Why Measure It?, Context/Notes
* 27.4 Optimal ranges reflect functional medicine practitioner guidance (not just conventional reference ranges)
* 27.5 Where the conventional reference range differs meaningfully from the optimal functional range, it is included in the Context/Notes column
* 27.6 The "Why Measure It?" column provides extremely concise explanations
* 27.7 The Context/Notes column covers relevant details such as fasting requirements, best paired tests, and time-of-day considerations
* 27.8 Qualitative markers are discussed (e.g., sleep quality, energy levels, cognitive clarity)

* 27.9 Baseline testing is introduced with a descriptive statement outside the biomarker table (not only implied by the table's contents)
* 27.10 Ongoing monitoring is introduced with a cadence statement specifying timepoints (e.g., "at 1 week, 4 weeks, then every 3–6 months" or "every 6–12 months")
* 27.11 Qualitative markers are presented as a bulleted list

* 27.12 If the section is not applicable to the intervention, this is briefly noted


## 28. Emerging Research

* 28.1 Content is framed for the target audience (see 1.12), not as population-level outcomes.

* 28.2 Major ongoing clinical trials are mentioned (e.g., from clinicaltrials.gov)
* 28.3 All clinical trial NCT IDs are hyperlinked to "https://clinicaltrials.gov/study/\<NCT ID>"

* 28.4 Areas of future research that could change current understanding are noted
* 28.5 Emerging research is presented from all relevant directions — studies that could strengthen and studies that could weaken the case for the intervention are both included

* 28.6 All cited publications include a hyperlink (PubMed, DOI, or publisher URL)
* 28.7 Every NCT ID, PMID, and DOI mentioned in the section is an active hyperlink, not plain text
* 28.8 Any other referenced source with a known URL is linked
* 28.9 Ongoing trials are presented with a descriptive title or aim, the NCT ID as a hyperlink (or a note if no NCT ID exists), and key details such as participant count, phase, or primary endpoint where available
* 28.10 Future research areas reference specific studies or meta-analyses by author/year with a hyperlink where relevant published evidence exists
* 28.11 Emerging Research entries are presented as bulleted items beginning with a short bold label

* 28.12 Each link is verified per Section 5 (Loading, Content, Semantics)
* 28.13 For PubMed links: the PubMed-retrieved title matches the title in the ER
* 28.14 For clinicaltrials.gov links: the NCT ID resolves to a trial related to the intervention


## 29. Conclusion

* 29.1 The conclusion is a cohesive summary of the document as a whole
* 29.2 It is framed for the target audience (see 1.12)
* 29.3 It reflects what the intervention is, the main benefits, the main risks and considerations
* 29.4 It reflects on the overall quality of the evidence base (including any conflicts of interest per 2.8–2.10)

* 29.5 It is 150–250 words
* 29.6 It is consistent with the evidence levels assigned in the Benefits and Risks sections
* 29.7 Where evidence is uncertain, the conclusion conveys that uncertainty

* 29.8 It DOES NOT introduce new evidence or claims not previously discussed
* 29.9 It DOES NOT use acronyms that would require glossary expansion or technical classifications that require specialist knowledge, uses plain-language terms instead

`For the conclusion section, the test is not whether a word looks technical, but whether a non-specialist would use it unprompted and know its exact meaning — this also catches ordinary-looking clinical-register words (e.g., "adjunct" → "add-on", "hormetic" → "brief beneficial stress", "potentiate" → "strengthen", "modality" →  "type/method").`

* 29.10 It DOES NOT cite specific trials (names, years, sample sizes, p-values)
* 29.11 It DOES NOT cite effect sizes, relative risks, or statistical results

* 29.12 It DOES NOT use prescriptive or advisory language — it summarizes evidence, not what anyone should do
* 29.13 It DOES NOT address the reader directly

* 29.14 It DOES NOT force a pro-vs-con structure or equal treatment of competing positions
* 29.15 It DOES NOT state what further evidence is needed or what should be done
* 29.16 It DOES NOT frame its takeaway around an average-risk or general population
* 29.17 It DOES NOT close on generic cautions that apply to a population the reader is not part of
* 29.18 It DOES NOT characterize any position on the intervention as the accepted, mainstream, or settled view
* 29.19 It DOES NOT frame one position as the default and the other as requiring extra justification

* 29.20 The conclusion section ends with a line stating "**\[Top]\(#top) - \[Benefits]\(#expected-benefits) - \[Risks]\(#potential-risks--side-effects) - \[Protocol]\(#therapeutic-protocol)**"
* 29.21 The ending line is preceded by a blank line

`The checklist items for the motivation section are the last items in the checklist, but the motivation section itself is the first section of the document. This is due to the motivation section should be written last, after all other sections are completed, to ensure it accurately reflects the full scope of the review.`

## 30. Motivation

* 30.1 The motivation section is located after the title and frontmatter but before all other sections
* 30.2 A statement by the author that the section was only written after the rest of the document was written to have the full scope of the topic is placed inside an HTML comment

* 30.3 The section provides a brief overview of the topic
* 30.4 The section covers the essential aspects of the topic
* 30.5 The section is a concise, accessible introduction
* 30.6 The purpose and focus of the document are clearly explained
* 30.7 The motivation does NOT jump to conclusions or preempt the analysis results
* 30.8 The motivation does NOT characterize any position on the intervention as the accepted, mainstream, or settled view — it presents the debate neutrally

* 30.9 Where relevant, the motivation connects the intervention to the health and longevity lens of the target audience (see 1.12)

* 30.10 The motivation consists of three paragraphs
* 30.11 First paragraph explains in plain language what the intervention is and why it is of interest
* 30.12 Second paragraph gives additional context — e.g., historical use, prevalence, or a single headline finding that motivates the review
* 30.13 The final paragraph is a crisp statement of what this review examines and why
* 30.14 The final paragraph does not use evaluative or advisory language — it states what the review
  examines, not what anyone should do

* 30.15 The motivation is 150–200 words

* 30.16 DOES NOT use acronyms that would require glossary expansion or technical classifications that require specialist knowledge, uses plain-language terms instead

`For the motivation section, the test is not whether a word looks technical, but whether a non-specialist would use it unprompted and know its exact meaning — this also catches ordinary-looking clinical-register words (e.g., "adjunct" → "add-on", "hormetic" → "brief beneficial stress", "potentiate" → "strengthen", "modality" →  "type/method").`

* 30.17 DOES NOT parenthetically define standard terms (e.g., "CRP (C-reactive protein, a general marker of systemic inflammation)")
* 30.18 Parenthetical alternate names are acceptable when introducing the intervention. E.g., "Niacin (vitamin B3)"
* 30.19 DOES NOT cite specific trials (names, years, sample sizes, p-values)
* 30.20 DOES NOT cite effect sizes, relative risks, or statistical results
* 30.21 DOES NOT contain exhaustive mechanism-of-action descriptions; a single sentence on the primary mechanism is sufficient
* 30.22 DOES NOT list every health domain the intervention touches, mentions the most notable 2–3 at most
* 30.23 DOES NOT mention specific influencers, podcasters, or biohackers (e.g., "Peter Attia recommends…")

* 30.24 The motivation section ends with a line stating "**\[Benefits]\(#expected-benefits) - \[Risks]\(#potential-risks--side-effects) - \[Protocol]\(#therapeutic-protocol) - \[Conclusion]\(#conclusion)**"
* 30.25 The ending line is preceded by a blank line

## 31. End of Document

* 31.1 The document does not include content past the conclusion

`DO NOT remove the <section> tags below. They are used for internal linking and must remain in place`

<section id="iterations" markdown="1"></section>

`DO NOT add any extra text to the heading of the Issues section.`
`The heading must be exactly as given below, in the form of "Issues '%d/%m/%Y %H:%M'`

## Issues [audit_date reformatted as '%d/%m/%Y %H:%M']

`List any issues identified during the audit here. Format described in the "Documentation" section.`

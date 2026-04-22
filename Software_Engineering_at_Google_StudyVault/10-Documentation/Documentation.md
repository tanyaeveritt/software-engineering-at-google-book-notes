---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: documentation, code comments, doc types, audience, freshness, technical writing
---

# Documentation (Importance: ★★★)

#documentation #processes

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Treat Docs Like Code | Source control, owners, reviews, bug tracking, freshness dates |
| Know Your Audience | Seekers (consistency) vs Stumblers (clarity); experts vs novices |
| Five Doc Types | Reference, design docs, tutorials, conceptual, landing pages |
| One Purpose Per Doc | Don't mix doc types; a doc doing multiple jobs does none well |
| Docs Reviews | Technical (accuracy), audience (clarity), writing (consistency) |
| Freshness Dates | Metadata reminders when docs haven't been reviewed in months |

## What Qualifies as Documentation?
Documentation includes **every supplemental text** an engineer writes: standalone documents **and** code comments. Most documentation at Google comes in the form of code comments.

## Why Is Documentation Needed?
Documentation benefits are **downstream** — they don't provide immediate returns to the author, unlike testing. But like testing investments, they pay off over time.
- Written once, read **hundreds or thousands** of times — cost is amortized across future readers
- Critical for the organization to **scale** — answers "Why were these design decisions made?" years later
- State of engineering documentation in 2010s is like software testing in late 1980s — recognized as important but lacking organizational commitment

**Why engineers resist documentation**:
- View writing as a separate skill from programming
- Don't feel like capable writers
- Limited tool support and workflow integration
- Seen as an extra burden rather than a maintenance enabler

**Benefits to the writer**:
- Helps **formulate APIs** — if you can't explain it, you haven't designed it well enough
- Provides a **road map for maintenance** and historical record
- Makes code look more **professional** and drives traffic
- **Fewer questions** from other users over time (biggest long-term benefit)

## Documentation Is Like Code
Documentation should be treated as code:
- Have **internal policies** and rules
- Be placed under **source control**
- Have clear **ownership** (documents without owners become stale)
- Undergo **reviews** for changes (change with the code it documents)
- Have **issues tracked** like bugs
- Be periodically **evaluated** for accuracy and freshness
- Designate **canonical documentation** to avoid duplicates (use go/ links)

### Case Study: The Google Wiki (GooWiki)
Early Google used a shared wiki. Problems at scale:
- No true owners → documents became **obsolete** (~90% had no views/updates when deprecated)
- No process for new documents → **duplicates** proliferated (7-10 docs on setting up Borg)
- People who could **fix** docs were not the people who **used** them
- Documentation became Google's **#1 developer complaint**

**Solution**: Move docs under **source control** with owners, code review, bug tracking. Quality improved dramatically. Introduction of **Markdown** and **g3doc** (docs alongside source code) further improved adoption.

## Know Your Audience
The most important mistake: **writing only for yourself**.

### Types of Audiences
Three dimensions:
- **Experience level**: expert programmers vs junior engineers
- **Domain knowledge**: team members vs engineers who know only API endpoints
- **Purpose**: end users needing quick answers vs maintainers needing implementation details

### Seekers vs Stumblers
- **Seekers** know what they want → need **consistency** (uniform comment format for scanning)
- **Stumblers** don't know what they want → need **clarity** (overviews, introductions, TL;DR)

### Customers vs Providers
Keep documents for **users** separate from documents for **team members**. Don't leak implementation details into API documentation.

## Documentation Types
Five main types — **don't mix them** within a single document:

### 1. Reference Documentation
Most common type. Includes code comments (API comments vs implementation comments). Generated from code comments where possible (**single-sourced**). Google embeds reference docs in header files (C++), Javadoc (Java), PyDoc (Python).
- **File comments**: outline what's in the code, main use cases, intended audience
- **Class comments**: "nouned" — describe the object aspect
- **Function comments**: start with **indicative verb** ("Merges," "Deletes," "Creates")
- Avoid boilerplate sections (Returns:, Throws:) — prefer **single prose** that naturally documents postconditions, parameters, return values, and exceptions together

### 2. Design Documents
Required before major projects. Collaborative (often in Google Docs). Cover goals, implementation strategy, key design decisions with **trade-offs**, alternative designs. Act as historical record and measure of project success. Cover security, internationalization, storage, privacy.

### 3. Tutorials
"Hello World" documents for new team members. Best written **when you first join a team**. Must:
- **Number every step** explicitly
- Don't number system actions (only user actions)
- Assume no domain knowledge; state prerequisites clearly
- Show user-visible input/output on separate lines in monospaced font

### 4. Conceptual Documentation
Augments (not replaces) reference docs. Sacrifices some accuracy for **clarity**. Like **integration tests** for documentation (vs code comments as unit tests). Most **difficult** to write and most **neglected** — no canonical location in source code.

### 5. Landing Pages
Should be a **traffic cop**, not a content page. Include only links to other pages. Don't serve both team and customer — create separate pages. Don't scroll multiple screens.

## Documentation Reviews
Three types:
- **Technical review** (accuracy): by subject matter expert
- **Audience review** (clarity): by someone unfamiliar with the domain
- **Writing review** (consistency): by technical writer

## Documentation Philosophy

### WHO, WHAT, WHEN, WHERE, and WHY
Most technical docs answer HOW, but address the other questions in the **first two paragraphs**:
- **WHO**: explicitly identify the audience
- **WHAT**: identify the document's purpose
- **WHEN**: creation/review date
- **WHERE**: canonical location (prefer source control)
- **WHY**: what readers should take away

### The Beginning, Middle, and End
All documents should have at least three sections. **Redundancy** in documentation is useful — introduce and summarize key points, then elaborate.

### Parameters of Good Documentation
Three aspects in tension: **completeness**, **accuracy**, and **clarity**. Rarely achieve all three — choose based on the document's purpose.

## Deprecating Documents
Old documents are as problematic as old code. Attach **freshness dates** with metadata that sends email reminders (e.g., every 3 months). Include owner and "Last reviewed by..." byline.

## When Do You Need Technical Writers?
Teams can write docs for **themselves** fine. They need help writing for **other audiences**. Technical writers should focus on **cross-API documentation** rather than supporting single teams — the latter doesn't scale and creates perverse incentives.

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Treat docs like code" | **Source control, owners, reviews, bug tracking, freshness** |
| "Seekers vs Stumblers" | **Seekers need consistency; Stumblers need clarity** |
| "One purpose per doc" | **Don't mix doc types; each doc does one job** |
| "GooWiki failure" | **No owners, duplicates, wrong people fixing docs; moved to source control** |
| "Freshness dates" | **Metadata reminders; "Last reviewed by..." byline** |
| "Function comments" | **Start with indicative verb; single prose over boilerplate** |
| "Conceptual docs" | **Most difficult to write; like integration tests for docs** |
| "Technical writers" | **Focus on cross-API docs, not single teams** |
| "Three doc reviews" | **Technical (accuracy), audience (clarity), writing (consistency)** |

## Related Notes
- [[Code Review]]
- [[Style Guides and Rules]]
- [[Knowledge Sharing]]
- [[Testing Overview]]
- [[Measuring Productivity]]

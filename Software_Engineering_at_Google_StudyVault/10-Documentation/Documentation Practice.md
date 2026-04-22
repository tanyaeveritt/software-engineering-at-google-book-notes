---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, documentation, audience, doc types, freshness
---

# Documentation Practice (10 questions)

#practice #documentation #processes

## Related Concepts
- [[Documentation]]
- [[Code Review]]
- [[Style Guides and Rules]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Docs like code | **Source control, owners, reviews, bug tracking** |
> | Seekers vs Stumblers | **Consistency vs clarity** |
> | Five doc types | **Reference, design, tutorial, conceptual, landing page** |
> | GooWiki failure | **No owners → stale; moved to source control** |
> | Freshness dates | **Metadata reminders every 3 months** |

---

## Question 1 - Docs Like Code [recall]
> What does it mean to "treat documentation like code" and what specific practices does this involve?

> [!answer]- Show Answer
> Documentation should follow the same lifecycle as code:
> - Have **internal policies and rules** to follow
> - Be placed under **source control**
> - Have clear **ownership** responsible for maintenance
> - Undergo **reviews** for changes (and change with the code it documents)
> - Have **issues tracked** like bugs in a bug tracking system
> - Be periodically **evaluated** (tested) for accuracy and freshness
> - Be measured for aspects like accuracy and freshness when possible

---

## Question 2 - Five Doc Types [recall]
> List the five main types of documentation described in Chapter 10.

> [!answer]- Show Answer
> 1. **Reference documentation** — code comments (API and implementation), generated from code
> 2. **Design documents** — goals, strategy, trade-offs, alternatives; written before major projects
> 3. **Tutorials** — "Hello World" step-by-step guides for onboarding
> 4. **Conceptual documentation** — overviews that augment reference docs; prioritize clarity over completeness
> 5. **Landing pages** — traffic cops with links only; don't mix team and customer content

---

## Question 3 - Seekers vs Stumblers [recall]
> What is the difference between "seekers" and "stumblers" as documentation audiences?

> [!answer]- Show Answer
> **Seekers** know what they want and need to quickly confirm whether the current document fits. The key tool for this audience is **consistency** — uniform comment formats that allow quick scanning.
>
> **Stumblers** don't know exactly what they want and may have only a vague idea. The key tool for this audience is **clarity** — overviews, introductions, and TL;DR statements that explain purpose upfront (e.g., "TL;DR: if you are not interested in C++ compilers at Google, you can stop reading now").

---

## Question 4 - GooWiki Case Study [recall]
> Why did Google's internal wiki (GooWiki) fail as a documentation system?

> [!answer]- Show Answer
> GooWiki failed at scale for four main reasons:
> 1. **No true owners** — documents became obsolete (~90% had no views/updates when deprecated)
> 2. **No process for new documents** — duplicates proliferated (7-10 docs on setting up Borg)
> 3. **Flat namespace** — poor hierarchy made finding canonical docs difficult
> 4. **Wrong people problem** — people who could fix docs didn't use them; people who found broken docs couldn't fix them
>
> Documentation quality became Google's **#1 developer complaint**. The solution was moving docs under source control with ownership, code review, and bug tracking.

---

## Question 5 - Function Comments [recall]
> How should function comments be written according to Google's style?

> [!answer]- Show Answer
> Function comments should start with an **indicative verb** describing what the function does and what is returned (e.g., "Merges the given strings," "Creates a new record," "Deletes the specified entry").
>
> Google prefers **single prose comments** over boilerplate sections like "Returns:", "Throws:", etc., because postconditions, parameters, return values, and exceptions are often not independent and are clearer when described together naturally.

---

## Question 6 - Three Doc Reviews [recall]
> What are the three types of documentation reviews?

> [!answer]- Show Answer
> 1. **Technical review** (accuracy): done by a subject matter expert, often during code review
> 2. **Audience review** (clarity): done by someone unfamiliar with the domain (e.g., new team member or API customer)
> 3. **Writing review** (consistency): done by a technical writer or volunteer
>
> High-profile or externally published documents should get all three types. Even one reviewer is better than none.

---

## Question 7 - Freshness Dates [recall]
> How does Google use "freshness dates" to maintain documentation quality?

> [!answer]- Show Answer
> Google attaches **freshness date metadata** to documents. When a document hasn't been touched in a specified period (e.g., three months), the system sends **email reminders** to the owner. Including the owner in the document with a "Last reviewed by..." byline increases adoption. Since documents under source control require code review to update the freshness date, this creates a low-cost mechanism for periodic review.

---

## Question 8 - Tutorial Design [application]
> You're writing a tutorial for setting up a new microservice. A colleague suggests combining the setup tutorial with an architectural overview and API reference into a single document. What's wrong with this approach?

> [!answer]- Show Answer
> This violates the principle of **one purpose per document**. Each of these is a different document type:
> - **Tutorial**: step-by-step setup instructions (number every user action, assume no domain knowledge)
> - **Conceptual documentation**: architectural overview (augments reference, prioritizes clarity)
> - **Reference documentation**: API details (complete, single-sourced from code)
>
> Mixing them creates a monolithic document that fails all audiences. Early Google had notorious wiki pages mixing everything together — they "scrolled through several dozen screens" and nobody read them. Instead, create three separate documents and use a **landing page** to link them together.

---

## Question 9 - Documentation ROI [analysis]
> The chapter compares the state of documentation in the 2010s to testing in the late 1980s. Analyze this comparison: what parallels exist, and what makes documentation harder to institutionalize than testing?

> [!answer]- Show Answer
> **Parallels**:
> - Both were recognized as important but lacked organizational commitment
> - Both require upfront investment for downstream benefits
> - Both benefit from being integrated into existing engineering workflows
> - Both improve with tooling and automation
>
> **Why documentation is harder**:
> - Tests can be **atomic** (unit tests) and follow prescribed form; documents cannot
> - Tests can be **automated** (run continuously); documentation automation schemes are lacking
> - Tests provide **immediate feedback** (pass/fail); documentation quality is measured **asynchronously** by the reader, not the writer
> - Documents are **inherently subjective** — quality depends on the reader's context and needs
> - There's no equivalent of a "test failure" for documentation — stale docs silently mislead rather than loudly breaking
>
> Despite these challenges, Google found that treating docs like code (source control, ownership, reviews) dramatically improved quality, paralleling how treating testing as a first-class engineering practice improved code quality.

---

## Question 10 - Technical Writers at Scale [analysis]
> A growing engineering organization hires technical writers and assigns one to each of its top 10 teams. Analyze whether this is an effective use of technical writers according to Google's findings.

> [!answer]- Show Answer
> This is **not effective** according to Google's experience. The chapter identifies several problems:
>
> 1. **Teams can write for themselves**: Engineering teams can produce documentation for their own team members perfectly well. The feedback loop is immediate, domain knowledge is shared, and perceived needs are obvious.
>
> 2. **Technical writers add value across boundaries**: Where teams struggle is writing for **other audiences** — because it's hard to write to an audience with different assumptions. Technical writers are better able to stand in as someone unfamiliar with the domain and challenge the team's assumptions.
>
> 3. **Perverse incentive**: Assigning writers to individual teams creates the incentive "become an important project and your engineers won't need to write documents." This **discourages engineers from writing**, which is the opposite of what you want.
>
> 4. **Doesn't scale**: Technical writers are a limited resource. Supporting single teams isn't the best use of this specialized resource.
>
> **Better approach**: Focus technical writers on **cross-API documentation** — documents that span project boundaries, like developer guides, where domain expertise from multiple teams must be synthesized into coherent documentation for external audiences.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Docs like code | **Source control, owners, reviews, bugs, freshness** |
> | Five doc types | **Reference, design, tutorial, conceptual, landing page** |
> | One purpose per doc | **Don't mix types; monolithic docs fail everyone** |
> | Seekers vs Stumblers | **Consistency vs clarity** |
> | GooWiki failure | **No owners → stale; moved to source control** |
> | Function comments | **Indicative verb; single prose, not boilerplate** |
> | Three reviews | **Technical (accuracy), audience (clarity), writing (consistency)** |
> | Freshness dates | **Metadata + email reminders; low-cost periodic review** |
> | Technical writers | **Cross-API docs, not single teams; don't discourage engineers from writing** |
> | Docs vs testing parallel | **Both need workflow integration; docs are harder (subjective, async, no "test failure")** |

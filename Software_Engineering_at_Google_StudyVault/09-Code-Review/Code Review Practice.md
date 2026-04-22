---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, code review, LGTM, ownership, knowledge sharing
---

# Code Review Practice (10 questions)

#practice #code-review #processes

## Related Concepts
- [[Code Review]]
- [[Style Guides and Rules]]
- [[Knowledge Sharing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three approvals | **LGTM, owner approval, readability approval** |
> | Primary benefit | **Comprehension/maintainability, not just correctness** |
> | Small changes | **~200 lines; enables single-reviewer model** |
> | PTAL | **"Please take another look"** |
> | Code is a liability | **Prefer reuse over writing from scratch** |

---

## Question 1 - Three Approval Bits [recall]
> What are the three types of approval required for a code change at Google?

> [!answer]- Show Answer
> 1. **LGTM** (correctness + comprehension): another engineer confirms the code works and is understandable
> 2. **Owner approval**: a code owner for the relevant directory confirms the change is appropriate for their part of the codebase
> 3. **Readability approval**: someone with language readability certification confirms the code follows style and best practices
>
> Often one person holds all three roles, speeding up the process.

---

## Question 2 - Primary Benefit [recall]
> What does Google consider the primary benefit of code review, and why is it not "catching bugs"?

> [!answer]- Show Answer
> The primary benefit is ensuring code is **understandable and maintainable over time** as the codebase scales — not just checking correctness. While correctness matters, code will be read far more times than it is written. As tooling for static analysis and automated testing becomes stronger, many correctness checks are performed automatically. The more important human review focuses on **comprehension** and **long-term sustainability**.

---

## Question 3 - Six Benefits [recall]
> List the six benefits of code review described in Chapter 9.

> [!answer]- Show Answer
> 1. **Code correctness** — catching defects early (shift-left)
> 2. **Comprehension** — unbiased check on whether code is understandable
> 3. **Code consistency** — enforcing codebase standards via readability review
> 4. **Psychological and cultural benefits** — collective ownership, validation, process as "bad cop"
> 5. **Knowledge sharing** — domain knowledge transfer between reviewer and author
> 6. **Historical record** — every change is archived for future code archeology

---

## Question 4 - Small Changes [recall]
> What is the recommended size for a code change at Google and why?

> [!answer]- Show Answer
> About **200 lines of code**. Benefits:
> - Easier for reviewers to digest and review quickly (most reviewed within a day)
> - Faster feedback cycle, reducing downtime for authors
> - Easier to identify the source of bugs
> - Easier to roll back if something goes wrong
> - Enables the **single-reviewer model** that makes code review scale at Google (~35% of changes are to a single file)

---

## Question 5 - OWNERS Files [recall]
> How do OWNERS files work in Google's codebase?

> [!answer]- Show Answer
> **OWNERS files** list usernames of people with ownership responsibilities for a directory and its children. They are **hierarchically additive**: a file is owned by the union of all OWNERS files above it in the directory tree. No central authority is needed — a new OWNERS file is sufficient for new projects. Root OWNERS can act as **global approvers** for large-scale changes. OWNERS files also serve as **documentation**, making it easy to find who is responsible for a given piece of code.

---

## Question 6 - Readability vs LGTM [recall]
> What is the difference between an LGTM approval and a readability approval?

> [!answer]- Show Answer
> **LGTM** indicates that the code is **correct** (does what it claims) and **comprehensible** (understandable to others). It can be given by any engineer.
>
> **Readability approval** ensures code follows the language's **style and best practices** and is consistent with the codebase. It can only be granted by engineers who have completed **readability training** for that specific programming language. This separation ensures code is both functionally sound and maintainable.

---

## Question 7 - Handling Disagreement [application]
> During a code review, you disagree with a reviewer's comment that suggests rewriting your approach. The reviewer's suggestion is equally valid but not objectively better. How should you handle this according to Google's best practices?

> [!answer]- Show Answer
> According to Google's best practices:
> - Reviewers should **defer to authors** on their particular approach when multiple approaches are equally valid
> - If you disagree, **offer an alternative** and ask the reviewer to **PTAL** (please take another look)
> - Treat each comment as a **TODO item** — it should be addressed, even if not accepted
> - Remember that "you are not your code" — keep discussion professional
> - In general, engineers are encouraged to **approve changes that improve the codebase** rather than wait for a "perfect" solution
> - If the reviewer's suggestion doesn't improve comprehension or functionality, you are within your rights to explain your reasoning and keep your approach.

---

## Question 8 - Bug Fix Scope [application]
> You're fixing a bug and notice that the surrounding code has poor naming conventions and could use refactoring. What should you do?

> [!answer]- Show Answer
> **Focus solely on the bug fix.** According to Chapter 9, you should resist the temptation to address other issues in a bug fix because:
> 1. It increases the **size** of the code review, making it harder to review
> 2. It makes **regression testing** more difficult
> 3. It makes it harder for others to **roll back** your change if needed
>
> Address the naming and refactoring in a **separate change**. The bug fix should include the fix itself and (usually) **updated tests** to catch the error that occurred.

---

## Question 9 - Code Review Scaling [analysis]
> Google requires code review for nearly every change in a codebase of billions of lines with tens of thousands of engineers. Analyze what specific design decisions make this process scalable.

> [!answer]- Show Answer
> Several interconnected design decisions enable code review to scale at Google:
>
> 1. **Small changes (~200 lines)**: Keep reviews digestible, enabling single-reviewer model
> 2. **Single reviewer**: Most reviews need only one person who often holds all three approval bits (LGTM + ownership + readability), preventing bottlenecks
> 3. **Flexible approval model**: The three bits can be combined in any configuration — a tech lead with readability needs only one LGTM; an intern routes through more reviewers
> 4. **Distributed ownership**: Hierarchical OWNERS files enable local authority without central registration
> 5. **Automation**: Presubmit checks (static analysis, formatters, linters) handle mechanical verification, freeing reviewers for higher-level concerns
> 6. **24-hour feedback expectation**: Prevents reviews from stalling
> 7. **Readability certification**: A pool of trained reviewers across the company ensures style consistency without requiring every team to maintain their own standards
>
> The key insight is that each decision **reduces friction** at the cost of some formality, optimizing for throughput at scale.

---

## Question 10 - Psychological Benefits [analysis]
> The chapter describes code review as acting as a "bad cop." Analyze how the code review process provides psychological benefits beyond technical ones, and explain why these matter at scale.

> [!answer]- Show Answer
> Code review provides several psychological benefits:
>
> **Depersonalizing criticism**: The process itself is the "bad cop" — since the process *requires* critical review (Google calls its tool "Critique"), reviewers are just doing their job. This buffers potentially emotional interactions and provides a **gentler introduction** for new engineers to team expectations.
>
> **Collective ownership**: Code review reinforces that code belongs to the **team, not individuals**. Without it, engineers naturally gravitate toward personal style. The process forces compromise for the collective good.
>
> **Validation and recognition**: Even capable engineers suffer from **imposter syndrome**. Code review provides validation — the exchange of ideas and the LGTM act as recognition of one's work.
>
> **Self-discipline**: Knowing code will be reviewed forces engineers to **clean up loose ends** before submitting. The "little moment of reflection" before sending a change prevents shortcuts.
>
> **At scale, these matter because**: With tens of thousands of engineers across diverse teams, you cannot rely on personal relationships or informal norms to maintain code quality culture. The code review process becomes a **scalable cultural mechanism** — it consistently teaches norms, distributes knowledge, and maintains psychological safety across the entire organization.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three approvals | **LGTM + owner + readability** |
> | Primary benefit | **Comprehension/maintainability over time** |
> | Six benefits | **Correctness, comprehension, consistency, psychology, knowledge sharing, history** |
> | Small changes | **~200 lines; single reviewer; fast feedback** |
> | OWNERS files | **Hierarchical, additive, no central authority** |
> | PTAL | **"Please take another look" — for disagreements** |
> | Bug fix scope | **Focus only on the bug; separate change for refactoring** |
> | 24-hour rule | **Initial feedback within 24 working hours** |
> | Presubmit | **Automated checks before reviewer sees the change** |
> | "Bad cop" | **Process depersonalizes criticism; reviewers do their job** |

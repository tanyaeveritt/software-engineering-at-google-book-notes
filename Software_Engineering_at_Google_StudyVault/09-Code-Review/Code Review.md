---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: code review, LGTM, readability, ownership, correctness, knowledge sharing
---

# Code Review (Importance: ★★★)

#code-review #processes

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Three Approvals | LGTM (correctness), Owner approval, Readability approval |
| Six Benefits | Correctness, comprehension, consistency, psychology/culture, knowledge sharing, historical record |
| Best Practices | Be polite, write small changes, good descriptions, minimize reviewers, automate |
| Types of Reviews | Greenfield, behavioral changes, bug fixes, refactorings/LSC |
| Code Is a Liability | New code = maintenance burden; prefer reuse over writing from scratch |
| OWNERS Files | Hierarchical directory-based ownership; enable distributed stewardship |

## Code Review Flow at Google
Code reviews are **precommit** — they happen before code is committed to the codebase. The flow:
1. Author creates a **snapshot** (patch + description) uploaded to Critique (the code review tool)
2. Author self-reviews, then **mails** the change to reviewers
3. Reviewers post **comments** on the diff (some require resolution, some are FYI)
4. Author modifies and uploads new snapshots; steps 3-4 repeat
5. Reviewer marks the change as **LGTM** ("looks good to me")
6. Author commits the change after resolving all comments and obtaining approval

## Three Types of Approval
Every change at Google requires three "approval bits" (often held by one person):
- **LGTM** (correctness + comprehension): another engineer confirms the code works and is understandable
- **Owner approval**: a code owner for that directory confirms the change is appropriate for their part of the codebase (via hierarchical OWNERS files)
- **Readability approval**: someone with language readability certification confirms the code follows style and best practices

A tech lead who owns the project and has readability needs only an LGTM from one other engineer. An intern needs all three from separate people. This **flexibility** is key to scaling.

## Code Review Benefits
Six benefits of code review at Google:

### 1. Code Correctness
Catching defects early costs less to fix (shift-left). But checking correctness is **not the primary benefit** at Google — comprehensibility over time matters more. Authors are given **deference** to their approach; reviewers suggest alternatives only if they improve comprehension or functionality.

### 2. Comprehension of Code
The first opportunity for **unbiased feedback** on whether code is understandable. Code is read many more times than written. Treat comprehension questions as valid — "the customer is always right." LGTM means the code does what it says **and** is understandable.

### 3. Code Consistency
Code must conform to standards for maintainability. **Readability approval** (separate from LGTM) is granted only by engineers who completed readability training for that language. Consistent code is easier to refactor with automated tools.

### 4. Psychological and Cultural Benefits
- Reinforces that code belongs to the **collective**, not individuals
- Acts as **"bad cop"** — the process requires criticism, so reviewers aren't personally blamed
- Provides **validation** against imposter syndrome
- Forces authors to **resolve loose ends** before submitting ("all ducks in a row")

### 5. Knowledge Sharing
- Reviewers impart **domain knowledge** to authors (suggestions, new techniques, FYI comments)
- Bidirectional learning: both authors and reviewers learn
- Scales across **time zones and projects** — engineers often "meet" through code reviews
- New patterns are advertised through code review and large-scale changes

### 6. Historical Record
Every change is archived. Engineers can inspect when a pattern was introduced and pull up the original review. This **code archeology** benefits far more engineers than the original participants.

## Code Review Best Practices

### Be Polite and Professional
- Defer to author's approach unless it's deficient; don't propose alternatives based on personal opinion
- Feedback expected **within 24 working hours**; avoid piecemeal feedback
- Authors: "You are not your code" — be receptive; treat comments as TODO items
- Offer alternatives with **PTAL** (please take another look) when disagreeing

### Write Small Changes
- ~200 lines of code ideal; most changes reviewed within about a day
- ~35% of Google changes are to a single file
- Small changes: easier to review, faster feedback, easier to find bugs, easier to roll back
- Most reviews at Google involve **only one reviewer** — this only works because changes are small

### Write Good Change Descriptions
- First line is the **summary** (used in email subjects, Code Search history)
- Describe **what** and **why**, not just "Bug fix"
- Update descriptions if decisions change during review; this is a **historical record**

### Keep Reviewers to a Minimum
- Most reviews need **precisely one reviewer** who handles all three approval bits
- Diminishing returns from additional reviewers
- If multiple reviewers needed, each should focus on **different aspects**

### Automate Where Possible
- **Presubmit** processes detect problems before code reaches the reviewer
- Static analysis, linters, formatters run automatically
- Frees reviewers to focus on meaningful concerns, not formatting

## Types of Code Reviews
- **Greenfield**: Entirely new code; most important to evaluate sustainability. Requires design review beforehand, full testing, proper OWNERS file.
- **Behavioral changes/improvements/optimizations**: Most common type. Must include test revisions. Deletions are among the best modifications for code health.
- **Bug fixes and rollbacks**: Focus solely on the bug; resist temptation to address other issues. Rollbacks revert to known state and can be created in seconds but still require review.
- **Refactorings and large-scale changes (LSC)**: Often machine-generated. Reviewed by designated approvers or individual engineers. Reviewers should limit comments to their specific code, not the underlying tool.

## Ownership and OWNERS Files
- **OWNERS files** list usernames of people with ownership responsibilities for a directory and its children
- Hierarchical and additive: a file is owned by the union of all OWNERS files above it in the directory tree
- No central authority for new ownership — a new OWNERS file is sufficient
- Ownership conveys approval rights but also responsibilities (understanding the code or knowing who does)

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Three approval bits" | **LGTM + owner approval + readability approval** |
| "Primary benefit of code review at Google" | **Comprehension and maintainability over time, not just correctness** |
| "LGTM means" | **Code does what it says AND is understandable** |
| "Readability approval" | **Granted by engineers with language readability training** |
| "Small changes" | **~200 lines; enables single-reviewer model** |
| "Feedback timeline" | **Within 24 working hours** |
| "Code is a liability" | **New code = maintenance burden; prefer reuse** |
| "PTAL" | **"Please take another look" — used when author disagrees with reviewer** |
| "Presubmit" | **Automated checks before change reaches reviewer** |

## Related Notes
- [[Style Guides and Rules]]
- [[Documentation]]
- [[Knowledge Sharing]]
- [[Testing Overview]]
- [[Measuring Productivity]]
- [[Leading at Scale]]

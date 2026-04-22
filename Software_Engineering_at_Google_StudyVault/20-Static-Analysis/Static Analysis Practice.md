---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, static analysis, Tricorder, Error Prone, false positives, developer happiness
---

# Static Analysis Practice (10 questions)

#practice #static-analysis #tools

## Related Concepts
- [[Static Analysis]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three key lessons | Developer happiness, core workflow, empower contributions |
> | Effective false positive | Developer took no positive action |
> | Tricorder criteria | Understandable, actionable, <10% false positives, significant impact |
> | Primary integration | Code review (Critique) |
> | No compiler warnings | Either error or suppress; devs ignore warnings |
> | Project-level customization | Not user-level; user-level hid bugs |

---

## Question 1 - Three Key Lessons [recall]
> State the three key lessons Google learned about making static analysis work.

> [!answer]- Show Answer
> 1. **Focus on developer happiness**: deploy only tools with low false-positive rates; actively solicit and act on feedback; track the "effective false positive" rate (developer-perceived, not technically measured). User trust is essential.
> 2. **Make static analysis part of the core developer workflow**: primary integration point is code review (Critique). Developers are already in a change mindset, there's time for analysis while awaiting reviewers, and peer pressure drives fixes. Code review is the "sweet spot" for analysis results.
> 3. **Empower users to contribute**: domain experts write analyzers; developers who find bugs create checks to prevent recurrence; simple APIs like Refaster lower the barrier; build an ecosystem easy to plug into.

---

## Question 2 - Effective False Positive [recall]
> Define "effective false positive" and explain how it differs from a traditional false positive.

> [!answer]- Show Answer
> A traditional **false positive** is when a tool incorrectly flags code as having an issue it doesn't have. An **effective false positive** is when a developer takes **no positive action** after seeing the issue — regardless of whether the finding is technically correct. Examples: (1) a technically correct warning that is confusingly worded, so the developer ignores it = effective false positive. (2) A technically incorrect warning, but the developer happily makes the fix to improve readability = NOT an effective false positive. (3) A real bug found, but the developer doesn't understand it and takes no action = effective false positive. The key insight is that **perception matters as much as technical accuracy**.

---

## Question 3 - Tricorder Criteria [recall]
> What four criteria must a new Tricorder check meet before deployment?

> [!answer]- Show Answer
> 1. **Be understandable**: any engineer can understand the output.
> 2. **Be actionable and easy to fix**: the result includes guidance on how to fix the issue (may require more effort than a compiler check).
> 3. **Produce less than 10% effective false positives**: developers should find the check useful at least 90% of the time.
> 4. **Have the potential for significant impact on code quality**: issues might not affect correctness, but developers should take them seriously and deliberately choose to fix them.

---

## Question 4 - No Compiler Warnings [recall]
> Explain Google's policy on compiler warnings and the reasoning behind it.

> [!answer]- Show Answer
> Google aims to **never issue compiler warnings**. A check is either enabled as a **compiler error** (breaking the build) or not shown in compiler output at all. The reasoning: developers **ignore** compiler warnings. Both Java and C++ compilers at Google are configured to suppress warnings. Go takes this further — things like unused variables or package imports that other languages treat as warnings are **compilation errors** in Go. Checks that can't break the build are either suppressed or routed to code review (via Tricorder). This ensures every compiler message is actionable.

---

## Question 5 - Project vs User Customization [recall]
> Why did Google remove user-level customization for Tricorder and switch to project-level only?

> [!answer]- Show Answer
> Early Tricorder/Critique integration allowed users to set confidence levels and suppress results from specific analyzers. When this was removed, complaints poured in. Instead of restoring customization, the team asked users **why** they were annoyed and discovered: (1) the C++ linter ran on Objective-C files producing useless results — a bug; (2) the HTML linter had an extremely high false-positive rate — it was disabled entirely. **User customization was hiding bugs and suppressing feedback.** Project-level customization ensures all team members have a consistent view, preventing situations where one developer fixes issues while another introduces them.

---

## Question 6 - Feedback Loop [recall]
> Describe the feedback mechanisms between Tricorder users and analyzer writers.

> [!answer]- Show Answer
> Two primary mechanisms: (1) **"Not useful" button** on each analysis result — files a bug against the analyzer writer with prepopulated information about the finding. (2) **"Please fix" button** — reviewers click this to create unresolved comments asking authors to address findings. The Tricorder team tracks the ratio of "Not useful" clicks to "Please fix" clicks for each analyzer. Analyzers with high "Not useful" rates that don't improve are **disabled**. Example: an Error Prone check generated weekly bug reports because the message was confusing. After updating the diagnostic text to clearly state "this function accepts only %s," the reports stopped. This virtuous feedback cycle built user trust and improved tools over time.

---

## Question 7 - Deploy a New Compiler Check [application]
> Outline the process Google follows to enable a new static analysis check as a compiler error.

> [!answer]- Show Answer
> 1. The check must meet three criteria: **actionable and easy to fix** (with a suggested fix), **no effective false positives** (never stops the build for correct code), and **reports only correctness issues** (not style).
> 2. **Clean up all existing instances** in the codebase first — can't break existing projects just because the compiler evolved. This is done using cluster-based tools (MapReduce-like) that run the compiler over the entire codebase and generate automated fixes.
> 3. Prepare and test a pending code change that **applies fixes across the entire codebase**.
> 4. Commit the codebase-wide fix.
> 5. **Enable the check as a compiler error** so no new instances can be introduced.
> 6. Going forward, CI catches violations post-commit, and presubmit checks catch them pre-commit.
> The bar for deployment is high because the value must justify fixing all existing instances.

---

## Question 8 - Code Review Sweet Spot [application]
> Your team debates whether to integrate static analysis into the IDE, the code review tool, or both. Using Google's experience, argue for prioritizing code review integration.

> [!answer]- Show Answer
> Code review is the **sweet spot** for static analysis because: (1) Developers are already in a **change mindset** — improvements are less disruptive. (2) There's **natural wait time** while context-switching after sending for review, allowing analyses to run. (3) **Peer pressure** from reviewers to address warnings drives actual fixes. (4) Analysis helps reviewers **scale** by highlighting common issues automatically. (5) Analysis sees the **entire change context**, which some analyses need for accuracy (e.g., dead code detection requires knowing all callsites). (6) Authors must convince reviewers to ignore warnings, adding accountability. IDE integration is nice but has drawbacks: requires <100ms latency, inconsistency across multiple IDEs, and IDEs rise and fall in popularity. Code review integration reaches all developers uniformly.

---

## Question 9 - Refaster for Domain Experts [analysis]
> The chapter mentions Refaster as a way to empower non-experts to write analyzers. Analyze how this design choice supports the "empower users to contribute" lesson and what risks it might introduce.

> [!answer]- Show Answer
> **Refaster** lets engineers write analyzers by specifying **pre-code and post-code snippets** (before/after transformation). This dramatically lowers the barrier: domain experts don't need to understand AST manipulation, compiler internals, or analysis frameworks. **Benefits**: (1) Scales analysis coverage by leveraging domain knowledge across the company; (2) Bug discoverers can immediately create prevention checks; (3) Google has 100+ Tricorder analyzers, most contributed from outside the Tricorder team. **Risks**: (1) Non-experts may create checks with high false-positive rates — mitigated by the <10% effective false-positive criterion; (2) Too many checks could create "analysis fatigue" — mitigated by tracking "Not useful" rates and disabling underperformers; (3) Checks might conflict or overlap — mitigated by the Tricorder platform's review and monitoring processes. The design puts **guard rails** around an open contribution model.

---

## Question 10 - Static Analysis at Scale [analysis]
> Google found that "user customization resulted in hidden bugs and suppressing feedback." Analyze the systemic consequences if Google had kept user-level customization.

> [!answer]- Show Answer
> **Systemic consequences**: (1) **Broken feedback loop**: analyzer writers would never learn about bugs in their tools because users silently suppress them instead of reporting. The C++ linter's Objective-C bug and the HTML linter's high false-positive rate would persist indefinitely. (2) **Inconsistent code quality**: within the same project, one developer might see and fix an issue while another has it suppressed, creating contradictory coding patterns. (3) **False sense of quality**: teams might believe code is clean (no analysis warnings shown) when issues are merely hidden. (4) **Undermined peer review**: reviewers couldn't rely on analysis results if authors could selectively suppress findings before review. (5) **Stagnant tool improvement**: without pressure from user complaints, there's no incentive to improve analysis quality. Project-level customization preserves the feedback loop and team consistency while still allowing domain-specific needs.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three lessons | Developer happiness, core workflow integration, empower contributions |
> | Effective false positive | Developer took no positive action (perception matters) |
> | Tricorder criteria | Understandable, actionable, <10% FP, significant impact |
> | No compiler warnings | Error or suppress; developers ignore warnings |
> | Project-level customization | User-level hid bugs; project-level ensures consistency |
> | Feedback loop | "Not useful" + "Please fix" buttons; disable bad analyzers |
> | New compiler check process | Clean all instances -> commit fix -> enable as error |
> | Code review sweet spot | Change mindset, natural wait, peer pressure, full context |
> | Refaster | Pre/post code snippets; lowers barrier; guard rails prevent abuse |

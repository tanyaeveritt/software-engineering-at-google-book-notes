---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, deprecation, advisory, compulsory, migration, Hyrum-Law
---

# Deprecation Practice (9 questions)

#practice #deprecation #processes

## Related Concepts
- [[Deprecation]]
- [[Testing Overview]]
- [[Software Engineering Fundamentals]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Code is a... | **Liability**, not an asset |
> | Advisory deprecation | No deadline; "hope is not a strategy" |
> | Compulsory deprecation | Deadline + enforcement + **dedicated team** |
> | Warning properties | **Actionable** and **relevant** |
> | Process owners | Without them, deprecation **won't progress** |

---

## Question 1 - Code as Liability [recall]
> Google asserts that "code is a liability, not an asset." Explain what this means and how it relates to deprecation.

> [!answer]- Show Answer
> Code itself doesn't bring value—it is the **functionality** that it provides that brings value. The code that implements functionality is simply a means to that end. Code carries ongoing costs: operational resources, maintenance as ecosystems evolve, esoteric expertise, and cognitive overhead. If you could get the same functionality from one line of maintainable code as from 10,000 lines of spaghetti code, you'd prefer the former. Deprecation directly follows from this: instead of maximizing code produced, maximize **functionality per unit of code** by removing excess code and obsolete systems.

---

## Question 2 - Advisory vs. Compulsory [recall]
> What are the two types of deprecation, and how do they differ in approach and effectiveness?

> [!answer]- Show Answer
> **Advisory deprecation**: no deadline, no enforcement, no dedicated resources. The team hopes clients will eventually migrate. This is essentially aspirational—"hope is not a strategy." It can slightly reduce new uses of the old system but **rarely leads to active migration**. Effective only when the new system offers transformative (not incremental) benefits.
> **Compulsory deprecation**: comes with a deadline for removal, an enforcement mechanism, and is staffed by a dedicated migration team. The team is empowered to break noncompliant users after sufficient warning. This is the only approach that reliably results in complete removal. The dedicated team centralizes expertise and develops tools that can be used across the organization.

---

## Question 3 - Hyrum's Law and Deprecation [recall]
> How does Hyrum's Law make deprecation particularly challenging?

> [!answer]- Show Answer
> Hyrum's Law states: the more users of a system, the higher the probability they are using it in **unexpected and unforeseen ways**. Their usage "happens to work" rather than being "guaranteed to work" by the API contract. In the context of deprecation, removing a system is the **ultimate change**—not just changing behavior but removing it completely. This shakes loose unexpected dependents. Additionally, the replacement system is necessarily **different** (otherwise it wouldn't provide benefits), so a one-to-one match is rare and every use must be individually evaluated.

---

## Question 4 - Deprecation Warnings [recall]
> What two properties must deprecation warnings have, and what happens when warnings are poorly implemented?

> [!answer]- Show Answer
> Deprecation warnings must be:
> 1. **Actionable**: the user can perform a concrete next step—not just "this is deprecated" but "replace X with Y" or specific migration steps an average engineer can follow
> 2. **Relevant**: surfaced at the right time—when the engineer is writing code that uses the deprecated function, not weeks after it's checked in
> Poorly implemented warnings lead to **alert fatigue**: transitive warnings accumulate (library A → B → C, warning surfaces when building A), overwhelming users until they ignore all warnings entirely. Google uses tools like ErrorProne and Tricorder to surface warnings in **targeted ways**, limiting intrusive warnings to compulsory deprecations.

---

## Question 5 - Planned Outages [recall]
> How does Google discover hidden dependencies on systems being deprecated?

> [!answer]- Show Answer
> Google uses several techniques:
> 1. **Static analysis**: Code Search and Kythe identify code-level dependencies
> 2. **Planned outages of increasing duration**: the deprecating team announces scheduled outages in the months/weeks before turndown (similar to DiRT exercises), discovering runtime dependencies by **seeing what breaks**
> 3. **Symbol renaming**: Google occasionally changes names of implementation-only symbols to see which users are depending on them unknowingly
> 4. **Runtime sampling/logging**: discovers dynamic dependencies that static analysis misses
> 5. **Global test suite**: serves as an oracle to determine whether all references to old symbols have been removed

---

## Question 6 - Unfunded Mandate [application]
> Your infrastructure team announces a compulsory deprecation of a widely-used internal library with a 6-month deadline, but provides no migration support. Customer teams are ignoring the deadline. What's going wrong and how should it be fixed?

> [!answer]- Show Answer
> This is the **unfunded mandate** problem Google warns about. Compulsory deprecation without staffing comes across as mean-spirited: customer teams see it as pushing aside their own priorities to do work just to keep services running, creating friction. Google strongly advocates that compulsory deprecations be **actively staffed by a specialized team** through completion.
> The fix: the infrastructure team should **centralize migration expertise** in a dedicated team. This team develops migration tools, gains experience with common patterns, and **does the migration work** (or provides substantial hands-on assistance) using tools like LSC (Large-Scale Changes). The team should also set incremental milestones, use Tricorder to prevent backsliding, and provide actionable guidance at every step.

---

## Question 7 - Design for Deprecation [application]
> A team is designing a new internal service. Using Google's guidance, what questions should they ask to make future deprecation easier?

> [!answer]- Show Answer
> Key questions:
> 1. **"How easy will it be for consumers to migrate from my product to a potential replacement?"** This encourages designing clean, well-documented APIs with clear contracts, making migration paths possible.
> 2. **"How can I replace parts of my system incrementally?"** This encourages modular architecture where components can be swapped independently rather than requiring wholesale replacement.
> Additional considerations: use well-known interfaces and standard dependency management, minimize implicit contracts (Hyrum's Law), keep the dependency surface small, and commit to long-term support from the start—the decision to support a project long-term is made when you **first decide to build it**.
> The analogy: nuclear power plants are designed with eventual decommissioning in mind, including pre-allocated funds. Software should be similarly thoughtful.

---

## Question 8 - Emotional Attachment [analysis]
> Google reports engineers sometimes resist deprecation with "I like this code!" Why is this response understandable but ultimately self-defeating? What organizational mechanism helps address it?

> [!answer]- Show Answer
> It's **understandable** because engineers invest significant time and creativity building systems—they feel ownership and pride. However, it's **self-defeating** because if a system is obsolete, it has a **net cost** on the organization: maintenance overhead, cognitive burden, interoperability complexity with replacement systems, and drag on new system evolution.
> Google's organizational mechanism: ensuring the source code repository is **historically searchable**. Even code that has been removed can be found again via version control history. This addresses the emotional concern—the work isn't truly "gone," just retired from active duty. Engineers can still reference, learn from, and take pride in their previous work without imposing ongoing costs on the organization.

---

## Question 9 - Deprecation at Scale [analysis]
> Google observes that "migrating to entirely new systems is extremely expensive, and the costs are frequently underestimated." Given this, when is wholesale replacement justified versus incremental in-place refactoring?

> [!answer]- Show Answer
> **Incremental in-place refactoring** is generally preferred because it:
> - Keeps existing systems running while delivering value
> - Breaks the work into smaller, manageable chunks with incremental benefits
> - Avoids the enormous coordination cost of wholesale replacement
> - Reduces the risk of underestimating migration costs
>
> **Wholesale replacement** may be justified when:
> - The old system's architecture fundamentally cannot support needed functionality
> - The maintenance cost of the old system is demonstrably high and growing
> - A replacement exists with **comparable functionality** (not just theoretical)
> - The organization is willing to staff a **dedicated team** and commit resources through completion
> - There is clear evidence (via research techniques) that deprecation benefits outweigh costs
>
> Key insight: even with wholesale replacement, the costs include not just building the new system but the entire **migration and removal** process. These "turndown costs" are frequently overlooked in planning. Evolving a system in situ, while maintaining backward compatibility, is usually the cheaper path when all costs are included.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Code is a liability | Functionality is the asset; **code is the cost** |
> | Advisory | No deadline; "hope is not a strategy" |
> | Compulsory | Deadline + enforcement + **dedicated team** |
> | Warning: actionable | User can take **concrete next step** |
> | Warning: relevant | Surfaced at the **right time** |
> | Alert fatigue | Transitive warnings → users ignore all |
> | Hyrum's Law | More users = more unexpected usage = **harder removal** |
> | Planned outages | Discover hidden runtime dependencies |
> | Backsliding prevention | Tricorder + visibility whitelists |
> | Incremental > wholesale | In-place refactoring usually **cheaper** |
> | Process owners | Removal as **primary goal**, not side project |
> | Design for deprecation | Ask: how easy to migrate? How to replace incrementally? |

---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: deprecation, advisory, compulsory, migration, Hyrum-Law, code-liability
---

# Deprecation (Importance: ★★☆)

#deprecation #processes

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Core Premise | **Code is a liability**, not an asset; functionality is the asset |
| Why Deprecate | Reduce maintenance costs, cognitive burden, and ecosystem complexity |
| Why It's Hard | Hyrum's Law, emotional attachment, political difficulty, unfunded mandates |
| Advisory Deprecation | No deadline, no enforcement; "hope is not a strategy" |
| Compulsory Deprecation | Has a deadline, enforcement mechanism, and dedicated team |
| Deprecation Warnings | Must be **actionable** and **relevant**; avoid alert fatigue |
| Process Owners | Without explicit owners, deprecation won't make progress |
| Deprecation During Design | Plan for eventual removal when building systems |
| Backsliding Prevention | Use static analysis (Tricorder) and visibility whitelists to block new uses |
| Incremental Milestones | Celebrate subcomponent removals, not just final shutdown |

## Why Deprecate?
The fundamental premise: **code is a liability, not an asset**. It's the functionality that provides value.
- Focus on **functionality per unit of code**—maximize by removing excess code and unused systems
- Having two systems for the same function creates interoperability costs, dependency tangles, and impedes evolution of the newer system
- Ongoing costs: operational resources, updating codebase as ecosystems evolve, esoteric expertise for old systems
- Old ≠ obsolete (e.g., LaTeX has been refined over decades and is not obsolete)
- Organizations have a **limit** on simultaneous deprecation work—choose projects carefully and commit to finishing

## Why Is Deprecation So Hard?
- **Hyrum's Law**: the more users, the more unexpected usage patterns. Removing a system shakes loose **unexpected dependents** whose usage "happens to work" rather than "guaranteed to work"
- **Functional differences**: the new system is different (otherwise why migrate?), so a 1-to-1 match is rare; every use must be individually evaluated
- **Emotional attachment**: "I like this code!"—engineers resist tearing down things they spent years building. Mitigated by making the repository **historically searchable**
- **Google joke**: "There are two ways of doing things: the one that's deprecated, and the one that's not-yet-ready"
- **Political difficulty**: staffing deprecation costs real money; costs of doing nothing are **not readily observable**; deprecation competes with feature development
- **Incremental > wholesale replacement**: migrating to entirely new systems is extremely expensive and frequently underestimated; in-place refactoring is often cheaper

## Deprecation During Design
Plan for deprecation **when building** systems. Questions to ask:
- How easy will it be for consumers to migrate to a replacement?
- How can I replace parts of the system incrementally?
- Analogy: nuclear power plants are designed with **decommissioning** in mind (including funding allocation)
- Software culture (including Google's) incentivizes building/shipping new products, not planning for removal
- Key decision: whether to support a project long-term is made **when you first decide to build it**

## Types of Deprecation

### Advisory Deprecation
No deadline, no enforcement, no dedicated resources. Also called "aspirational deprecation."
- Useful for advertising a new system and encouraging early adopters
- **"Hope is not a strategy"**: advisory deprecation rarely leads to active migration
- Existing uses create **conceptual pull** toward the old system—new uses follow the majority regardless of deprecation notices
- Effective only when new system offers **transformative** (not incremental) benefits

### Compulsory Deprecation
Comes with a **deadline** for removal of the obsolete system.
- Best scaling strategy: **centralize expertise** in a dedicated migration team (the team removing the old system)
- Schedule needs an **enforcement mechanism**—the team must be empowered to break noncompliant users after sufficient warning
- **Without staffing**, compulsory deprecation feels like an "unfunded mandate" to customer teams
- **Political challenges**: what if the last remaining user is critical infrastructure everyone depends on?
- Discovery techniques: Google announces **planned outages of increasing duration** (like DiRT exercises) to discover hidden dependencies
- Also: rename implementation-only symbols to see who depends on them unknowingly

## Deprecation Warnings
Must have two properties:
- **Actionable**: the user can perform a concrete next step (not just "this is deprecated" but "replace X with Y")
- **Relevant**: surfaced at the right time (when writing code that uses deprecated function, not weeks later)
- **Alert fatigue**: transitive warnings accumulate and overwhelm users, causing them to ignore all warnings
- Google uses **ErrorProne** and **clang-tidy** to surface warnings in targeted ways
- Limit intrusive warnings to **compulsory** deprecations with active migration teams
- Tricorder surfaces new uses at **review time** with the deprecated system owner's messaging

## Managing the Deprecation Process

### Process Owners
Without explicit owners, deprecation **never makes meaningful progress**.
- Abandoned projects (no clear owner/maintainer) are especially problematic—still dependencies of critical projects, hoping to fade away
- Deprecation teams should have **removal as their primary goal**, not a side project
- Good use of **20% time** and provides exposure to other parts of the codebase

### Milestones
Unlike building (clear milestones), deprecation can feel like no progress until the final shutdown.
- Set **incremental milestones**: deleting key subcomponents, migrating major user groups
- Celebrate incremental achievements just as you would for launching a new product
- The final removal is often the **least noticed** step (by then, no users remain)

### Deprecation Tooling
Three categories:
- **Discovery**: Code Search, Kythe for static analysis; logging/runtime sampling for dynamic dependencies; global test suite as an oracle for reference removal
- **Migration**: LSC (Large-Scale Changes) process and tooling for updating the codebase
- **Backsliding prevention**: Tricorder static analysis warns about new uses; `@deprecated` annotations surface at review time; build system **visibility whitelists** prevent new dependencies; automated pruning of whitelists as systems migrate

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Code is a liability" | Functionality is the asset; **code is the means** |
| Advisory deprecation | No deadline, no enforcement; **hope is not a strategy** |
| Compulsory deprecation | Deadline + enforcement + **dedicated team** |
| Deprecation warning properties | Must be **actionable** and **relevant** |
| Alert fatigue | Transitive warnings accumulate → users **ignore them all** |
| Hyrum's Law + deprecation | More users = more unexpected usage = **harder to remove** |
| Discovery tools | Code Search, Kythe, test suite as oracle, **planned outages** |
| Backsliding prevention | Tricorder, @deprecated annotation, **visibility whitelists** |
| Process owners | Without explicit owners, deprecation **won't progress** |
| Incremental vs. wholesale | In-place refactoring is often **cheaper** than full replacement |

## Related Notes
- [[Testing Overview]]
- [[Larger Testing]]
- [[Software Engineering Fundamentals]]
- [[Code Review]]
- [[Continuous Integration]]

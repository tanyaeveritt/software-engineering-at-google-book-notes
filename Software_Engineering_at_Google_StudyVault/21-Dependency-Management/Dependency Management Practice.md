---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, dependency management, SemVer, diamond dependency, Live at Head
---

# Dependency Management Practice (10 questions)

#practice #dependency-management

## Related Concepts
- [[Dependency Management]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Diamond dependency | Conflicting versions across ≥3 layers; **unsatisfiable** |
> | SemVer | Lossy estimate: major.minor.patch; solved by **SAT-solvers** |
> | Live at Head | Always current version; **CI tests** replace version numbers |
> | MVS | Choose **oldest satisfying** version for higher fidelity |
> | Source control vs dep mgmt | **Prefer source control** for transparency and coordination |
> | Hyrum's Law + deps | Every observable behavior depended upon; breaks "safe" patches |

---

## Question 1 - Diamond Dependency Definition [recall]
> What is the diamond dependency problem and how many layers does it require?

> [!answer]- Show Answer
> The diamond dependency problem occurs when a low-level dependency (libbase) is required in two **incompatible versions** by two mid-level libraries (liba and libb), both of which are used by a higher-level component (libuser). It requires at least **three layers** of dependency. If liba needs libbase v1 and libb needs libbase v2, there is no general way for libuser to put everything together.

---

## Question 2 - SemVer Components [recall]
> What do the three numbers in Semantic Versioning represent, and what does each type of change signal?

> [!answer]- Show Answer
> SemVer uses **major.minor.patch**:
> - **Major**: breaking change to existing API (can break existing usage)
> - **Minor**: purely additive functionality (should not break existing usage)
> - **Patch**: non-API-impacting implementation details and bug fixes (low risk)
> Version requirements are typically expressed as "anything newer than X, barring major version changes."

---

## Question 3 - SemVer Overconstrain [recall]
> How can SemVer overconstrain a dependency network?

> [!answer]- Show Answer
> If a library (libbase) has two functions Foo and Bar, and a breaking change is made only to Bar, SemVer requires a **major version bump**. Consumers that use only Foo are now blocked from upgrading, even though the change doesn't affect them. The compression of "I made a breaking change" into a single major bump is **lossy** when it doesn't apply at the granularity of individual APIs. This creates **dependency hell** — the SAT-solver reports unsatisfiable constraints for versions that would actually work fine.

---

## Question 4 - Source Control vs Dependency Management [recall]
> What is Google's strongest single piece of advice regarding source control vs dependency management?

> [!answer]- Show Answer
> **Prefer source control problems over dependency management problems.** If you can redefine "organization" more broadly (your entire company rather than just one team), that's very often a good trade-off. Source control problems are easier to think about and cheaper to deal with. When you control the code, you have visibility into usage, can run tests, and can coordinate changes directly.

---

## Question 5 - Live at Head Model [application]
> Your team maintains an open-source library used by 500 downstream projects. You want to adopt the "Live at Head" model. What prerequisites must be in place, and what changes to your release process would be needed?

> [!answer]- Show Answer
> Prerequisites for Live at Head:
> 1. All downstream dependencies must provide **unit tests**
> 2. The dependency network must be **understood and indexed** (reverse edges visible)
> 3. **Compute resources** must be available to run CI across downstream deps
> 4. Dependencies must be **unpinned** (not locked to specific versions)
>
> Process changes: Drop SemVer version numbers. Before committing any change, run tests of affected downstream dependencies. If tests fail, either fix the downstream code or provide an **automated migration tool**. The burden shifts to the API provider to ensure downstream compatibility before committing.

---

## Question 6 - Minimum Version Selection [recall]
> How does Minimum Version Selection (MVS) differ from typical SemVer resolution, and why is it higher fidelity?

> [!answer]- Show Answer
> Traditional SemVer resolution picks the **newest** version that satisfies constraints. MVS picks the **oldest** satisfying version. For example, if liba requires libbase ≥1.7, MVS uses exactly 1.7, not 1.8. This is higher fidelity because when the developer of liba specified ≥1.7, they almost certainly had 1.7 installed and tested against it. MVS produces builds **closest to what the author actually tested**, providing at least anecdotal evidence of interoperability. It acknowledges that newer versions might introduce practical incompatibilities despite SemVer claims.

---

## Question 7 - Importing Dependencies Scenario [application]
> An engineer at your company wants to import a popular but fast-moving open-source library (no stability promises, like Boost) for a critical production system expected to last 10+ years. What concerns should you raise?

> [!answer]- Show Answer
> Key concerns:
> - **No compatibility promise** means upgrades could break at any time, and with a 10+ year lifespan, upgrades will be forced (security vulnerabilities, platform changes)
> - **Hyrum's Law accumulation**: Over time, code will depend on unspecified behaviors of the current version
> - **Orphan risk**: The original importers may leave; no one has experience with the library's internals when an urgent upgrade is needed
> - **Transitive growth**: Other teams may silently depend on it, making it critical infrastructure without anyone realizing
> - Google's experience shows that "fast-moving" libraries with no stability promises are particularly risky for long-lived codebases. Consider whether the functionality can be reimplemented internally with better long-term maintenance guarantees.

---

## Question 8 - Compatibility Promises Comparison [recall]
> Compare the compatibility promises of the C++ Standard Library, Go, and Google's Abseil project.

> [!answer]- Show Answer
> - **C++ Standard Library**: Nearly indefinite **backward + ABI compatibility**. Binaries built against older versions expected to build/link with newer. Governed by SD-8 which defines allowed change types.
> - **Go**: Explicit **source compatibility** between releases, but **no binary compatibility**. You cannot link a library built with one Go version into a program built with another.
> - **Abseil**: No ABI compatibility. Promises a limited form of **API compatibility**: won't make a breaking API change without also providing an **automated refactoring tool** to transform code transparently. Shifts risk in favor of users.

---

## Question 9 - Exporting Dependencies Risk [analysis]
> Google open-sourced its gflags library circa 2006 and the result was described as damaging. Analyze why this happened and what general principle it illustrates about exporting dependencies.

> [!answer]- Show Answer
> The gflags failure resulted from multiple compounding factors:
> 1. Google couldn't perform large-scale refactoring at the time, so internal code couldn't move
> 2. External contributions couldn't be reincorporated due to legal/licensing concerns
> 3. Portability requirements diverged from Google's narrowing internal toolchain focus
> 4. Original maintainers moved on; no team had mandate or priority to support it
> 5. Internal and external versions slowly diverged, creating a hidden fork
>
> **General principle**: Releasing an API externally exposes you to **Hyrum's Law inertia from users you can't monitor**, with competing priorities from outside groups. External dependencies are **far more expensive to maintain** than internal ones. Don't release without a plan (and mandate) to support long-term. "Throw it over the wall and forget" damages reputation and creates unexpected technical constraints.

---

## Question 10 - Infinite Resources Thought Experiment [analysis]
> If the entire OSS ecosystem had infinite compute resources, how would dependency management change? What is the fundamental insight this thought experiment reveals about SemVer?

> [!answer]- Show Answer
> With infinite compute, dependency management would converge to the **Live at Head** model:
> - Every proposed change would run tests of all affected downstream dependencies
> - Safety would be determined by **experimental evidence** (tests pass), not by human estimates (version numbers)
> - SemVer's role of classifying changes as major/minor/patch would become unnecessary
>
> The fundamental insight: **SemVer dominates today because of resource constraints**, not because it's the best model. It requires only local information and doesn't assume tests/CI exist. The "truth" of whether a change is safe comes from running affected tests, not from a human's estimate. SemVer is a lossy compression of this truth, built on shaky foundations of self-attestation treated as absolute. This reveals that unit testing, CI, and cheap compute have the potential to **fundamentally change** how the industry approaches dependency management.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Diamond dependency | ≥3 layers, conflicting versions, **unsatisfiable** |
> | SemVer | major.minor.patch; **lossy estimate**, SAT-solver based |
> | Overconstrain | Major bump blocks unrelated consumers → **dependency hell** |
> | Overpromise | "Safe" patches break via **Hyrum's Law** |
> | Live at Head | Current version always; **CI replaces** version numbers |
> | MVS | **Oldest satisfying** version = higher fidelity |
> | Source control > dep mgmt | Internal = visible, testable; External = costly, opaque |
> | Exporting risk | External users = **Hyrum's Law inertia** you can't control |

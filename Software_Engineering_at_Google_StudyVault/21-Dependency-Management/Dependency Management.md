---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: dependency management, SemVer, diamond dependency, Hyrum's Law, Live at Head, third_party, compatibility
---

# Dependency Management (Importance: ★★★)

#dependency-management #tools

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Core Problem | Managing networks of external dependencies and their changes over time |
| Diamond Dependency | Conflicting version requirements when two deps need different versions of a shared dep |
| SemVer | Lossy compression of compatibility risk into major.minor.patch numbers |
| Live at Head | Always depend on current version; providers test changes against downstream |
| Nothing Changes | Static model; works short-term but unsustainable for security/bug fixes |
| Bundled Distribution | A distributor (e.g., Linux distro) curates compatible sets of deps |
| MVS (Minimum Version Selection) | Use the oldest satisfying version, not newest, for higher fidelity |
| Hyrum's Law | With enough users, every observable behavior becomes depended upon |

## Why Dependency Management Is Difficult
Dependency management is one of the **least understood** and **most challenging** problems in software engineering. The core difficulty is managing a **network** of dependencies, not just a single one.
- Over time, all nodes in the dependency graph will have new versions, and some updates will be critical (security)
- **Conflicting requirements** arise when two nodes need incompatible versions
- The **diamond dependency problem** requires at least three layers and is the canonical unsatisfiable scenario
- Different languages have different tolerance: **Java** can shade symbols; **C++** has near-zero tolerance (One Definition Rule)

## Source Control vs. Dependency Management
Google's strongest advice: **prefer source control problems over dependency management problems**. If you can bring more code into your organization (monorepo), you gain transparency and coordination.
- Source control: you control the project, have visibility, can run tests
- Dependency management: changes happen outside your organization without full access
- Internal dependencies at Google are managed as source control, not dependency management

## Importing Dependencies: Considerations
When importing, Google engineers ask: Does it have tests? Who provides it? What compatibility is promised? How popular? How long will we depend on it? How hard to upgrade?
- **Alice and Bob scenario**: An engineer imports a package for a demo, promises to maintain it, then moves on; thousands of projects depend on it indirectly; a security vulnerability forces a painful upgrade
- Google's `third_party` policies struggle with orphaned-but-critical packages
- **Reuse is healthy** for programming tasks, but costly for long-lived software engineering projects

## Compatibility Promises
Different projects promise different levels of compatibility over time:
- **C++ Standard Library**: Nearly indefinite backward + ABI compatibility
- **Go**: Source compatible between releases, no binary compatibility
- **Abseil (Google)**: No ABI compatibility; promises API compatibility with automated refactoring tools for any breaking change
- **Boost**: No compatibility promises between versions; experimental proving ground

## Semantic Versioning (SemVer)
The de facto standard: **major.minor.patch**. Major = breaking change, minor = additive, patch = bug fix. Version-satisfiability is solved via **SAT-solvers** in package managers.
- SemVer is an **estimate**, not a guarantee, of compatibility
- **Overconstrain**: A breaking change to an unused API forces a major bump that blocks unrelated consumers
- **Overpromise**: Hyrum's Law means "safe" patch changes can break real users who depend on unspecified behaviors
- **Human fallibility**: Maintainers must manually classify changes; errors are inevitable at scale
- Dependency hell = when the SAT-solver finds no satisfying version assignment

## Minimum Version Selection (MVS)
Proposed by Russ Cox for Go: when updating, choose the **oldest satisfying version**, not the newest.
- Produces builds closest to what the author actually tested against
- Provides higher fidelity than "newest version" strategies
- Acknowledges that newer versions might introduce practical incompatibilities despite SemVer claims

## Live at Head
Google's preferred model (extension of trunk-based development to dependencies): always depend on the **current version** of everything.
- Providers test changes against the entire downstream ecosystem before committing
- No SemVer needed; CI and tests determine safety experimentally
- Requires: ubiquitous unit tests, visible dependency network, compute resources for CI, unpinned dependencies
- Places the burden on API providers to not break downstream consumers

## Bundled Distribution Model
An organization curates a compatible set of dependencies and releases them as a unit (e.g., Linux distros, NPM at a point in time).
- Introduces **distributors** who find, patch, and test mutually compatible versions
- Converts a dependency network into a single aggregated dependency with a version number

## Exporting Dependencies
Releasing APIs externally is expensive to maintain over time. Google's experience with **gflags** open source release illustrates the risk: no plan for long-term support, internal/external versions diverged, reputation was damaged.
- **AppEngine case**: Paying customers couldn't upgrade Python/compiler versions, blocking internal infrastructure improvements for ~3 years
- External users cost **far more** to maintain than internal ones due to Hyrum's Law inertia

## Dependency Management with Infinite Resources
If compute were free, the ideal model converges to **Live at Head**: run all affected downstream tests for every proposed change.
- SemVer dominates because it requires only local info, doesn't assume tests/CI exist, and is existing practice
- With infinite compute, evidence-based safety (tests pass) replaces estimate-based safety (version numbers)

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "diamond dependency" | Conflicting version requirements across ≥3 layers; **no general solution** without patching |
| "SemVer overconstrain" | Major bump on unused API blocks unrelated consumers = **dependency hell** |
| "SemVer overpromise" | Patch change breaks real users via **Hyrum's Law** on unspecified behavior |
| "Live at Head" | Always use current version; providers run downstream tests; **no SemVer needed** |
| "MVS" | Choose **oldest satisfying version** for higher fidelity builds |
| "source control vs dependency mgmt" | **Prefer source control** — easier, cheaper, more transparent |
| "Bundled distribution" | Distributor curates compatible set; e.g., **Linux distro** or NPM snapshot |
| "Exporting deps risk" | External users impose **Hyrum's Law inertia**; far more expensive to maintain |

## Related Notes
- [[Large-Scale Changes]]
- [[Continuous Integration]]
- [[Version Control]]
- [[Build Systems]]
- [[Testing Overview]]
- [[Deprecation]]
- [[Software Engineering Fundamentals]]

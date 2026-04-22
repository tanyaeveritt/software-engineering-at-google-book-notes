---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, version control, branch management, trunk-based development, monorepo
---

# Version Control Practice (12 questions)

#practice #version-control #tools

## Related Concepts
- [[Version Control]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | VCS mapping formula | `VCS(filename, time, branch) => file contents` |
> | One-Version Rule | Never allow choice between dependency versions |
> | Dev branches | Harmful; use trunk-based development instead |
> | Source of truth in DVCS | Defined by policy, not technology |
> | Monorepo benefit | Trivially enforces One Version |
> | Release branches | OK if short-lived, no remerge to trunk |

---

## Question 1 - VCS Mapping Formula [recall]
> How does the book conceptualize a Version Control System as an extension of a filesystem?

> [!answer]- Show Answer
> A filesystem maps `filename => contents`. A VCS extends this to `VCS(filename, time, branch) => file contents`, making time and branching explicit dimensions. In default usage, the branch input has a commonly understood default called "head," "default," or "trunk."

---

## Question 2 - Source of Truth in DVCS [recall]
> Why does a Distributed VCS (like Git) require explicit policy that a Centralized VCS does not?

> [!answer]- Show Answer
> In a centralized VCS, the source of truth is baked into the design: the central server's trunk is authoritative. In DVCS, every clone is a full repository with no inherent centrality. Without explicit policy declaring one specific branch in one specific repository as the source of truth, teams risk the "Presentation v5 - final - Josh's version v2" chaos problem. This is why most DVCS projects converge on a hosted central repo (e.g., GitHub).

---

## Question 3 - One-Version Rule Definition [recall]
> State the One-Version Rule and explain why it matters.

> [!answer]- Show Answer
> **"Developers must never have a choice of 'What version of this component should I depend upon?'"** This prevents diamond dependency problems where two incompatible versions of the same library are linked into one binary. It also eliminates merge strategy discussions, reduces wasted effort, and enables efficient large-scale changes. Forking a library without renaming creates a partitioning property in the codebase that can require testing the entire codebase for any new dependency addition.

---

## Question 4 - Dev Branches Harmful [recall]
> Why does Google consider long-lived development branches harmful?

> [!answer]- Show Answer
> Long-lived dev branches are harmful because: (1) the same commits must merge to trunk eventually, and small merges are easier than large ones; (2) merge failures are harder to diagnose with many combined changes; (3) they create coordination overhead (merge coordinator roles, scheduled merge meetings); (4) they don't scale with team size. The better alternative is trunk-based development with CI, extensive testing, and runtime feature flags to disable incomplete features.

---

## Question 5 - Release vs Dev Branch [recall]
> What is the key difference between a release branch and a dev branch in terms of expected lifecycle?

> [!answer]- Show Answer
> A **dev branch** is expected to merge back to trunk, creating reintegration risk and overhead. A **release branch** is expected to be **abandoned** eventually. This makes release branches benign: they capture the exact code of a release, accept minimal cherry-pick fixes, and are never remerged. Organizations with Continuous Deployment often skip release branches entirely.

---

## Question 6 - Shading Limitation [recall]
> Java's "shading" technique can work around some multiple-version problems. What is its fundamental limitation?

> [!answer]- Show Answer
> Shading tweaks the names of internal dependencies to hide them, which works for **functions**. However, it **fails for types** that are passed between dependencies. There is no good solution to shading types: if a `Map` defined in libbase v1 is passed to an API from libbase v2, the types are incompatible. This means shading is a partial workaround, not a general solution to the diamond dependency problem.

---

## Question 7 - Feature Flags Instead of Branches [application]
> Your team is debating whether to use a long-lived feature branch for a 3-month project. Propose an alternative approach aligned with Google's practices.

> [!answer]- Show Answer
> Instead of a long-lived feature branch, use **trunk-based development** with **runtime feature flags**. Commit small, incremental changes directly to trunk. Disable the incomplete feature in production using a feature flag so it doesn't affect users. This avoids the merge hell of a 3-month branch, allows continuous integration testing, and makes the feature easy to enable/disable independently. When the feature is ready, flip the flag. The code has been continuously tested against trunk, so there is no risky "big bang" merge.

---

## Question 8 - Monorepo Trade-offs [application]
> Your growing company (200 engineers) has 50 separate Git repositories. Teams frequently encounter dependency version conflicts. What approach would you recommend?

> [!answer]- Show Answer
> Adopt the **One-Version Rule** across repositories. Options include: (1) Migrate to a **monorepo** where One Version is trivially enforced. (2) If a true monorepo isn't feasible, create a **Virtual Monorepo (VMR)** using Git submodules, Bazel with external dependencies, or similar tooling that provides a consistent "virtual trunk" across repos. The key requirement is that for any given dependency, there must be only one version available. This eliminates diamond dependency problems and simplifies tooling. Also enforce interrepository dependencies to be unpinned/"at head."

---

## Question 9 - Google's VCS Scale [recall]
> Describe Google's internal VCS (Piper) and its scale metrics.

> [!answer]- Show Answer
> **Piper** is Google's custom in-house centralized VCS built as a distributed microservice on Google's production infrastructure. It stores more than **80 TB** of content and metadata, serves ~50,000 engineers, and handles **60,000-70,000 commits per work day**. It takes about 15 seconds to create a new client, add a file, and commit a change. Piper supports granular ownership via **OWNERS files** at every level of the hierarchy, and enforces policies like the One-Version Rule natively.

---

## Question 10 - DORA Research [recall]
> What does the DORA research say about trunk-based development?

> [!answer]- Show Answer
> The DevOps Research and Assessment (DORA) organization, through the State of DevOps reports and the book *Accelerate*, found a **strong positive correlation** between trunk-based development (no long-lived dev branches) and high-performing software organizations. This research confirms Google's experience that long-lived dev branches are not a good default plan and that committing frequently to trunk leads to better technical outcomes.

---

## Question 11 - Uncommitted Work as Branch [analysis]
> An organization asks all teams to rename "Widget" to "OldWidget" across the codebase. Why does the book consider uncommitted work relevant to this task, and how does branch policy affect the difficulty?

> [!answer]- Show Answer
> The book argues that every piece of uncommitted work-in-progress is conceptually equivalent to a branch. When renaming a symbol, there are three possible scopes: (1) rename on trunk only, (2) rename on all branches, or (3) rename on all branches plus find all devs with outstanding uncommitted changes referencing the symbol. In organizations with many long-lived dev branches, option (3) becomes impossibly expensive: you'd have to discover and update an unbounded set of hidden branches. Trunk-based development with minimal pending changes makes this much simpler, as there are far fewer "branches" in flight.

---

## Question 12 - Virtual Monorepo Future [analysis]
> The book predicts a convergence toward "virtual monorepos." Analyze the technical and social forces driving this prediction.

> [!answer]- Show Answer
> **Technical forces**: (1) VCSs like Git are improving support for larger repos (shallow clones, sparse branches), reducing the need to keep repos small. (2) Build systems (Bazel, CMake) increasingly support cross-repo dependencies with consistent versioning. (3) Version numbers are essentially timestamps, and allowing version skew adds costly time-dimension complexity. **Social forces**: (1) The OSS "manyrepo" model creates coordination challenges at scale. (2) Large organizations (Google, Microsoft, Facebook) have demonstrated monorepo benefits. (3) Linux distributions already act as de facto bundled distribution models, finding compatible versions across packages. The prediction is that someone will catalyze a standard VMR that provides easy access to compatible dependency sets, eliminating the costly "which version" dimension from software engineering.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | VCS mapping | `VCS(filename, time, branch) => file contents` |
> | DVCS source of truth | Must be defined by explicit **policy** |
> | One-Version Rule | No choice in dependency versions; prevents diamond deps |
> | Dev branches | Harmful; trunk-based dev + CI + feature flags is better |
> | Release branches | OK if short-lived and abandoned (not remerged) |
> | Shading | Works for functions, fails for **types** |
> | Monorepo | Trivially enforces One Version; VMR can approximate |
> | DORA findings | Trunk-based dev correlates with high-performing orgs |
> | Piper | Google's centralized VCS: 80+ TB, 50K engineers, 60-70K commits/day |

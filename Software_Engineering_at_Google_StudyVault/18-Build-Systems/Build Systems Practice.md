---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, build systems, Bazel, artifact-based, distributed builds, dependencies
---

# Build Systems Practice (10 questions)

#practice #build-systems #tools

## Related Concepts
- [[Build Systems]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Task-based problem | Too much power to engineers, not enough to system |
> | Artifact-based advantage | Declarative; enables parallelism, caching, reproducibility |
> | Sandboxing | Filesystem isolation via LXC; prevents action conflicts |
> | Remote caching | ObjFS at Google; requires reproducible builds |
> | Strict transitive deps | Must explicitly declare direct deps; prevents accidental coupling |
> | One-Version Rule | One version per external dep; prevents diamond dependency |

---

## Question 1 - Two Build Properties [recall]
> What are the two key properties a build system should optimize for, according to the chapter?

> [!answer]- Show Answer
> **Fast**: a developer types a single command and gets back the resulting binary quickly. **Correct**: every time any developer runs a build on any machine, they get the same result (given the same inputs). Many older build systems make trade-offs between speed and correctness, but Bazel's objective is to ensure both are always achievable.

---

## Question 2 - Task-Based Dark Side [recall]
> List three fundamental problems with task-based build systems.

> [!answer]- Show Answer
> 1. **Difficulty parallelizing**: The system can't know if tasks conflict (shared state/files), so it must either risk rare bugs or restrict to single-threaded execution.
> 2. **Difficulty with incremental builds**: Tasks can do anything (download files, write timestamps), so the system can't determine if outputs are still valid. Engineers end up running `clean` before every build.
> 3. **Difficulty maintaining scripts**: Implicit dependencies between tasks, environment assumptions, nondeterminism, and race conditions create hard-to-debug failures.
> Root cause: **too much power to engineers, not enough to the system**.

---

## Question 3 - Functional Programming Analogy [recall]
> Explain the analogy between artifact-based build systems and functional programming.

> [!answer]- Show Answer
> **Task-based** systems are like **imperative** programming: engineers define a series of steps executed in order, and the system has limited ability to optimize. **Artifact-based** systems are like **functional** programming: engineers describe a computation (declare artifacts and dependencies), and the system decides how to execute it. Just as functional languages can trivially parallelize pure functions and guarantee correctness, artifact-based builds can run steps in parallel and reuse cached outputs because each step is a pure function of its inputs.

---

## Question 4 - Sandboxing Mechanism [recall]
> How does Bazel use sandboxing to prevent action conflicts?

> [!answer]- Show Answer
> On supported systems (Linux), every action is isolated via a **filesystem sandbox** using LXC (the same technology behind Docker). Each action can see only a restricted view of the filesystem: its **declared inputs** and any **outputs it produces**. Files written but not declared as outputs are discarded when the action finishes. Actions also cannot communicate via the network. This makes it impossible for actions to conflict with each other, since they cannot read undeclared files or observe each other's side effects.

---

## Question 5 - Remote Caching vs Remote Execution [recall]
> Distinguish between remote caching and remote execution in distributed builds.

> [!answer]- Show Answer
> **Remote caching**: Build outputs are stored in a shared cache. Before building a target, the system checks the cache; if the artifact exists (keyed on target + hash of inputs), it downloads instead of building. The build itself still runs locally. Google uses **ObjFS** with FUSE daemon for on-demand file content delivery.
> **Remote execution**: The actual work of building is distributed across a pool of **workers** coordinated by a central **build master**. Workers execute actions, read/write to the distributed cache, and return results. Google uses **Forge** (Distributor + Scheduler + Executors). Remote execution requires fully self-describing, self-contained, deterministic build processes.

---

## Question 6 - Strict Transitive Dependencies [recall]
> What problem did Google encounter without strict transitive dependency enforcement, and how was it solved?

> [!answer]- Show Answer
> Without enforcement, if target A depended on B which depended on C, A could use symbols from C without declaring a direct dependency. If B later removed its dependency on C, A (and everything depending on C via B) would break. Dependencies effectively became **part of the public contract** and could never be safely removed, causing them to accumulate and slow builds. Google solved this by introducing **strict transitive dependency mode** in Blaze: it detects when a target references a symbol without a direct dependency, fails the build, and provides a shell command to automatically add the dependency. The multi-year rollout made builds faster and let engineers safely remove unnecessary dependencies.

---

## Question 7 - Choosing Build Systems [application]
> A startup with 15 engineers currently uses shell scripts for builds. They're growing quickly. What build system strategy would you recommend and why?

> [!answer]- Show Answer
> Adopt an **artifact-based build system** like Bazel from the start. The book argues that even for small projects, artifact-based systems scale down well. Shell scripts will fail as the team grows: they're non-reproducible across machines, can't parallelize, don't support incremental builds, and become a maintenance burden. Task-based systems (Maven, Gradle) improve on scripts but still suffer from parallelism and caching limitations. Since the startup is growing, the investment in Bazel pays off quickly: deterministic builds, remote caching, and eventually distributed builds. Google's experience shows that build system migration becomes more expensive as the project grows, so "start right" is cheaper than "migrate later."

---

## Question 8 - External Dependency Hash [application]
> Your team notices that a third-party library's behavior changed even though the version number stayed the same. How would Bazel's approach prevent this?

> [!answer]- Show Answer
> Bazel requires a workspace-wide **manifest file** listing a **cryptographic hash** for every external dependency. When Bazel downloads a dependency, it checks the actual hash against the expected hash in the manifest. If they differ (because the artifact was silently modified), **the build fails**. The hash can only be updated via a checked-in source control change that must be approved and reviewed. This provides: (1) **a record** of when every dependency was updated, (2) **protection against tampering** (supply chain attacks), and (3) **reproducibility** — checking out an old version guarantees the same dependency versions. For extra safety, mirror dependencies onto controlled servers.

---

## Question 9 - One-Version Rule Rationale [application]
> A colleague wants to add a second version of a JSON library because the new version has a breaking API change. Explain why the One-Version Rule prohibits this.

> [!answer]- Show Answer
> The **One-Version Rule** states that only one version of each external dependency is allowed in the repository. Allowing two versions creates the **diamond dependency problem**: if target X depends on JSON v1 and target Y depends on JSON v2, any target depending on both X and Y will link two incompatible versions of the same library. In C++, this causes undefined behavior; in Java, it creates classpath conflicts. Even if it works initially, as dependencies grow transitively, the incompatibility becomes nearly inevitable. Instead, the team should: (1) migrate all uses to the new version in a coordinated effort, or (2) temporarily have both with a plan to eliminate the old one, but never allow new code to depend on the old version.

---

## Question 10 - Fine-Grained vs Coarse Modules [analysis]
> Analyze the trade-offs between defining one large BUILD target for an entire project versus having fine-grained targets per directory (1:1:1 rule).

> [!answer]- Show Answer
> **Single large target**: Easy to maintain (rarely touch the BUILD file), but the build system must always build the entire project at once. No parallelism, no incremental caching, no ability to run only affected tests. Every change triggers a full rebuild.
> **Fine-grained targets (1:1:1)**: Build system can parallelize compilation, cache individual module outputs, and run only tests affected by a change. At Google's scale, a typical binary depends on tens of thousands of fine-grained targets. Downside: more BUILD file maintenance and explicit dependency management. Google mitigates this with **automated tooling** that manages BUILD files. The benefits of finer granularity compound at scale: distributed builds and smart test selection depend on many small, independent units. The book concludes that fine-grained modules are decisively better for large codebases.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Speed + correctness | Two core build system properties |
> | Task-based flaw | Too much engineer power; no safe parallelism or caching |
> | Artifact-based = functional | Declare what to build; system decides how |
> | Sandboxing | LXC filesystem isolation; each action sees only declared I/O |
> | Remote caching vs execution | Cache = shared artifacts; Execution = distributed workers |
> | Strict transitive deps | Must declare direct deps; prevents accumulation and breakage |
> | One-Version Rule | One version per external dep; prevents diamond dependency |
> | Fine-grained modules | Better parallelism, caching, test selection; tooling reduces overhead |
> | Cryptographic hashes | Detect tampered or silently updated external deps |

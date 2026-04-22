---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: build systems, Bazel, Blaze, artifact-based, task-based, distributed builds, remote caching, dependencies, One-Version Rule
---

# Build Systems and Build Philosophy (Importance: ★★★)

#build-systems #tools

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Build System Goal | Transform source code into executables; optimize for **speed** and **correctness** |
| Task-Based (Ant, Maven) | Engineers define arbitrary scripts as tasks; flexible but limited parallelism/caching |
| Artifact-Based (Bazel/Blaze) | Declarative manifests; system controls execution; enables parallelism + caching |
| Functional Analogy | Artifact-based builds are like functional programming: declare what, system decides how |
| Remote Caching | Share build outputs across machines via central cache (ObjFS at Google) |
| Remote Execution | Distribute build actions across worker pool (Forge at Google) |
| 1:1:1 Rule | One package, one target, one BUILD file per directory |
| Strict Transitive Deps | Targets must explicitly list all direct dependencies |
| One-Version Rule | Only one version of each external dependency in the repository |
| Sandboxing | Actions isolated via filesystem sandbox (LXC); prevents conflicts |

## Purpose of a Build System
A build system transforms source code into executables, optimizing for two properties:
- **Fast**: a developer types a command and gets a binary quickly
- **Correct**: the same inputs always produce the same outputs on any machine

Build systems aren't just for humans: the majority of Google builds are **automated** (testing, releases, CI). Nearly all development tools tie into the build system.

## What Happens Without a Build System
The progression of pain as a project grows:
1. **Just a compiler** works for tiny projects but fails with multiple languages/dependencies
2. **Shell scripts** become tedious, slow, non-reproducible, and fragile across machines
3. **New team members** can't reproduce the environment
4. **Automated builds** break because of subtle environment differences
5. **Idle machines** can't be leveraged for parallel building

## Task-Based Build Systems (Ant, Maven, Gradle)
Engineers define **tasks** as scripts with dependencies forming an acyclic graph. Tasks can execute arbitrary logic.

### The Dark Side
- **Parallelism is hard**: system can't know if tasks conflict (shared state)
- **Incremental builds unreliable**: can't determine if task outputs are still valid
- **Debugging is painful**: implicit dependencies, environment assumptions, nondeterminism, race conditions
- The fundamental problem: **too much power to engineers, not enough to the system**

## Artifact-Based Build Systems (Bazel/Blaze)
Instead of arbitrary tasks, engineers write **declarative manifests** specifying artifacts to build, their dependencies, and limited options. The system controls the "how."

### The Functional Programming Analogy
Task-based builds are like **imperative** programming (steps executed in order). Artifact-based builds are like **functional** programming (declare computation, compiler decides execution). This enables trivial parallelization and strong correctness guarantees.

### Bazel Example
```
java_binary(name = "MyBinary", srcs = ["MyBinary.java"], deps = [":mylib"])
java_library(name = "mylib", srcs = [...], deps = ["//java/com/example/common"])
```
- **Targets**: `java_binary`, `java_library` — each produces a specific artifact
- **deps**: explicit dependencies within same package, across packages, or external
- System parses all BUILD files, builds transitive dependency graph, executes in parallel

### Key Advantages
- **Parallelism**: system knows exactly what each step does; safe to run concurrently
- **Incremental builds**: outputs depend only on inputs; unchanged inputs = reusable outputs
- **Reproducibility**: deterministic builds from the same inputs

### Bazel Tricks
| Feature | How It Works |
|---------|-------------|
| **Tools as dependencies** | Compilers/tools are declared dependencies; auto-downloaded if missing |
| **Toolchains** | Platform-independent: targets depend on toolchain types, not specific tools |
| **Custom rules** | Extensible via rule definitions with declared inputs/outputs/actions |
| **Sandboxing** | Each action sees only its declared inputs via filesystem sandbox (LXC/Docker) |
| **Deterministic external deps** | Workspace manifest with cryptographic hashes; build fails on hash mismatch |

## Distributed Builds

### Remote Caching
Build outputs stored in a shared remote cache. Before building, check if artifact already exists in cache.
- Google's implementation: **ObjFS** (Bigtable backend + FUSE frontend `objfsd`)
- File contents downloaded **on-demand**; builds 2x faster than local-only storage
- Requires **completely reproducible** builds (same inputs = same outputs)

### Remote Execution
Build actions distributed across a scalable pool of **workers** coordinated by a **build master**.
- Google's implementation: **Forge** (Distributor client + Scheduler service + Executor pool)
- Workers read dependencies from and write results to the distributed cache
- Requires **self-describing** build environments and **deterministic** processes
- Scales to all builds at Google: millions of builds, millions of test cases, petabytes of output daily

## Dealing with Modules and Dependencies

### Fine-Grained Modules (1:1:1 Rule)
Google favors **small modules**: one package, one target, one BUILD file per directory for languages like Java. A typical production binary depends on tens of thousands of targets.
- Benefits: faster distributed builds, better caching, smarter test selection
- Tooling automates BUILD file management to reduce burden

### Module Visibility
Targets specify **visibility** (public, private, or explicit whitelist). Minimize visibility like minimizing API surface in code.

### Strict Transitive Dependencies
Blaze enforces **strict transitive dependency mode**: if target A uses a symbol from target C (transitively through B), A must explicitly depend on C.
- Multi-year rollout across Google; prevents dependencies from becoming part of public contract
- Builds are faster (fewer unnecessary deps); engineers can safely remove deps

### External Dependencies
- **Manual version management** (not automatic): explicit versions in source control for reproducibility
- **One-Version Rule**: only one version of each third-party dependency allowed
- **No automatic transitive downloads**: global file lists all external deps and versions explicitly
- **Vendoring**: Google checks all third-party libs into `third_party` directory
- **Security**: require cryptographic hashes; mirror dependencies onto controlled servers

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "task-based vs artifact-based" | Task-based: flexible but no parallelism/caching. Artifact-based: declarative, parallel, cacheable |
| "functional programming analogy" | Artifact-based = functional (declare what); task-based = imperative (define steps) |
| "why not shell scripts" | Non-reproducible, slow, no parallelism, environment-dependent, don't scale |
| "remote caching" | Share build outputs; requires reproducible builds; Google uses ObjFS |
| "remote execution" | Distribute actions across worker pool; Google uses Forge |
| "sandboxing" | Filesystem isolation via LXC; prevents action conflicts |
| "strict transitive deps" | Must explicitly declare all direct deps; prevents accidental coupling |
| "One-Version Rule" | One version per external dependency; prevents diamond dependency |
| "1:1:1 rule" | One package, one target, one BUILD file per directory |
| "deterministic external deps" | Cryptographic hash in workspace manifest; build fails on mismatch |

## Related Notes
- [[Version Control]]
- [[Code Search]]
- [[Continuous Integration]]
- [[Large-Scale Changes]]
- [[Style Guides and Rules]]

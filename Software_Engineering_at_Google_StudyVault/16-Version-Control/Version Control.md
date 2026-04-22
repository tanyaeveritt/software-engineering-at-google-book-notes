---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: version control, branch management, trunk-based development, monorepo, One-Version Rule, DVCS, source of truth
---

# Version Control and Branch Management (Importance: ★★★)

#version-control #tools

*Reading time: ~4 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| VCS Purpose | Tracks revisions of files over time; coordinates multi-developer work |
| VCS as Mapping | `VCS(filename, time, branch) => file contents` |
| Source of Truth | One branch in one repo must be the canonical, authoritative source |
| One-Version Rule | Developers must never choose between versions of a dependency |
| Trunk-Based Dev | Commit frequently to trunk; no long-lived dev branches |
| Monorepo | Single repo for all code; trivially enforces One Version |
| Release Branches | Acceptable if short-lived, minimal cherry picks, never remerged |
| DVCS vs CVCS | DVCS (Git) is dominant but needs policy to define central source of truth |

## Why Version Control Matters
VCS is the engineer's primary tool for managing the interplay between source code and **time**. It extends a filesystem from `filename => contents` to `(filename, time, branch) => contents`.
- Enables **multi-developer coordination** with atomic commits
- Provides **audit trail** for every change to every line (regulatory/legal)
- The ritual of committing triggers **reflection** (run tests, static analysis, check coverage)
- Scales teams: removes the question of "which version is more recent?"

## Centralized VCS vs Distributed VCS
**Centralized VCS** (Subversion, CVS, Perforce): single central repository; operations require server communication. Source of truth is baked into the design.
- Early systems used file-level **locking** (RCS); later systems tracked changes and allowed concurrent edits
- Google uses **Piper**, a custom centralized VCS handling 80+ TB of data, 60-70K commits/day

**Distributed VCS** (Git, Mercurial): every clone is a full repository. No inherent central source of truth.
- Enables offline work and distributed collaboration
- Requires **explicit policy** to define which repo/branch is authoritative
- In practice, most DVCS projects converge on a single central repo (e.g., GitHub)
- Scaling challenge: cloning large repos is expensive; transmitting full history is wasteful at scale

## Source of Truth
Every project must have a **single, unambiguous source of truth**. Without it, collaboration becomes chaotic (the "Presentation v5 - final - Josh's version v2" problem).
- CVCS provides this by design; DVCS needs it by **policy**
- Even in DVCS, hosted solutions (GitHub, GitLab) restore centralization
- Hierarchical source of truth works (e.g., RedHat vs Linus for Linux kernel)

## Branch Management

### Dev Branches Are Harmful
Long-lived development branches are a **productivity drain** that doesn't scale.
- Merging large branches is risky and difficult to debug
- The same commits must merge to trunk eventually; **small merges are easier**
- Creates a "merge coordinator" bottleneck and scheduled merge meetings
- **Better alternative**: trunk-based development + extensive testing + CI + runtime feature flags

### Release Branches Are Acceptable
Release branches are benign because they are expected to be **abandoned**, not merged back.
- Useful when release lifetime > a few hours
- Keep cherry picks to a minimum
- Organizations with **Continuous Deployment** often skip release branches entirely

### Work in Progress = Branch
Every uncommitted local change is conceptually a branch. This matters for refactoring: renaming a symbol on trunk doesn't rename it in everyone's pending changes.

## One-Version Rule
**"Developers must never have a choice of which version of a component to depend upon."**
- Prevents **diamond dependency** problems (incompatible forks linked into one binary)
- Forking a library without renaming creates a partitioning problem in the codebase
- Java **shading** can hack around function conflicts but fails for shared types
- Consistency at all levels is critical for organizational efficiency

## Google's Monorepo Approach
Google stores virtually all code in a single monorepo managed by **Piper**.
- ~50,000 engineers, 60-70K commits/day
- **OWNERS files** at every level enforce granular code ownership
- One Version is trivially enforced; no version selection decisions
- Monorepo is not the only way: **Virtual Monorepo (VMR)** via Git submodules, Bazel external deps, or CMake subprojects can approximate the benefits
- Fine-grained repos work if they maintain a consistent "virtual head/trunk"

## Future of Version Control
- VCSs will improve to handle **larger repositories** with better performance
- Lightweight **inter-repository linkage** will become standard
- Version numbers are essentially **timestamps**; allowing version skew adds costly time-dimension complexity
- The industry will converge toward something like a virtual monorepo with One-Version semantics

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "long-lived dev branches" | **Harmful**: use trunk-based development + CI instead |
| "source of truth in DVCS" | Must be defined by **policy** (e.g., main branch on GitHub) |
| "One-Version Rule" | Developers must never choose between dependency versions |
| "diamond dependency" | Caused by multiple incompatible versions of same library |
| "monorepo advantage" | Trivially enforces One Version; simplifies tooling and visibility |
| "release branch vs dev branch" | Release branches are OK (abandoned); dev branches are harmful (remerged) |
| "VCS as mapping" | `VCS(filename, time, branch) => file contents` |
| "trunk-based development correlation" | DORA research shows strong correlation with high-performing orgs |
| "Piper" | Google's custom centralized VCS; 80+ TB, 60-70K commits/day |
| "shading limitation" | Works for functions but **fails for shared types** |

## Related Notes
- [[Code Search]]
- [[Build Systems]]
- [[Large-Scale Changes]]
- [[Continuous Integration]]
- [[Code Review]]
- [[Software Engineering Fundamentals]]

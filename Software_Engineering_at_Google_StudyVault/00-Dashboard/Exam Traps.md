---
source_pdf: swe_at_google.2.pdf
part: all
keywords: exam traps, weak areas, common mistakes, software engineering
---

# Exam Traps

#dashboard #exam-traps #swe-at-google

> [!warning] Purpose of this note
> A collection of commonly confused or frequently missed points — a **mistake and trap reference note**.

## Software Engineering vs Programming

> [!danger]- Trap: Confusing programming with software engineering
> - **Trap**: Treating them as synonyms
> - **Why confusing**: Both involve writing code
> - **Correct**: Software engineering = programming **integrated over time**. It includes maintenance, scaling, and team collaboration — not just writing code
> - [[Software Engineering Fundamentals]]

> [!danger]- Trap: Hyrum's Law applies only to APIs
> - **Trap**: Thinking Hyrum's Law is limited to public API contracts
> - **Why confusing**: It sounds like an API versioning rule
> - **Correct**: Hyrum's Law applies to **all observable behaviors** — including hash ordering, error message text, timing, and performance characteristics. Any behavior users can observe **will** be depended on
> - [[Software Engineering Fundamentals]]

## Team Culture

> [!danger]- Trap: Genius Myth means brilliant engineers are bad
> - **Trap**: Interpreting the "Genius Myth" as devaluing individual skill
> - **Why confusing**: The chapter criticizes solo heroes
> - **Correct**: The point is that **hiding work** and working in isolation is harmful, not that being skilled is bad. Even brilliant engineers need teams
> - [[Team Culture and Collaboration]]

> [!danger]- Trap: "Googleyness" is about culture fit
> - **Trap**: Thinking Googleyness means "someone I'd like to have a beer with"
> - **Why confusing**: It was informally used this way for years
> - **Correct**: Google redefined it as explicit attributes: thrives in ambiguity, values feedback, challenges status quo, puts user first, cares about team, does the right thing
> - [[Team Culture and Collaboration]]

## Testing

> [!danger]- Trap: Test size = test scope
> - **Trap**: Confusing test **size** (resource constraints) with test **scope** (code under test)
> - **Why confusing**: Both use Small/Medium/Large terminology informally
> - **Correct**: **Size** = execution constraints (single process, single machine, multi-machine). **Scope** = what code is being tested (unit, integration, end-to-end). A "small" test can test multiple units; a "unit" test can be "medium" sized
> - [[Testing Overview]]

> [!danger]- Trap: DAMP means don't use any helpers in tests
> - **Trap**: Thinking DAMP prohibits all shared code in tests
> - **Why confusing**: "Descriptive And Meaningful Phrases" sounds like "inline everything"
> - **Correct**: DAMP means prefer **clarity over DRY deduplication**. Shared test helpers, values, setup, and infrastructure are all fine — just don't sacrifice readability for deduplication
> - [[Unit Testing]]

> [!danger]- Trap: Mocking is the best test double technique
> - **Trap**: Defaulting to mocking frameworks for all test doubles
> - **Why confusing**: Mocking frameworks are popular and widely taught
> - **Correct**: **Prefer real implementations** first. If not possible, prefer **fakes** (realistic behavior) over **stubbing** (hardcoded returns) over **interaction testing** (verifying calls). Overuse of mocking leads to brittle, change-detector tests
> - [[Test Doubles]]

> [!danger]- Trap: 100% code coverage guarantees quality
> - **Trap**: Treating code coverage as a sufficient metric
> - **Why confusing**: Coverage is easy to measure and track
> - **Correct**: Coverage measures what code is **executed**, not what is **verified**. A test can execute code without asserting anything useful. Coverage is necessary but not sufficient
> - [[Testing Overview]]

## Version Control & Dependencies

> [!danger]- Trap: SemVer solves dependency compatibility
> - **Trap**: Believing semantic versioning guarantees safe upgrades
> - **Why confusing**: SemVer is the industry standard for versioning
> - **Correct**: SemVer expresses **intent**, not guarantee. Providers can misjudge what's breaking (Hyrum's Law). The book advocates **Live at Head** where providers take responsibility for compatibility
> - [[Dependency Management]]

> [!danger]- Trap: Trunk-based dev means no branches ever
> - **Trap**: Thinking trunk-based development prohibits all branches
> - **Why confusing**: The name suggests "only trunk"
> - **Correct**: Short-lived feature branches and release branches are acceptable. The rule is **no long-lived development branches** — merge to main frequently
> - [[Version Control]]

> [!danger]- Trap: Monorepo means everything in one directory
> - **Trap**: Confusing monorepo with a flat file structure
> - **Why confusing**: "Single repository" sounds like "single folder"
> - **Correct**: A monorepo is a single version control repository containing multiple projects, each with its own directory structure, build targets, and ownership. It enables atomic cross-project changes and consistent tooling
> - [[Version Control]]

## Build & CI/CD

> [!danger]- Trap: Task-based and artifact-based builds are equivalent
> - **Trap**: Treating Make-style and Bazel-style builds as interchangeable
> - **Why confusing**: Both produce build outputs
> - **Correct**: Task-based (Make) executes arbitrary commands — can't be parallelized/cached safely. Artifact-based (Bazel) declares inputs/outputs — enables caching, remote execution, and reproducibility
> - [[Build Systems]]

> [!danger]- Trap: Hermetic tests just mean "no network calls"
> - **Trap**: Reducing hermeticity to network isolation
> - **Why confusing**: Network access is the most obvious external dependency
> - **Correct**: Hermetic means isolated from **all** external environment changes — filesystem, time, system state, environment variables, other tests, and network. The goal is deterministic, repeatable results
> - [[Continuous Integration]]

## Deprecation & Large-Scale Changes

> [!danger]- Trap: Advisory deprecation is sufficient to remove old systems
> - **Trap**: Assuming warning annotations will motivate migration
> - **Why confusing**: Deprecation warnings feel like they should trigger action
> - **Correct**: Advisory deprecation rarely leads to complete migration. **Compulsory deprecation** with enforced deadlines, migration tooling, and team ownership is needed for actual removal
> - [[Deprecation]]

> [!danger]- Trap: LSCs are just big refactorings
> - **Trap**: Treating large-scale changes as scaled-up regular changes
> - **Why confusing**: The mechanics seem similar
> - **Correct**: LSCs require distinct infrastructure: automated tooling, sharding across reviewers, incremental testing, rollback mechanisms, and organizational policy support. They face unique barriers like haunted graveyards and heterogeneity
> - [[Large-Scale Changes]]

---

## Related
- [[MOC - Software Engineering at Google]] → Weak Areas section
- [[testing_tutor/Software_Engineering_at_Google_StudyVault/00-Dashboard/Quick Reference]]

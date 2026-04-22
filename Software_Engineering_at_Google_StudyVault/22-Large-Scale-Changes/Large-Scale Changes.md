---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: large-scale changes, LSC, Rosie, refactoring, codebase migration, code review, automation
---

# Large-Scale Changes (Importance: ★★★)

#large-scale-changes #tools

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| LSC Definition | Logically related changes that cannot be submitted as a single atomic unit |
| Who Does LSCs | Infrastructure teams with domain expertise, not individual product teams |
| Rosie | Google's tool for sharding, testing, reviewing, and submitting LSC shards |
| TAP Train | Batches LSC shards for testing; 5-step process every 3 hours |
| Authorization | Lightweight approval by experienced engineer committee |
| Global Approver | Single expert reviewer for all shards instead of each local owner |
| Haunted Graveyards | Ancient, untested code that blocks migrations; solution is **testing** |
| Cattle vs Pets | LSC shards are cattle — nameless, rollback-able at any time |

## What Is a Large-Scale Change?
An LSC is any set of changes that are **logically related but cannot practically be submitted as a single atomic unit**. This might be because it touches too many files, would always have merge conflicts, or spans distributed repositories.
- Common categories: cleaning up antipatterns, replacing deprecated features, enabling compiler upgrades, migrating to new systems
- Most LSCs are **near-zero functional impact** — widespread textual updates for clarity, optimization, or compatibility
- At scale, infrastructure teams routinely change hundreds of thousands of references; in the largest cases, **millions**

## Who Deals with LSCs?
**Infrastructure teams** that build and manage the underlying systems, not product teams.
- They have the **domain knowledge** to fix references correctly
- **Centralization** allows faster recovery from errors (common error playbooks)
- Avoids **unfunded mandates** — forcing product teams to organically migrate is slow and usually fails
- Engineers tend to use **existing code as examples** for new code, so old patterns perpetuate without centralized removal

## Barriers to Atomic Changes
Four major barriers prevent large changes from being submitted atomically:
- **Technical limitations**: VCS operations scale linearly; large commits can block other writers or exceed memory
- **Merge conflicts**: Probability grows with change size; at Google's scale, someone is always making changes
- **No haunted graveyards**: Ancient, untested code blocks migrations; the counter is **thorough testing**
- **Heterogeneity**: Different CI systems, tooling, formatting, and presubmit checks across projects make automation difficult

## LSC Infrastructure

### Policies and Culture
A **lightweight approval process** overseen by experienced engineers reviews LSC proposals. Authors submit a brief document explaining the change, its estimated impact, and an FAQ.
- The committee often directs reviews to a **single global approver** rather than many local owners
- Cultural norm: code owners don't hold a **veto** over LSCs; they trust infrastructure domain expertise
- Good FAQ + solid track record have generated widespread endorsement

### Codebase Insight
Google uses **Kythe** (semantic indexing) for questions like "Where are callers of this function?" and tools like **ClangMR, JavacFlume, Refaster** for AST-based transformations.
- Human effort should scale **sublinearly** with codebase size
- Rule of thumb: if a change requires more than **500 edits**, use automated change-generation tools

### Change Management (Rosie)
Rosie is Google's platform for sharding, testing, reviewing, and submitting LSC changes.
- Splits a master change into smaller **shards** based on project boundaries and ownership
- Each shard goes through an independent **test → mail → review → submit** pipeline
- Caps outstanding shards to manage CI load

### Testing LSCs (TAP Train)
The TAP Train batches LSCs for efficient testing (runs every 3 hours):
1. Run **1,000 random tests** per change as a sample
2. Combine passing changes into one "uber-change" (the train)
3. Run the **union** of all directly affected tests (can be every test at Google)
4. For each failing non-flaky test, rerun against **individual changes** to find the culprit
5. Generate a report for each change with pass/fail evidence

### Language Support
- **Type aliasing and forwarding functions** are invaluable for incremental migration
- **Statically typed languages** are much easier for automated changes than dynamic ones (Python, Ruby, JavaScript)
- **Automatic formatters** (google-java-format, clang-format) are crucial — generated changes must look human-written

### Code Review
LSC shards use either local owner review (when context is needed) or a **global approver** who has domain expertise.
- Global approvers use **pattern-based tooling** to auto-approve expected changes and manually examine only anomalies

## The LSC Process (4 Phases)
1. **Authorization**: Brief proposal document → committee review → approval
2. **Change creation**: Automated tooling generates the global change; runs formatters as a separate pass
3. **Shard management**: Rosie shards the change, tests each shard, mails reviewers, manages review and submission
4. **Cleanup**: Prevent backsliding via **Tricorder** static analysis flags at review time; deprecation tools

## Case Studies
- **scoped_ptr → std::unique_ptr**: 500,000+ references; months of work; 700+ independent changes touching 15,000+ files per day at peak. Long tail of Hyrum's Law behavior dependencies.
- **Operation RoseHub**: Mad Gadget vulnerability in Apache Commons; 50+ volunteers sent 2,600 patches to GitHub projects; human-driven LSC at open-source scale
- **Filling Potholes**: Renaming stl_util.h vs map-util.h (inconsistent separator); small fix, big productivity gain

## Cattle vs Pets for Changes
Individual handcrafted changes are **pets** — engineers invest days/weeks and feel ownership. LSC shards are **cattle** — nameless commits that might be rolled back at any time with little cost. Automation means tooling can be updated and new changes generated at very low cost.

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "LSC definition" | Logically related changes **too large for a single atomic commit** |
| "Who does LSCs" | **Infrastructure teams** with domain expertise, not product teams |
| "Unfunded mandate" | Forcing product teams to migrate → **slow, usually fails** |
| "Haunted graveyard" | Ancient untested code blocking migration; solution = **thorough testing** |
| "Rosie" | Google's LSC platform: shard → test → mail → review → submit |
| "TAP Train" | Batches LSCs, runs union of tests, isolates culprits; **every 3 hours** |
| "Global approver" | Single expert reviewer using **pattern-based tooling** to auto-approve |
| "Cattle vs pets" | LSC shards = **cattle**; handcrafted changes = pets |
| "Preventing backsliding" | **Tricorder** static analysis flags new uses of deprecated systems |

## Related Notes
- [[Dependency Management]]
- [[Continuous Integration]]
- [[Version Control]]
- [[Build Systems]]
- [[Testing Overview]]
- [[Deprecation]]
- [[Software Engineering Fundamentals]]

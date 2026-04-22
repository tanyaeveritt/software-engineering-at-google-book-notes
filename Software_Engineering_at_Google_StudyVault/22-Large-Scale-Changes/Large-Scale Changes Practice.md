---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, large-scale changes, LSC, Rosie, refactoring, TAP Train
---

# Large-Scale Changes Practice (10 questions)

#practice #large-scale-changes

## Related Concepts
- [[Large-Scale Changes]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | LSC definition | Cannot be submitted as **single atomic unit** |
> | Infrastructure teams | Have **domain knowledge**; avoid unfunded mandates |
> | Rosie | Shard → test → mail → review → submit |
> | TAP Train | 5 steps, every **3 hours**, batches changes |
> | Global approver | Pattern-based auto-approve of expected changes |
> | Haunted graveyard | Untested legacy code; solution = **thorough testing** |

---

## Question 1 - LSC Definition [recall]
> What qualifies as a large-scale change (LSC), and what categories do they typically fall into?

> [!answer]- Show Answer
> An LSC is any set of changes that are **logically related but cannot practically be submitted as a single atomic unit** — either because of tooling limits, merge conflicts, or distributed repos. Common categories:
> 1. Cleaning up common **antipatterns** using codebase-wide analysis
> 2. Replacing uses of **deprecated** library features
> 3. Enabling low-level infrastructure improvements (e.g., **compiler upgrades**)
> 4. Moving users from an old system to a **newer one**
> Most LSCs have near-zero functional impact — they are behavior-preserving textual updates.

---

## Question 2 - Why Infrastructure Teams [recall]
> Why does Google have infrastructure teams (not product teams) perform LSCs?

> [!answer]- Show Answer
> Three reasons:
> 1. **Domain expertise**: Infrastructure teams have the knowledge to fix hundreds of thousands of references correctly. Centralization enables faster recovery from errors.
> 2. **No unfunded mandates**: Expecting product teams to organically migrate is globally inefficient and rarely succeeds.
> 3. **Aligned incentives**: Teams that own the systems requiring LSCs have a vested interest in removing the old system. Engineers copy existing code as examples, perpetuating old patterns without centralized removal.

---

## Question 3 - Barriers to Atomic Changes [recall]
> Name four barriers that prevent large changes from being committed atomically.

> [!answer]- Show Answer
> 1. **Technical limitations**: VCS operations scale linearly; large commits exhaust memory or block other writers
> 2. **Merge conflicts**: Probability grows with change size; at Google's scale, someone is always modifying the repo
> 3. **Haunted graveyards**: Ancient, untested, business-critical code that nobody dares modify blocks migration
> 4. **Heterogeneity**: Different CI systems, presubmit checks, formatting, and tooling across projects make automation difficult

---

## Question 4 - TAP Train Process [recall]
> Describe the five steps of the TAP Train for testing LSCs.

> [!answer]- Show Answer
> The TAP Train runs every **3 hours**:
> 1. For each change, run **1,000 randomly-selected** tests as a sample
> 2. Gather passing changes and create one **uber-change** ("the train")
> 3. Run the **union of all directly affected tests** (can be every test at Google; takes 6+ hours)
> 4. For each non-flaky failing test, **rerun individually** against each change to find the culprit
> 5. Generate a **report** for each change describing pass/fail evidence
>
> Key insights: LSCs are narrow refactorings that rarely interact, and individual changes are correct more often than not, so batching is efficient.

---

## Question 5 - Rosie Process [application]
> You need to migrate 100,000 references from an old API to a new one across Google's codebase. Describe the end-to-end process using Rosie.

> [!answer]- Show Answer
> 1. **Authorization**: Submit a brief document explaining the change, estimated impact, and FAQ to the oversight committee for approval
> 2. **Change creation**: Use automated tools (ClangMR, Refaster, etc.) to generate the global change; run automatic formatters
> 3. **Shard management**: Rosie splits the master change into smaller shards based on project boundaries and ownership. Each shard goes through:
>    - **Testing**: Run via TAP/TAP Train against transitively affected tests
>    - **Mailing**: Send to appropriate reviewer (global approver or local owner via owner-detection service)
>    - **Reviewing**: Global approver uses pattern-based tooling to auto-approve expected changes, manually examining only anomalies
>    - **Submitting**: Passes project precommit checks before final commit
> 4. **Cleanup**: Use Tricorder to flag any new uses of the deprecated API at review time to prevent backsliding

---

## Question 6 - Haunted Graveyards [recall]
> What is a "haunted graveyard" in the context of codebases, and how does Google counter it?

> [!answer]- Show Answer
> A haunted graveyard is a system (production or code) so **ancient, obtuse, or complex** that no one dares change it. It's often business-critical and frozen in time, with bureaucratic layers preventing modification. It blocks LSC completion, decommissioning of dependent systems, and compiler/library upgrades.
>
> Google's counter: **thorough testing**. When software is thoroughly tested, you can make arbitrary changes with confidence about whether they break anything, regardless of age or complexity. Writing those tests takes effort but allows the codebase to evolve over long periods.

---

## Question 7 - Global Approver Strategy [application]
> Why does Google use "global approvers" instead of local code owners for LSC reviews, and how does this scale?

> [!answer]- Show Answer
> Local owners often don't treat LSCs with the same rigor as regular changes — they trust infrastructure teams, so review is usually cursory anyway. Global approvers are more efficient because they:
> - Have **specific knowledge** of the language/library being changed
> - Work with LSC authors to know **what to expect** and what failure modes exist
> - Use **pattern-based tooling** to automatically approve changes matching expectations
> - Manually examine only **anomalies** (merge conflicts, tooling malfunctions)
>
> This allows one person to review thousands of shards efficiently, scaling sublinearly with codebase size.

---

## Question 8 - scoped_ptr Migration [recall]
> Describe the scoped_ptr to std::unique_ptr migration at Google.

> [!answer]- Show Answer
> Google's codebase had **500,000+ references** to scoped_ptr. std::unique_ptr (C++11) was strictly better. Over several months, engineers:
> - Used LSC infrastructure to change references and adapt scoped_ptr to behave more like std::unique_ptr
> - At peak: generated, tested, and committed **700+ independent changes** touching **15,000+ files per day**
> - The long tail involved Hyrum's Law behavior dependencies, race conditions with other engineers, and generated code
> - Final strategy: first made scoped_ptr a **type alias** of std::unique_ptr, then did textual substitution, then removed the alias

---

## Question 9 - Language Impact on LSCs [analysis]
> Why are statically typed languages easier for large-scale changes than dynamically typed ones? What infrastructure is critical regardless of language?

> [!answer]- Show Answer
> Statically typed languages provide:
> - **Compiler-based tools** and strong static analysis that give significant information about code structure
> - Ability to **reject invalid transformations** before testing phase
> - Features like **type aliasing and forwarding functions** for incremental migration
>
> Dynamic languages (Python, Ruby, JavaScript) lack these guarantees, making automated changes extra difficult and risky.
>
> Critical infrastructure regardless of language: **automatic formatters** (google-java-format, clang-format). Generated changes must look human-written for readability. Without automatic formatting, LSCs would never have become accepted at Google.

---

## Question 10 - Operation RoseHub [analysis]
> Compare Operation RoseHub (the Mad Gadget vulnerability fix) with Google's typical automated LSC process. What does it reveal about LSCs beyond Google's monorepo?

> [!answer]- Show Answer
> **Typical LSC**: Automated tooling generates changes, Rosie shards/tests/reviews/submits, a few dozen engineers support tens of thousands. Relies on monorepo, unified CI (TAP), and single ownership model.
>
> **Operation RoseHub**: Apache Commons vulnerability (Mad Gadget) affected thousands of open-source projects on GitHub. **50+ human volunteers** manually identified affected projects using BigQuery and sent **2,600 patches**. No automated pipeline — humans replaced Rosie.
>
> Key insight: The LSC **process** (identify → change → test → review → submit) generalizes beyond Google's infrastructure. The difference is the **tooling and automation** that makes it scalable. For federated/distributed repos without unified CI, human effort can substitute, but at much higher cost. It also shows that LSC thinking has implications for the **broader OSS ecosystem**, not just monorepos.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | LSC | Logically related, **can't commit atomically** |
> | Infrastructure teams | Domain expertise + avoid **unfunded mandates** |
> | Rosie | Shard → test → mail → review → submit pipeline |
> | TAP Train | Batch LSCs, 5 steps, **every 3 hours** |
> | Global approver | **Pattern-based** auto-approve; manual review only for anomalies |
> | Haunted graveyard | Solution = **thorough testing** |
> | scoped_ptr migration | 500K+ refs; type alias strategy; **700+ changes/day** at peak |
> | Cattle vs Pets | LSC shards = **cattle**; individual changes = pets |
> | Static vs dynamic | Static types = easier for LSCs; **formatters** critical for all |

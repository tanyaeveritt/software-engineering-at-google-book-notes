---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, Critique, code review tool, LGTM, scoring, attention set
---

# Critique Tool Practice (10 questions)

#practice #code-review #tools

## Related Concepts
- [[Critique Tool]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Four principles | Simplicity, trust, generic communication, workflow integration |
> | Scoring model | LGTM + Approval + no unresolved comments |
> | Attention set | Tracks whose turn it is; bold names in UI |
> | Analyzer display | Tricorder: gray comment boxes on diff |
> | Gerrit | Open-source; Git-based; -2 veto; plug-in system |
> | Atomic comments | Draft individually, publish all at once |

---

## Question 1 - Four Guiding Principles [recall]
> List and briefly explain the four guiding principles of Critique's design.

> [!answer]- Show Answer
> 1. **Simplicity**: fast-loading UI, minimal unnecessary choices, clear visual markers for review state. The team rejected many features to keep the model simple.
> 2. **Foundation of trust**: code review empowers, not slows. Authors are trusted to address minor comments without additional review rounds. Changes are openly accessible across Google.
> 3. **Generic communication**: free-form comments are preferred over complicated protocols. Users spell out what they want instead of relying on complex data models.
> 4. **Workflow integration**: touchpoints with Cider (editor), Code Search, Tricorder (analysis), Rapid (releases), Zapfhahn (coverage). Critique links to these tools but keeps review as the primary focus.

---

## Question 2 - Three-Part Scoring [recall]
> Describe Critique's three-part scoring model and the conditions for a change to be committable.

> [!answer]- Show Answer
> 1. **LGTM** ("Looks Good To Me"): a reviewer stamps meaning "I've reviewed this, it meets our standards, OK to commit after addressing unresolved comments."
> 2. **Approval**: a gatekeeper (code owner) stamps meaning "I allow this change into the codebase."
> 3. **Unresolved comments**: count of comments requiring author action (soft requirement; author can mark as resolved).
> A change is committable when it has: **at least one LGTM** + **sufficient approvals** + **no unresolved comments**. This is shown prominently as a green page header. Every change requires LGTM regardless of approval status, ensuring at least two pairs of eyes.

---

## Question 3 - Attention Set [recall]
> What is the "attention set" feature and why was it so impactful?

> [!answer]- Show Answer
> The **attention set** is the set of people on which a change is currently blocked. When a reviewer or author is in the attention set, they are expected to respond in a timely manner. Relevant usernames are rendered in **bold** in the UI. It auto-updates when comments are published but can be manually managed. After implementation, users said: **"how did we get along without this?"** Previously, reviewers and authors had to chat out-of-band to understand who was dealing with a change. The feature emphasizes the **turn-based nature** of code review and becomes even more valuable with multiple reviewers.

---

## Question 4 - Why No Thumbs-Down [recall]
> Critique removed the "Needs More Work" rating. Why, and what replaced it?

> [!answer]- Show Answer
> Critique deliberately made LGTM/Approval **always positive**. Reviewers cannot thumbs-down a change with no useful feedback. All negative feedback must be tied to **specific unresolved comments**. If a change definitely needs more work, primary reviewers add comments without granting LGTM/Approval. This had a positive influence on code review culture: every piece of criticism is actionable and tied to something specific. The phrasing "unresolved comment" was also chosen to sound relatively non-confrontational.

---

## Question 5 - Atomic Comment Publishing [recall]
> Why are comments in Critique published atomically rather than individually as they are written?

> [!answer]- Show Answer
> Comments are drafted as-you-go but published **atomically** (all at once). This allows reviewers to: (1) provide a **complete, coherent thought** on the entire change rather than piecemeal feedback; (2) ensure they are happy with all comments before the author sees them; (3) avoid confusing the author with partial feedback while the reviewer is still forming their opinion. This supports the principle of generic communication and reduces back-and-forth misunderstandings.

---

## Question 6 - Diff Optimization [recall]
> Name three techniques Critique uses to optimize the diff viewing experience.

> [!answer]- Show Answer
> 1. **Intraline diffing**: shows character-level differences factoring in word boundaries, not just line-level.
> 2. **Move detection**: chunks of code moved between locations are marked as "moved" rather than showing separate add/remove.
> 3. **Snapshot version chain widget**: a compact UI showing the chain of snapshots; users drag-and-drop to select which versions to compare. Auto-collapses similar snapshots, highlighting important ones (test coverage, reviewed, comments). All snapshots are **prefetched** for instant loading.
> Additional: syntax highlighting, Kythe cross-references, side-by-side view with no padding, and keyboard shortcuts for navigation.

---

## Question 7 - Analyzer Integration [application]
> You're building a code review tool for your company. Describe how you would integrate static analysis results, drawing on Critique's approach.

> [!answer]- Show Answer
> Based on Critique's Tricorder integration: (1) Display analyzer results as **distinct visual elements** (e.g., gray comment boxes) on the diff, clearly distinguishable from human comments. (2) Show actionable/non-actionable as a **binary option** (highlighted in red for critical findings). (3) Include **"Please fix" button** allowing reviewers to convert findings into unresolved comments. (4) Provide **suggested fixes** that can be previewed and applied with one click. (5) Show analyzer status chips (in-progress = yellow, complete = gray) below the change description. (6) Run analyzers on snapshot upload (not just at commit time) so results are available before review starts. This saves reviewer time by catching mechanical issues automatically.

---

## Question 8 - Critique vs Gerrit [application]
> A team managing both internal monorepo code and open-source projects needs to choose code review tooling. How would you advise them, based on Google's approach?

> [!answer]- Show Answer
> Use **Critique** (or similar internal tool) for monorepo code, and **Gerrit** for open-source projects. This mirrors Google's approach: Critique has tight interdependencies with the monorepo and internal tools, making it unsuitable for external projects. Gerrit is standalone, open-source, and tightly integrated with **Git**. Key Gerrit advantages for OSS: (1) fine-grained permission model for restricting access; (2) rich plug-in system for custom environments; (3) supports stacking commits and atomic chain commits; (4) more sophisticated scoring (including -2 veto). Both tools share the same fundamental model: each commit is reviewed separately.

---

## Question 9 - Trust and Time Zones [analysis]
> The book states that Critique's trust model is "particularly important for saving time when there is a significant time zone difference." Analyze why.

> [!answer]- Show Answer
> When author and reviewer are in different time zones, each round-trip in the review process can add **24 hours of delay**. Critique's trust model mitigates this by allowing a reviewer to grant **LGTM with unresolved comments** — trusting the author to address them without requiring another review cycle. The author can mark comments as "resolved" and commit. Without this trust model, the reviewer would need to re-review after each set of fixes, potentially adding multiple days to a simple change. Additionally, unresolved comments being "soft requirements" means the author can use judgment about when a comment is adequately addressed, rather than needing explicit re-approval. This trust reduces round-trips from potentially 3-4 (with 24h each) to as few as 1.

---

## Question 10 - Simplicity vs Features [analysis]
> Critique's team considered building a "Code Central" tool combining editing, reviewing, and searching. They decided against it. Analyze the reasoning behind this decision using the principles from the chapter.

> [!answer]- Show Answer
> The decision aligns with the **simplicity** principle: combining editing, reviewing, and searching into one tool would make the data model and UI more complex, serving a small set of power users at the cost of the majority. Code review is a distinct activity with specific cognitive requirements (understanding changes, providing feedback), and mixing it with editing creates distraction. The **workflow integration** principle offers the alternative: link to specialized tools (Cider for editing, Code Search for browsing) rather than embed them. This separation of concerns means each tool can be optimized for its primary use case — Code Search for browsing/understanding, Cider for editing, and Critique for reviewing. The team found that simplicity, with a tension against integration, should favor keeping the primary focus clear. Features are linked from Critique but implemented in separate subsystems.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Four principles | Simplicity, trust, generic communication, workflow integration |
> | Scoring | LGTM + Approval + no unresolved comments = committable |
> | Attention set | Whose turn to act; bold names; turn-based review |
> | No thumbs-down | Negative feedback requires specific unresolved comments |
> | Atomic publishing | Draft individually, publish all at once for complete thought |
> | Analyzer integration | Gray comment boxes; "Please fix" button; suggested fixes |
> | Gerrit | Open-source; Git; -2 veto; plug-ins; used for OSS projects |
> | Trust + time zones | LGTM with comments saves 24h round-trips |
> | Simplicity over features | Link to specialized tools; don't build "Code Central" |

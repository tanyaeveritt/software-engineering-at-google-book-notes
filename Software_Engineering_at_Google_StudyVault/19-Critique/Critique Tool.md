---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: Critique, code review, code review tool, LGTM, attention set, diffing, Gerrit, presubmits
---

# Critique: Google's Code Review Tool (Importance: ★★)

#code-review #tools

*Reading time: ~4 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| What Is Critique | Google's primary code review tool; supports the full review lifecycle |
| Guiding Principles | Simplicity, trust, generic communication, workflow integration |
| Scoring Model | LGTM (quality) + Approval (gatekeeper) + Unresolved Comments |
| Attention Set | Tracks whose turn it is to act; renders names in bold |
| Diffing | Syntax highlighting, intraline diffs, move detection, cross-references (Kythe) |
| Analyzers | Static analysis results (Tricorder) shown as gray comment boxes on diffs |
| Integration | Links to Cider (editor), Code Search, Tricorder, Rapid (release), Zapfhahn (coverage) |
| Gerrit | Open-source alternative used for Chrome/Android; Git-integrated; richer scoring |

## Code Review Tooling Principles
Critique was shaped by four guiding principles:
- **Simplicity**: Easy to use, fast-loading UI, minimal unnecessary choices, clear visual markers for review state
- **Foundation of trust**: Code review empowers others, not slows them down. Authors trusted to address minor comments without extra review rounds. Changes are openly accessible across Google.
- **Generic communication**: Prioritize free-form comments over complex protocols. Critique encourages users to spell out what they want rather than making the data model more complex.
- **Workflow integration**: Touchpoints with Cider (IDE), Code Search, Tricorder, Rapid (releases), Zapfhahn (test coverage). Links to tools rather than embedding them.

## The Six-Stage Code Review Flow

### Stage 1: Create a Change
- Author creates a change, uploads a **snapshot** to Critique
- Triggers automatic **code analyzers** (Tricorder)
- Author sees diff as reviewer would, preventing misunderstandings
- Supports linking to bugs, suggesting reviewers, and preliminary comments

### Stage 2: Request Review
- Author sends change to reviewers
- **GwsQ** tool assigns specific reviewers from team aliases based on ownership, familiarity, availability, and timezone
- **Presubmits** (precommit hooks) run: automated tests, email list additions, project-specific invariants

### Stages 3 & 4: Comment and Iterate
- Reviewers draft comments on the diff; comments are **published atomically** (complete thought at once)
- Comments are **unresolved** (must address) or **resolved** (optional/informational)
- Anyone can comment ("drive-by review")
- Per-person state: checkboxes to track which files have been reviewed
- "**Please fix**" button on analyzer findings creates unresolved comment
- Reviewers can suggest inline edits that become comments with attached fixes
- Shortcuts: "**Done**" (addressed) and "**Ack**" (acknowledged) buttons

### Stage 5: Change Approval (Scoring)
Three-part scoring system:
- **LGTM** ("Looks Good To Me"): "I've reviewed this and believe it meets our standards"
- **Approval**: "As gatekeeper, I allow this to be committed" (ownership-based)
- **Unresolved comments**: soft requirements; author can mark as resolved

A change is ready when: at least one LGTM + sufficient approvals + no unresolved comments. Shown as **green page header**.
- LGTM/Approval are always positive (no "thumbs down"); negative feedback requires specific unresolved comments
- Reviewers can grant LGTM with unresolved comments, trusting the author to fix them (important for cross-timezone collaboration)
- Every change requires at least **two pairs of eyes** (LGTM regardless of approval status)

### Stage 6: Commit
- One-click commit button from Critique (avoids context-switching to CLI)
- Presubmits run again before final commit
- Emergency: author can force-commit and get reviewed after

## Key Features

### Diffing
Optimized for understanding code changes:
- Syntax highlighting, Kythe cross-references
- **Intraline diffing**: character-level differences respecting word boundaries
- **Move detection**: chunks moved between locations are marked as moves (not add/remove)
- Side-by-side diff view with minimal padding
- Snapshot version chain widget with drag-and-drop comparison
- Prefetching ensures instant loading of different snapshots

### Attention Set ("Whose Turn")
Tracks the set of people currently **blocking** a change. Renders relevant usernames in bold. Auto-updated when comments are published, but manually adjustable.
- Emphasizes the **turn-based** nature of code review
- Prevailing opinion after implementation: "how did we get along without this?"
- Before this feature, reviewers/authors had to chat out-of-band to understand who should act

### Dashboard
- User-customizable sections, each powered by **Changelist Search** queries
- Default first section: changes needing user's attention
- Changelist Search indexes the latest state of all changes (pre and post-submit) across all Google users

### After Commit: Tracking History
- Critique serves as **code archaeology** tool: view past history, comments, evolution of changes
- Supports post-commit comments (for later-discovered problems or additional context)
- Rollback support: see if a change has been rolled back

## Notifications
Critique publishes event notifications consumed by:
- **Chrome extension**: desktop notifications when action needed
- **Email**: important events trigger emails; email replies translated to comments
- Other supporting tools (separation of concerns)

## Gerrit: Open-Source Alternative
Used by Google teams working on open source (Chrome, Android) or projects outside the monorepo.
- Standalone, open source, tightly integrated with **Git**
- Supports stacking commits for individual review + atomic commit of chains
- More sophisticated scoring (reviewer can veto with -2 score)
- Rich **plug-in system** for custom environments
- Available at gerritcodereview.com

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "four principles" | **Simplicity, trust, generic communication, workflow integration** |
| "LGTM vs Approval" | LGTM = quality review; Approval = gatekeeper (ownership) permission |
| "scoring model" | LGTM + Approval + no unresolved comments = green (ready to commit) |
| "attention set" | Tracks whose turn to act; bold usernames; turn-based review |
| "analyzer integration" | Tricorder results shown as gray comment boxes on diff |
| "why no thumbs-down" | All negative feedback must be tied to specific unresolved comments |
| "trust in scoring" | Reviewer can LGTM with unresolved comments, trusting author to fix |
| "Gerrit" | Open-source alternative; Git-integrated; richer scoring; plug-in system |
| "atomic comment publishing" | Comments drafted individually but published all at once |

## Related Notes
- [[Code Review]]
- [[Static Analysis]]
- [[Code Search]]
- [[Version Control]]
- [[Continuous Integration]]
- [[Large-Scale Changes]]
- [[Style Guides and Rules]]

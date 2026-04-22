---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: static analysis, Tricorder, Error Prone, false positives, developer happiness, code review integration
---

# Static Analysis (Importance: ★★★)

#static-analysis #tools

*Reading time: ~6 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Definition | Analyzing source code without executing it to find bugs, antipatterns, and issues |
| Key Platform | **Tricorder** — Google's static analysis platform integrated with Critique |
| Three Key Lessons | Focus on developer happiness; integrate into core workflow; empower users to contribute |
| Effective False Positive | User-perceived false positive: developer took no positive action |
| Integration Points | Code review (primary), compiler, presubmits, IDE, code browsing |
| Tricorder Scale | 50,000+ changes/day; 100+ analyzers; <5% effective false-positive rate |
| Error Prone | Extends Java compiler to identify AST antipatterns |
| Compiler Checks | No warnings; either error (break the build) or don't show |
| Feedback Loop | "Not useful" button on findings; analyzers disabled if not improved |
| Per-Project Customization | Project-level (not user-level) to ensure team consistency |

## What Is Static Analysis
Static analysis examines **source code** to find potential issues without executing the program ("static" vs "dynamic" analysis of running programs). At Google, it serves multiple purposes:
- Find **bugs** early (before production): constant expression overflow, invalid format strings, tests never run
- Codify **best practices** and keep code current with modern APIs
- **Prevent technical debt** and assist in API deprecation (prevent backsliding)
- **Educate** developers: checks can actually prevent antipatterns from entering the codebase

## Characteristics of Effective Static Analysis

### Scalability
Analysis tools must scale to Google's multi-billion-line codebase:
- Tools are **shardable and incremental**: analyze files affected by a pending change, not entire projects
- Show results only for **edited files or lines** (newly introduced warnings)
- Scale the number and variety of analyses by soliciting contributions from across the company

### Usability (Cost-Benefit Trade-off)
- Fixing a warning could introduce a bug (e.g., calling previously dead code)
- Focus on **newly introduced warnings**, not existing issues in working code
- Anything fixable automatically **should be fixed automatically**
- Show only issues with **negative impact on code quality** to avoid wasting developer time
- Smooth workflow integration reduces cost of reviewing results

## Three Key Lessons

### 1. Focus on Developer Happiness
- Deploy only tools with **low false-positive rates**
- Track tool performance; actively solicit and act on real-time developer feedback
- The **"effective false positive"** rate matters: if a developer takes no positive action after seeing the issue, it counts as an effective false positive, even if technically correct
- Conversely, if a developer happily fixes a technically incorrect report (e.g., improves readability), that is NOT an effective false positive
- **User trust** is critical: nurturing the feedback loop creates a virtuous cycle

### 2. Make Static Analysis Part of the Core Developer Workflow
Primary integration point is **code review** (Critique):
- Developers are already in a "change mindset" when sending code for review
- Time for analyses to run while authors context-switch after sending for review
- **Peer pressure** from reviewers to address warnings
- Static analysis helps reviewers **scale**: highlights common issues automatically
- Code review is the **sweet spot** for analysis results

### 3. Empower Users to Contribute
- Domain experts write new analyzers or checks within existing tools
- Developers who discover bugs write analyzers to prevent the same class of bug elsewhere
- Simple APIs (e.g., **Refaster**: specify pre/post code snippets to define an analyzer)
- Focus on building an **ecosystem** that is easy to plug into, not a small set of tools

## Tricorder: Google's Static Analysis Platform

### Architecture and Scale
- **Microservices architecture**: sends analyze requests to servers with change metadata
- Servers access source files via FUSE-based filesystem and cached build I/O
- Analyzes **50,000+ code review changes per day**; often several analyses per second
- Results displayed as **gray comment boxes** in Critique's diff viewer
- Overall effective false-positive rate: **just below 5%**

### Four Criteria for New Checks
| Criterion | Requirement |
|-----------|-------------|
| **Understandable** | Any engineer can understand the output |
| **Actionable and easy to fix** | Guidance on how to fix; may require more thought than a compiler check |
| **<10% effective false positives** | Developers should find the check useful at least 90% of the time |
| **Significant impact on code quality** | Developers should take the issues seriously |

### Integrated Tools
- **Error Prone** (Java) and **clang-tidy** (C++): extend compiler to find AST antipatterns
- **Deleted Artifact Analyzer**: warns if a source file referenced by docs/non-code is deleted
- **IfThisThenThat**: portions of two files must change in tandem
- **Finch**: Chrome A/B experiment configuration analysis (makes RPCs to check experiment conflicts)
- **Binary size checker**: warns when changes significantly affect binary size
- Most analyzers are **intraprocedural** (within a single function)

### Integrated Feedback Channels
- **"Not useful" button** on each finding: files a bug against the analyzer writer with prepopulated info
- **"Please fix" button** for reviewers to convert findings into unresolved comments
- Tricorder team tracks analyzers with high "Not useful" rates and will **disable** underperforming ones
- Example: Error Prone check message updated from confusing to clear, stopping a flood of false "Not useful" reports

### Suggested Fixes
- Many Tricorder checks provide **automated fixes** shown in Critique
- Fixes can be applied directly from Critique or via command-line tool
- Style issues should be fixed automatically (formatters)
- Reviewers click "Please Fix" **thousands of times per day**; authors apply fixes **~3,000 times per day**

### Per-Project Customization
- **Project-level** customization (not user-level) ensures team consistency
- Optional analyzers for specific project needs (e.g., Proto Best Practices for projects with stored serialized data)
- User-level customization was removed because it hid bugs and suppressed feedback

## Compiler Integration
When possible, push analysis into the **compiler** (breaking the build is impossible to ignore):
- Error Prone "ERROR" checks enabled in Google's Java compiler
- **Three criteria**: actionable + no effective false positives + correctness only (not style)
- Must clean up all existing instances before enabling (MapReduce over entire codebase)
- **No compiler warnings**: either error or don't show. Developers ignore warnings.
- Java and C++ compilers configured to avoid displaying warnings
- Go takes this further: unused variables/imports are compilation errors

## Presubmit Integration
Analysis can block commits (**presubmit checks**):
- Simple checks: commit message rules ("DO NOT SUBMIT"), test file inclusion
- Format checks: well-formatted code required
- Team-specific presubmits: enforce higher best-practice standards
- Run at review time and again at commit time

## Analysis in IDE and Code Browsing
- IDE integration requires **<1 second** (ideally <100ms) analysis time
- Challenge: ensuring same analysis across multiple IDEs
- Code review has advantages: full change context, peer accountability
- **Code browsing**: some teams (e.g., security) want holistic views of all instances across the codebase

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "three key lessons" | **Developer happiness, core workflow integration, empower user contributions** |
| "effective false positive" | Developer took no positive action after seeing the issue |
| "Tricorder" | Google's static analysis platform; integrated with Critique; <5% false-positive rate |
| "primary integration point" | **Code review** (Critique); developers already in change mindset |
| "four Tricorder criteria" | Understandable, actionable, <10% false positives, significant impact |
| "Error Prone" | Extends Java compiler for AST antipattern detection |
| "no compiler warnings" | Either error (break build) or don't show; developers ignore warnings |
| "Not useful button" | Feedback channel; tracks analyzer quality; disables poor analyzers |
| "project vs user customization" | Project-level only; user-level hid bugs and suppressed feedback |
| "Refaster" | Write analyzers by specifying pre/post code snippets |

## Related Notes
- [[Critique Tool]]
- [[Code Review]]
- [[Code Search]]
- [[Build Systems]]
- [[Continuous Integration]]
- [[Large-Scale Changes]]
- [[Style Guides and Rules]]

---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: style guides, rules, consistency, code formatting, readability, tooling
---

# Style Guides and Rules (Importance: ★★★)

#style-guide #processes

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Rules vs Guidance | Rules are law (mandatory); guidance is "shoulds" (recommendations) |
| Guiding Principles | Pull weight, optimize for reader, be consistent, avoid danger, concede to practicalities |
| Three Rule Categories | Avoid dangers, enforce best practices, ensure consistency |
| Style Arbiters | Senior language experts who make final decisions by consensus |
| Changing Rules | Solution-based proposals backed by evidence from existing code |
| Enforcement | Automate with tooling (clang-tidy, Error Prone, formatters) wherever possible |

## Why Have Rules?
Rules encourage "good" behavior and discourage "bad" behavior. What counts as "good" is **organization-specific** — it depends on what the organization values.
- Rules shape a **common vocabulary** of coding, letting engineers focus on what code says rather than how it says it
- Engineers tend to do the "good" things **by default**, even subconsciously, when rules are internalized
- Google maintains **separate style guides per language**, varying in scope and length

## Guiding Principles for Creating Rules
Five overarching principles guide Google's style guide development:

### 1. Rules Must Pull Their Weight
Not everything belongs in a style guide. Every new rule imposes a **nonzero cost** — engineers must learn and remember it, new engineers must onboard to it. If only one or two engineers get something wrong, adding a rule doesn't scale.
- Google deliberately excludes **self-evident** rules (e.g., no rule against `goto` in C++)

### 2. Optimize for the Reader
Code is read **far more** than it is written. Prefer tedious-to-type over difficult-to-read.
- Require explicit evidence of intended behavior (e.g., `override` keyword in Java/C++)
- **Local reasoning**: readers should understand code at the call site without needing to find the function's implementation
- Documentation and implementation comments support this goal

### 3. Be Consistent
Consistency enables **expert chunking** (cognitive grouping), easier tooling, cross-project mobility, and resilience to time.
- Hierarchy of consistency: **file > team > project > codebase**
- Sometimes defer to **external standards** (e.g., Python switching from 2-space to 4-space indent to match PEP 8)
- At massive scale, **perfect consistency** may be too costly — focus on current best practices

### 4. Avoid Error-Prone and Surprising Constructs
Restrict language features that are tricky, have subtle pitfalls, or can introduce hard-to-find bugs.
- Example: Python style guide restricts **reflection** (`hasattr()`, `getattr()`) because it obfuscates attribute access and can cause security flaws
- Prefer **straightforward code** over clever power features — SREs debugging production outages need to understand any code quickly

### 5. Concede to Practicalities
Allow exceptions for **performance** (e.g., C++ `noexcept`), **interoperability** (e.g., `snake_case` for standard library mimics), and **generated code** (often out of scope).

## Three Categories of Style Guide Rules
- **Avoiding danger**: rules about static members, lambdas, exceptions, threading, access control, class inheritance
- **Enforcing best practices**: commenting rules, source file structure, naming conventions, formatting, limitations on new language features
- **Building in consistency**: naming conventions, indentation, import ordering — the choice itself matters more than which choice

## Changing the Rules
Style guides are **not static**. Google has a process for updates:
- Changes are **solution-based**: identify a proven problem in existing code, then propose a fix
- Community discussion via **language-specific mailing lists**
- **Style arbiters** (senior language experts) make final decisions by **consensus**, not voting
- Decisions are based on **engineering trade-offs**, not personal preference

### Case Studies
- **std::unique_ptr**: Initially disallowed in C++ due to unfamiliar move semantics. Later adopted because ownership clarity was a significant long-term benefit.
- **CamelCase → snake_case in Python**: Originally CamelCase for C++ consistency. Changed to snake_case as Python became its own ecosystem and external consistency became more valuable.

## Guidance (Beyond Rules)
In addition to rules, Google maintains:
- **Language primers**: descriptive (not prescriptive) explanations of endorsed features
- **"Tip of the Week"** series: narrowly focused advice from real problems (e.g., C++ tips on object lifetime, move semantics)
- **"Language@Google 101"** courses: full-day courses on Google-specific idioms and tooling

## Applying and Enforcing Rules
- **Social enforcement**: readability process (code review mentorship for new Googlers)
- **Automated enforcement** (preferred): removes reliance on memory, minimizes interpretation variance, scales with org size
- **Error checkers**: clang-tidy (C++), Error Prone (Java) — ~90% of C++ style guide rules can be auto-verified
- **Code formatters**: clang-format, yapf (Python), gofmt (Go), dartfmt (Dart), buildifier (BUILD files)
- **Presubmit checks**: code rejected if formatter produces diffs; eliminates formatting debates in code review

### Case Study: gofmt
Go shipped with `gofmt` from day one. No configuration knobs. All Go code worldwide is formatted identically. Engineers initially complained but now cite it as a reason they like Go. Enabled tools like `gofix` to automatically update code with clean diffs.

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Rules vs guidance" | **Rules are mandatory law; guidance is recommended best practices** |
| "Optimize for the reader" | **Code is read more than written; prefer readable over writable** |
| "Expert chunking" | **Consistency enables cognitive grouping of familiar patterns** |
| "Style arbiters" | **Senior language experts; decide by consensus, not vote** |
| "Streetlight effect in rules" | **Rule changes must be evidence-based, not preference-based** |
| "gofmt impact" | **Shipped day one, no config, universal Go format; enabled tooling** |
| "Pull their weight" | **Don't add rules for self-evident things; nonzero cost per rule** |
| "Hierarchy of consistency" | **File > team > project > codebase** |

## Related Notes
- [[Code Review]]
- [[Documentation]]
- [[Measuring Productivity]]
- [[Knowledge Sharing]]
- [[Software Engineering Fundamentals]]

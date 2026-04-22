---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, style guides, rules, consistency, formatting, tooling
---

# Style Guides and Rules Practice (10 questions)

#practice #style-guide #processes

## Related Concepts
- [[Style Guides and Rules]]
- [[Code Review]]
- [[Documentation]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Rules vs guidance | **Rules are law; guidance is recommendations** |
> | Five guiding principles | **Pull weight, optimize reader, consistency, avoid danger, concede practicalities** |
> | Style arbiters | **Senior experts; consensus, not voting** |
> | Three rule categories | **Avoid danger, enforce best practices, ensure consistency** |
> | gofmt | **Standard Go formatter; no config; shipped day one** |

---

## Question 1 - Rules vs Guidance [recall]
> What is the difference between "rules" and "guidance" in Google's style guides?

> [!answer]- Show Answer
> **Rules** are mandatory laws — they are universally enforceable and may not be disregarded except through an approved waiver process. **Guidance** provides recommendations and best practices — highly advisable to follow but with room for variance. Rules are the "musts"; guidance is the "shoulds."

---

## Question 2 - Five Guiding Principles [recall]
> List the five guiding principles for creating rules at Google.

> [!answer]- Show Answer
> 1. **Rules must pull their weight** — every rule has a nonzero cost; don't add self-evident rules
> 2. **Optimize for the reader** — code is read far more than written; prefer readable over writable
> 3. **Be consistent** — enables expert chunking, tooling, mobility, and resilience to time
> 4. **Avoid error-prone and surprising constructs** — restrict tricky features with subtle pitfalls
> 5. **Concede to practicalities** — allow exceptions for performance, interoperability, and generated code

---

## Question 3 - Three Rule Categories [recall]
> What are the three categories of rules in Google's style guides?

> [!answer]- Show Answer
> 1. **Rules to avoid dangers** — technical rules about language features (static members, lambdas, exceptions, threading, etc.)
> 2. **Rules to enforce best practices** — commenting conventions, file structure, naming, formatting, limitations on new features
> 3. **Rules to ensure consistency** — naming conventions, indentation, import ordering; the choice itself matters more than which specific choice is made

---

## Question 4 - Consistency Hierarchy [recall]
> What is the hierarchy of consistency that Google uses when local conventions conflict with global ones?

> [!answer]- Show Answer
> **File > Team > Project > Codebase**. "Be consistent" starts locally: norms within a given file precede those of a given team, which precede those of the larger project, which precede those of the overall codebase. The style guides contain rules that explicitly defer to local conventions, valuing local consistency over a scientific technical choice.

---

## Question 5 - Style Arbiters [recall]
> Who are the "style arbiters" and how do they make decisions?

> [!answer]- Show Answer
> Style arbiters are **senior, long-time language experts** who own each language's style guide. They make decisions by **consensus** (not voting) based on **engineering trade-offs**, not personal preference. For example, the C++ style arbiter group has four members — an even number works fine because decisions are trade-off evaluations, not majority votes.

---

## Question 6 - Automation Benefits [recall]
> Why does Google prefer automated enforcement of style guide rules over human enforcement?

> [!answer]- Show Answer
> Three key reasons:
> 1. **Rules aren't dropped or forgotten** as time passes or the organization scales
> 2. **Minimizes variance** in interpretation — a single unchanging definition replaces subjective human bias
> 3. **Scales efficiently** — a single team writes tools the whole company uses; doubling org size doesn't double enforcement cost
>
> Tools like clang-tidy, Error Prone, and code formatters with presubmit checks automate ~90% of C++ style guide rules.

---

## Question 7 - std::unique_ptr Case [recall]
> Why was std::unique_ptr initially disallowed and later permitted in Google's C++ style guide?

> [!answer]- Show Answer
> **Initially disallowed** because the behavior was unfamiliar to most engineers, and the related move semantics were new and confusing. The safer choice was to prevent introduction while engineers adjusted.
>
> **Later permitted** because the ownership information that unique_ptr facilitates at function call sites makes code **far easier to read**. The significant improvement in long-term codebase clarity made it a worthwhile trade-off despite the added complexity of move semantics.

---

## Question 8 - Python Naming Convention [application]
> Your company uses CamelCase for Python method names to match your C++ codebase. Python engineers complain about friction with external libraries and open source contributions. Using the chapter's reasoning, what would you recommend?

> [!answer]- Show Answer
> Recommend **switching to snake_case** for Python, following Google's own experience:
> - As Python usage grows independent of C++, cross-language consistency becomes less valuable than **ecosystem consistency**
> - Engineers reading/writing Python interact with external Python code far more often than C++ code
> - Maintaining a non-standard convention creates friction when incorporating **third-party libraries** and when **open-sourcing** internal code
> - Allow the change as a **file-wide choice** with exemptions for existing code, giving projects latitude to decide timing
> - This mirrors Google's own CamelCase → snake_case case study, where they weighed the cost of internal reeducation against the benefit of external consistency.

---

## Question 9 - "Pull Their Weight" Trade-off [analysis]
> A new engineer proposes adding 15 detailed rules about error handling patterns to your team's style guide, including rules about things most experienced engineers already do correctly. Analyze this proposal using the "pull their weight" principle.

> [!answer]- Show Answer
> The "pull their weight" principle states that every rule has a **nonzero cost**: engineers must learn, remember, and comply with each rule, and new hires face a steeper onboarding curve. Adding 15 rules imposes significant cognitive overhead.
>
> **Analysis**:
> - If most experienced engineers already handle these correctly, the rules are targeting **self-evident behavior** — Google deliberately excludes such rules (e.g., no rule against `goto` in C++)
> - If only 1-2 engineers get something wrong, adding rules for all engineers **doesn't scale**
> - Better alternatives: address the few violations through **code review feedback** (social enforcement) or add automated checks for the most critical patterns only
> - Consider promoting these as **guidance** (recommendations) rather than rules (laws), which carries lower compliance cost
> - A smaller, focused set of rules about the most dangerous or non-obvious error handling patterns would be more effective.

---

## Question 10 - gofmt Philosophy [analysis]
> Go shipped with gofmt (a mandatory code formatter with no configuration options) from its first public release. Analyze why this "no choice" approach was more successful than letting teams choose their own formatting conventions.

> [!answer]- Show Answer
> The "no choice" approach succeeds for several interconnected reasons:
>
> **Eliminates formatting debates**: Time spent arguing about formatting in code reviews is wasted. With gofmt, formatting is a non-issue — it's automated away entirely. At Google's scale (30,000+ engineers), even small debates multiply to enormous costs.
>
> **Enables tooling at scale**: When all code is formatted identically, machine-edited code is indistinguishable from human-edited code. This enabled tools like `gofix` to automatically update pre-1.0 code with **clean diffs** that showed only meaningful changes, not formatting noise.
>
> **Network effects grow over time**: Initially engineers complained, but as adoption grew, the universal format became a **reason people like Go**. Thousands of open source packages can read/write Go code because all editors and IDEs agree on the format, making tools portable across environments.
>
> **Retrofitting is nearly impossible**: If you don't standardize from day one, the cost of retrofitting grows with the codebase. Google's experience retrofitting BUILD files (6 weeks for one engineer across 200,000 files) shows the scale of this challenge even with good tooling.
>
> The key insight: for formatting rules, **the choice itself doesn't matter** — what matters is that a **single, consistent choice exists**. This is the "building in consistency" principle in action.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Rules vs guidance | **Rules = law; guidance = recommendations** |
> | Five principles | **Pull weight, reader, consistency, avoid danger, practicalities** |
> | Three categories | **Avoid danger, best practices, consistency** |
> | Consistency hierarchy | **File > team > project > codebase** |
> | Style arbiters | **Senior experts; consensus on trade-offs** |
> | Automate enforcement | **Removes memory, bias, and scales with org** |
> | gofmt | **No config, day one, universal format, enables tooling** |
> | unique_ptr | **Initially banned (unfamiliar); later allowed (ownership clarity)** |
> | CamelCase → snake_case | **External consistency outweighed internal as Python grew** |
> | Rule change process | **Solution-based, evidence from existing code, community → arbiters** |

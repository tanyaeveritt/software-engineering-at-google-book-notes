---
source_pdf: swe_at_google.2.pdf
part: Part I
keywords: practice, software-engineering, hyrums-law, beyonce-rule, shifting-left, sustainability
---

# Software Engineering Fundamentals Practice (12 questions)

#practice #swe-at-google #culture

## Related Concepts
- [[Software Engineering Fundamentals]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | SE vs Programming | SE = programming integrated over **time** |
> | Hyrum's Law | All observable behaviors **depended on** by somebody |
> | Beyonce Rule | Put a **CI test** on it |
> | Shifting Left | Earlier detection = **cheaper** fix |
> | Sustainability | Capability to react to **change** |
> | Churn Rule | Infrastructure teams do **migration** themselves |
> | Factor of 100,000 | Range of code **lifespans** |
> | Trade-offs | Data-driven, explicit **cost/benefit** analysis |

---

## Question 1 - SE vs Programming Definition [recall]
> Google defines software engineering as "programming integrated over ____."

> [!answer]- Show Answer
> **Time.** Software engineering is "programming integrated over time." Programming produces code; SE extends that to include maintenance of code for its useful lifespan. The addition of time adds the critical dimension of managing change, dependencies, and scale.

---

## Question 2 - Code Lifespan Factor [recall]
> By what factor do reasonable code lifespans vary, according to the chapter?

> [!answer]- Show Answer
> **100,000 times.** Code lifespans range from minutes (a one-off script) to decades (Google Search, Linux kernel). It is "silly to assume that the same best practices apply universally on both ends of that spectrum."

---

## Question 3 - Hyrum's Law Statement [recall]
> State Hyrum's Law in your own words. What analogy does the book use to describe it?

> [!answer]- Show Answer
> "With a sufficient number of users of an API, it does not matter what you promise in the contract: **all observable behaviors** of your system will be depended on by somebody." The book compares it to **entropy** — it can be mitigated but never eradicated. Just as entropy never decreases, Hyrum's Law will always apply to maintained software.

---

## Question 4 - Hash Ordering Example [recall]
> What example does the chapter use to illustrate Hyrum's Law in practice?

> [!answer]- Show Answer
> **Hash iteration ordering.** Although hash tables are not contractually ordered, programmers write code that depends on the order elements come out. Some code even uses hash iteration ordering as an inefficient random-number generator. This makes changing hash algorithms (for security or efficiency) painful, because it breaks these implicit dependencies.

---

## Question 5 - Beyonce Rule [recall]
> What is the "Beyonce Rule" at Google, and what scaling problem does it solve?

> [!answer]- Show Answer
> The Beyonce Rule: "If a product experiences outages from infrastructure changes, but the issue wasn't surfaced by tests in the **CI system**, it is not the fault of the infrastructure change." It solves the scaling problem of infrastructure engineers needing to track down every affected team — without this rule, an engineer would need to find every team with affected code and ask how to run their tests, which is impossible at Google's scale.

---

## Question 6 - Shifting Left [recall]
> What does "shifting left" mean, and where did the term originate?

> [!answer]- Show Answer
> **Shifting left** means finding problems earlier in the developer workflow (conception → design → implementation → review → testing → commit → canary → production). The term originated from arguments that **security** must not be deferred until the end of the development process — "shift left on security." Earlier detection = cheaper fixes.

---

## Question 7 - Sustainability Definition [recall]
> Define software sustainability as described in the chapter.

> [!answer]- Show Answer
> A project is **sustainable** if, for the expected lifespan of the software, you are capable of **reacting to whatever valuable change** comes along, for either technical or business reasons. Importantly, sustainability is about **capability** — you might choose not to perform a given upgrade, but you must not be fundamentally incapable of doing so.

---

## Question 8 - Compiler Upgrade Scenario [application]
> Your company hasn't upgraded its compiler in 5 years. Engineers are afraid the upgrade will be too painful. Using concepts from the chapter, what would you advise?

> [!answer]- Show Answer
> The chapter's Google 2006 compiler upgrade story directly applies. Key advice:
> 1. The first upgrade is always the most expensive — this is expected, not a reason to avoid it
> 2. Stagnation (avoiding upgrades) has hidden costs: missed optimizations (~25% extra compute), security vulnerabilities, and deepening Hyrum's Law dependencies
> 3. Invest in making upgrades **less painful** rather than avoiding them: build automation, establish the Beyonce Rule (CI testing), and develop expertise
> 4. Frequent upgrades make each one cheaper — code stops depending on implementation nuances and instead depends on actual abstractions
> 5. **Expertise, stability, conformity, familiarity, and policy** are the five factors that improve codebase flexibility over time

---

## Question 9 - Churn Rule Application [application]
> Your infrastructure team has built a new Widget and wants all 200 teams to migrate off the old one by a deadline. What approach does Google recommend, and why?

> [!answer]- Show Answer
> Google recommends the **Churn Rule** (established 2012): the infrastructure team should do the migration work themselves, not push it to the 200 customer teams. Reasons:
> - **Expertise scales better**: a dedicated team learns the problem deeply and applies that expertise to every subproblem
> - Forcing users to respond to churn means each team does a worse job ramping up, solves their immediate problem, then discards that now-useless knowledge
> - The old approach ("delete old Widget on August 15th") fails as dependency depth and breadth increase — a single break affects a growing percentage of the company

---

## Question 10 - Trade-off Analysis [application]
> An engineer proposes spending two weeks converting a linked-list to a high-performance data structure. It would use 5 GiB more RAM but save 2,000 CPUs. How should you evaluate this decision?

> [!answer]- Show Answer
> Use Google's approach of **explicit cost/benefit trade-off analysis**:
> 1. Consult the organization's **conversion table** (relative costs of CPU, RAM, network, engineer-time)
> 2. Account for **personnel cost** (2 weeks of engineer salary and opportunity cost — what else could they produce?)
> 3. Account for **resource costs** (5 GiB RAM cost vs 2,000 CPU savings)
> 4. The decision should not be "because I said so" — it should be backed by data or reasoned argument
> 5. Remember that in creative fields like SE, personnel cost usually dominates — a focused, productive engineer matters more than hardware costs

---

## Question 11 - Jevons Paradox and Distributed Builds [analysis]
> Google built a distributed build system that dramatically sped up builds, but new problems emerged. What happened and what broader principle does this illustrate?

> [!answer]- Show Answer
> This illustrates **Jevons Paradox**: consumption of a resource may increase as a response to greater efficiency in its use. When engineers built locally, they were personally incentivized to keep builds fast and lean. Once distributed builds hid the cost, bloated/unnecessary dependencies became rampant — nobody was incentivized to monitor build bloat. The broader principle: **even simple trade-offs have unforeseen downstream effects**. The savings far outweighed the costs, but Google had to reconceptualize system goals, establish best practices for small dependencies, and fund new tooling. This shows that decisions must be **revisited** as new data emerges.

---

## Question 12 - Time vs Scale Conflict [analysis]
> A team debates whether to fork an open-source library and customize it, or use it as a standard dependency. Using concepts from Chapter 1, analyze the trade-offs.

> [!answer]- Show Answer
> This is the classic **time vs scale conflict**:
> **Forking (favors short-term control):**
> - Customization for narrow domain may outperform the general solution
> - Isolates you from upstream changes — you control when/how to react to change
> - But: every fork creates a separate codebase that must be independently patched for security issues (scalability suffers)
>
> **Using the dependency (favors long-term sustainability):**
> - A single update patches all users at once (scales well)
> - Consistency across the org has great value
> - But: you're subject to the upstream team's decisions about change
>
> **Decision factors from the chapter:**
> - If project lifespan is **short**, forks are less risky
> - **Avoid forking interfaces** that operate across time or project boundaries (data structures, serialization formats, networking protocols)
> - "Consistency has great value, but generality comes with its own costs"
> - The answer depends on expected lifespan, scope of the fork, and type of interface

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | SE = programming + ? | **Time** (+ scale + trade-offs) |
> | Lifespan factor | **100,000x** |
> | Hyrum's Law | All observable behaviors **depended on** |
> | Beyonce Rule | CI test coverage **protects** infrastructure changes |
> | Shifting Left | Earlier = **cheaper** |
> | Sustainability | **Capability** to react to change |
> | Churn Rule | Infra teams do **migration** work |
> | Jevons Paradox | Efficiency gains can **increase** consumption |
> | Data-driven | Revisit when **data changes** |
> | "Because I said so" | **Terrible** reason for decisions |

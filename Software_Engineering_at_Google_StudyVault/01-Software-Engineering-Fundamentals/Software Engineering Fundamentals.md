---
source_pdf: swe_at_google.2.pdf
part: Part I
keywords: software-engineering, programming, sustainability, time, scale, trade-offs
---

# Software Engineering Fundamentals (Importance: ★★★)

#swe-at-google #culture #hyrums-law #beyonce-rule #shifting-left

*Reading time: ~4 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| SE vs Programming | SE = programming integrated over time; adds maintenance and collaboration |
| Time | Code lifespan ranges 100,000x; longer life = more change management needed |
| Scale | Policies and processes must scale sublinearly with org growth |
| Trade-offs | Decisions based on data, evidence, and explicit cost/benefit analysis |
| Hyrum's Law | All observable API behaviors will be depended on by somebody |
| Beyonce Rule | "If you liked it, you should have put a CI test on it" |
| Shifting Left | Find problems earlier in dev workflow = cheaper to fix |
| Sustainability | Capability to react to valuable change over the code's expected lifespan |

## Software Engineering vs Programming
Google defines software engineering as **"programming integrated over time."** Programming is the act of producing code; software engineering is the set of policies, practices, and tools to make that code useful for as long as needed.
- The expected lifespan of code varies by a factor of **100,000** (minutes to decades)
- Short-lived code is "just" a programming problem; long-lived code requires engineering
- "It's programming if 'clever' is a compliment, but it's software engineering if 'clever' is an accusation"
- SE adds the dimensions of **time**, **scale**, and **trade-offs** to programming

## Sustainability
A project is **sustainable** if, for the expected lifespan of the software, you are capable of reacting to whatever valuable change comes along (technical or business).
- Sustainability is about **capability**, not obligation — you may choose not to upgrade
- Being incapable of reacting to change is a high-risk bet that change never becomes critical
- Planning and managing the impact of required change is the essence of long-term sustainability
- The first upgrade is always the most expensive; subsequent ones get cheaper

## Hyrum's Law
> "With a sufficient number of users of an API, it does not matter what you promise in the contract: **all observable behaviors** of your system will be depended on by somebody."

- Conceptually akin to **entropy** — it can be mitigated but never eradicated
- Even with best intentions and solid code review, perfect contract adherence cannot be assumed
- Example: **hash iteration ordering** — programmers depend on unspecified ordering, making changes to hash algorithms painful
- The complexity of a change depends on how useful users find some observable behavior

## The Beyonce Rule
Google's policy: "If a product experiences outages from infrastructure changes, but the issue wasn't surfaced by tests in the CI system, it is **not the fault of the infrastructure change**."
- Colloquially: "If you liked it, you should have put a CI test on it"
- This scales well: infrastructure teams don't need to track down every affected team
- Bespoke, one-off tests not triggered by the common CI system do not count
- Protects infrastructure teams' ability to make changes safely

## Shifting Left
Finding problems **earlier** in the developer workflow reduces costs. The timeline runs: conception → design → implementation → review → testing → commit → canary → production.
- Originated from "shift left on security" arguments
- A security problem in production is extremely expensive; catching it before commit is cheap
- Static analysis and code review catch bugs far more cheaply than production incidents
- Google uses a **defense-in-depth** approach, catching as many defects early as possible

## Scale and Efficiency
Everything an organization does repeatedly should be **scalable** (sublinear) in terms of human effort.
- The **Churn Rule** (2012): infrastructure teams must migrate internal users themselves, not push work to customers
- Expertise scales better than forcing every user to ramp up on each change
- **Development branches** have built-in scaling problems — merge costs grow with org size
- Watch for **boiled-frog problems**: slow-growing costs that never manifest as a crisis

## Trade-offs and Costs
Decisions should be based on explicit cost/benefit analysis, not "because I said so."
- **Cost** includes: financial, resource (CPU), personnel, transaction, opportunity, and **societal** costs
- Google provides conversion tables (CPU vs RAM vs network) so engineers can self-analyze
- Status quo bias and loss aversion are real — personnel cost usually dominates in creative fields
- Example: **whiteboard markers** — optimizing for obstacle-free brainstorming outweighs protecting against marker theft
- Example: **distributed builds** — saved engineer time far outweighed construction costs, but caused Jevons Paradox (bloated dependencies)

## Revisiting Decisions
A data-driven culture requires the ability and willingness to **admit mistakes**.
- Data changes over time; assumptions get dispelled; decisions must be revisited
- Leaders who admit mistakes are **more respected**, not less
- "Being data driven over time implies the need to change directions when the data changes"
- Not everything is measurable — treat unmeasurable decisions with equal priority and greater care

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "programming vs SE" | SE = programming + **time** + scale + trade-offs |
| "Hyrum's Law" | All observable behaviors will be depended on by somebody |
| "Beyonce Rule" | If you liked it, you should have put a **CI test** on it |
| "shifting left" | Find bugs **earlier** in workflow = cheaper to fix |
| "sustainability" | Capable of reacting to change for the code's expected **lifespan** |
| "Churn Rule" | Infrastructure teams do migration work, **not** customers |
| "factor of 100,000" | Range of expected code **lifespans** (minutes to decades) |
| "boiled frog" | Slow-growing scale problems that never manifest as a singular crisis |
| "data-driven decisions" | Based on data + evidence + precedent; revisit when data changes |

## Related Notes
- [[Team Culture and Collaboration]]
- [[Knowledge Sharing]]
- [[Equity in Engineering]]
- [[Leading Teams]]

---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: practice, knowledge-sharing, psychological-safety, readability, documentation
---

# Knowledge Sharing Practice (10 questions)

#practice #knowledge-sharing #culture

## Related Concepts
- [[Knowledge Sharing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Psychological safety | **Most important** team factor |
> | Information islands | Knowledge **fragmentation** |
> | SPOF | One-person **bottleneck** |
> | Haunted graveyards | Fear-based code **avoidance** |
> | Readability | Mentorship via **code review** |
> | Chesterson's fence | Understand **why** before removing |
> | go/ links | **Predictable** internal URLs |

---

## Question 1 - Psychological Safety [recall]
> According to Google's research, what is the most important part of an effective team?

> [!answer]- Show Answer
> **Psychological safety.** Google's research showed that psychological safety — where people feel comfortable asking questions, being wrong, and learning new things — is the most important part of an effective team. To learn, you must first acknowledge there are things you don't understand, and the environment must welcome such honesty rather than punish it.

---

## Question 2 - Six Challenges [recall]
> Name and briefly define at least four of the six challenges to learning described in the chapter.

> [!answer]- Show Answer
> The six challenges are:
> 1. **Lack of psychological safety** — fear of punishment for mistakes or admitting ignorance
> 2. **Information islands** — knowledge fragmentation across non-communicating groups; leads to fragmentation, duplication, and skew
> 3. **Single point of failure (SPOF)** — critical info from only one person; "Let me take care of that for you" optimizes short-term at cost of long-term scalability
> 4. **All-or-nothing expertise** — split between experts who know "everything" and novices; no middle ground
> 5. **Parroting** — mimicry without understanding; copying patterns without knowing their purpose
> 6. **Haunted graveyards** — places in code people avoid changing out of fear and superstition, not understanding

---

## Question 3 - Recurse Center Rules [recall]
> What are the four social rules from the Recurse Center that Google uses to maintain psychological safety in groups?

> [!answer]- Show Answer
> 1. **No feigned surprise**: "What?! I can't believe you don't know what the stack is!" — creates a barrier to asking questions
> 2. **No "well-actuallys"**: pedantic corrections that are about grandstanding rather than precision
> 3. **No back-seat driving**: interrupting an existing discussion to offer opinions without committing to the conversation
> 4. **No subtle "-isms"**: "It's so easy my grandmother could do it!" — small expressions of bias (racism, ageism, etc.) that make people feel unwelcome

---

## Question 4 - Readability Process [recall]
> Describe Google's "readability" process. Who started it, and what percentage of engineers participate?

> [!answer]- Show Answer
> Google's readability process is a standardized, company-wide mentorship process for disseminating programming language best practices through code review. It covers language idioms, code structure, API design, documentation, and test coverage.
> - Started by **Craig Silverstein** (employee #3), who did line-by-line reviews with every new hire
> - Today ~**20%** of Google engineers participate at any time (as reviewers or authors)
> - ~**1-2%** are readability reviewers (volunteers held to highest standards)
> - Every changelist (CL) requires **readability approval**
> - EPR studies show it has a **net positive impact** on engineering velocity

---

## Question 5 - Chesterson's Fence [recall]
> What is the "Chesterson's fence" principle and how does it apply to knowledge sharing?

> [!answer]- Show Answer
> **Chesterson's fence**: before removing or changing something, first understand **why** it's there. The principle states that if you see a fence across a road and say "I don't see the use of this; let us clear it away," the intelligent reformer responds: "If you don't see the use of it, I certainly won't let you clear it away. Go away and think."
> In practice: engineers have a tendency to reach for "this is bad!" far too quickly for unfamiliar code. Seek out and understand the **context** behind decisions that seem unusual. If your change still makes sense after understanding, proceed; if not, document your reasoning.

---

## Question 6 - Tribal vs Written Knowledge [recall]
> How do tribal knowledge and written documentation complement each other?

> [!answer]- Show Answer
> **Written knowledge** scales better — it can reach the entire organization, not just those with direct access to an expert. But it can be more generalized, less applicable to specific situations, and requires maintenance.
> **Tribal knowledge** (human experts) can synthesize their expertise, assess what's applicable to the individual's use case, determine if docs are still relevant, and know where to find answers — or who knows them.
> Neither alone is sufficient: "Even a perfectly expert team with perfect documentation needs to communicate, coordinate, and adapt strategies over time."

---

## Question 7 - Documentation Culture [application]
> You've just figured out a complex setup process after struggling for three days. The existing docs were incomplete. What should you do according to Google's knowledge-sharing philosophy?

> [!answer]- Show Answer
> **Update the documentation immediately.** The chapter states: "The first time you learn something is the best time to see ways that the existing documentation and training materials can be improved." By the time you've absorbed the process, you'll have forgotten what was difficult. The principle is "leave the campground cleaner than you found it" — fix omissions and mistakes, even when the documentation is owned by a different part of the organization. At Google, engineers feel empowered to update docs regardless of ownership. Also: write it down for your future self, and share it so future newcomers benefit too.

---

## Question 8 - SPOF Scenario [application]
> On your team, one senior engineer handles all production debugging. They say "Let me take care of that for you" whenever issues arise. What's the problem and how would you fix it?

> [!answer]- Show Answer
> This is the **Single Point of Failure (SPOF)** antipattern. The senior engineer is a bottleneck: if they go on vacation or leave, the team cannot debug production issues. "Let me take care of that for you" optimizes for **short-term efficiency** ("It's faster for me to do it") at the cost of **long-term scalability** (the team never learns). It also leads to **all-or-nothing expertise** — the expert accumulates all knowledge while others are left as novices.
> **Fixes**: Have the expert **document** their debugging procedures, create a **codelab** or run **office hours** to train others, use **pair debugging** sessions so knowledge transfers, and ensure there's a **primary and secondary** owner for production debugging responsibilities.

---

## Question 9 - Scaling Knowledge Mechanisms [analysis]
> Compare group chats, mailing lists, and YAQS (Q&A platform) as knowledge-sharing mechanisms. When would you use each?

> [!answer]- Show Answer
> | Mechanism | Strengths | Weaknesses | Best For |
> |-----------|-----------|------------|----------|
> | **Group chats** | Quick back-and-forth; many people at once; can be archived | Little structure; hard to extract info if not actively involved | Quick questions needing fast answers |
> | **Mailing lists** | Searchable archives; more structure; easy to share widely; indexed by Moma (Google's intranet search) | Clumsy for quick exchanges; old threads may be outdated; lower signal-to-noise | Complex questions requiring context; broader audience |
> | **YAQS** | Helpful answers promoted; content editable to stay current; structured Q&A format | Requires active curation | Reusable answers that should evolve over time |
>
> Key insight: "Each of these approaches have different trade-offs and **complement one another**." Use group chats for immediacy, mailing lists for archival reach, and YAQS for living knowledge that stays accurate as code and facts change.

---

## Question 10 - Readability Trade-offs [analysis]
> Google's readability process is "controversial" — some engineers call it unnecessary bureaucracy. Analyze the trade-offs the chapter describes and whether the benefits outweigh the costs.

> [!answer]- Show Answer
> **Costs:**
> - Increased friction for teams without any readability-certified members (must find external reviewers)
> - Potential additional rounds of code review
> - Scales only **linearly** with org growth (human-driven), not sublinearly
> - Short-term code-review latency increases
>
> **Benefits:**
> - Codebase-wide **consistency** — code in a given language looks similar across tens of thousands of engineers writing over decades
> - Exposes engineers to knowledge **beyond their own team's** tribal knowledge
> - Enables **large-scale changes** across the monorepo by maintaining uniform patterns
> - Engineers who complete it report **higher satisfaction** with code quality
> - EPR studies show CLs from readability-certified authors take **statistically significantly less time** to review and submit
>
> **Verdict from the chapter:** The program makes a deliberate trade-off of increased short-term costs for **long-term payoffs** of higher quality, consistency, and expertise. The benefits are measured on a longer timescale (code written with a lifetime of years or decades), which is why some engineers focused on short-term velocity find it frustrating.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Psychological safety | **#1** effective team factor |
> | Information islands | Fragmentation, duplication, **skew** |
> | SPOF | Short-term efficient, long-term **unscalable** |
> | Haunted graveyards | Avoidance from **fear**, not understanding |
> | Readability | Code review **mentorship**; net positive velocity |
> | Chesterson's fence | Understand **why** before changing |
> | go/ links | Predictable, memorable, **persistent** URLs |
> | Peer bonuses | **Bottom-up** grassroots recognition |
> | Tribal + written | **Complement** each other; neither alone sufficient |
> | "Leave campground cleaner" | Update **docs** when you learn something |

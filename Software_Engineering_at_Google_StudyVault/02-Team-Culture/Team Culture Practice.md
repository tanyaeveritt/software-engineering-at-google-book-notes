---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: practice, teamwork, humility, respect, trust, genius-myth
---

# Team Culture Practice (10 questions)

#practice #team-dynamics #culture

## Related Concepts
- [[Team Culture and Collaboration]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Genius Myth | Team success ascribed to **single** person |
> | Three Pillars | **Humility, Respect, Trust** |
> | Bus Factor | People needed to **doom** a project |
> | Hiding | Increases **risk** |
> | You are not your code | Separate self-worth from **output** |
> | Postmortem | Learn, don't **blame** |
> | Googleyness | Explicit **rubric** for culture |

---

## Question 1 - Genius Myth Definition [recall]
> What is the "Genius Myth" and how does the chapter debunk it?

> [!answer]- Show Answer
> The **Genius Myth** is the tendency to ascribe the success of a team to a single person/leader. The chapter debunks it by showing that Linus Torvalds only wrote a proof-of-concept kernel — Linux was built by thousands. Guido Van Rossum wrote the first Python version, but hundreds contributed to subsequent versions. Michael Jordan's genius was in how he worked with his team under Phil Jackson's coaching. The real achievements are **collective**, not individual.

---

## Question 2 - Three Pillars [recall]
> Name and briefly define the three pillars of social interaction described in the chapter.

> [!answer]- Show Answer
> 1. **Humility**: You are not the center of the universe. You're neither omniscient nor infallible. You're open to self-improvement.
> 2. **Respect**: You genuinely care about others. You treat them kindly and appreciate their abilities and accomplishments.
> 3. **Trust**: You believe others are competent and will do the right thing. You're OK with letting them drive when appropriate.
> The root cause of almost any social conflict can be traced to a lack of one or more of these pillars.

---

## Question 3 - Bus Factor [recall]
> Define "bus factor" and explain why it matters for project health.

> [!answer]- Show Answer
> **Bus factor**: the number of people that need to get hit by a bus before your project is completely doomed. It matters because if only one person understands the prototype code, they have job security but the project is "toast" if they leave. Team members may not literally be hit by buses but can get married, move away, leave the company, or take leave. Mitigation: ensure good **documentation** plus a **primary and secondary owner** for each area.

---

## Question 4 - Hiding is Harmful [recall]
> Why does the chapter say "hiding" (working alone in secrecy) is harmful?

> [!answer]- Show Answer
> Hiding is harmful because:
> 1. You risk making **fundamental design mistakes** early on with no feedback
> 2. You risk **reinventing wheels** — someone else may be building the same thing
> 3. You forfeit the **pace benefit** of collaboration (your neighbor moved faster by working with others)
> 4. You reduce the **bus factor** to one
> 5. You might awaken to find the world has changed and your project is **irrelevant**
> The chapter uses the bicycle gear-shifter analogy: a neighbor built the same invention faster with help, and could have pointed out simple flaws in the first week.

---

## Question 5 - Postmortem Contents [recall]
> List the key elements that a good blameless postmortem should contain.

> [!answer]- Show Answer
> A good postmortem should include:
> 1. A brief **summary** of the event
> 2. A **timeline** from discovery through investigation to resolution
> 3. The **primary cause** of the event
> 4. **Impact and damage** assessment
> 5. Action items to **fix the problem immediately** (with owners)
> 6. Action items to **prevent the event from happening again**
> 7. **Lessons learned**
> It must NOT be a list of apologies, excuses, or finger-pointing. The goal is to light up the runway for those who follow.

---

## Question 6 - Googleyness Attributes [recall]
> Name at least four attributes in Google's explicit "Googleyness" rubric.

> [!answer]- Show Answer
> The six Googleyness attributes are:
> 1. **Thrives in ambiguity** — builds consensus and makes progress in shifting environments
> 2. **Values feedback** — humble enough to receive and give feedback gracefully
> 3. **Challenges status quo** — sets ambitious goals despite resistance
> 4. **Puts the user first** — empathy and respect for users
> 5. **Cares about the team** — actively helps coworkers without being asked
> 6. **Does the right thing** — strong ethics; willing to make difficult decisions
> Google created this explicit rubric because the informal "Googley" test was becoming a source of **unconscious bias** ("is just like me").

---

## Question 7 - Constructive Criticism [application]
> A colleague writes code with a confusing control flow. Compare a poor way and a good way to give feedback, using principles from the chapter.

> [!answer]- Show Answer
> **Poor approach**: "Man, you totally got the control flow wrong. You should be using the standard xyzzy pattern like everyone else." This tells them they're "wrong," demands change, and makes them feel stupid.
> **Good approach**: "Hey, I'm confused by the control flow in this section. I wonder if the xyzzy pattern might make this clearer and easier to maintain?" This uses **humility** (the problem is framed as your confusion, not their error), offers a **suggestion** rather than a demand, and keeps focus on **the code**, not the person.
> Key principle: "You are not your code." Separate the creative output from the person's identity.

---

## Question 8 - New Team Member Scenario [application]
> You join a new team and start sending code review comments to teammates. After two weeks, your director says teammates are upset about your "harsh criticism." What went wrong, and how should you fix it?

> [!answer]- Show Answer
> This is the **Joe story** from the chapter. Joe was asking legitimate questions but failed to read the team's existing culture and insecurity around code review. What went wrong:
> - He didn't consider the team's **psychological safety** level
> - He introduced code reviews without discussing the idea with the team first
> Fix: Use a **subtler approach** — discuss the idea of code reviews with the team in advance, ask them to try it out for a few weeks, and frame feedback using humility ("I'm confused by..." rather than "This is wrong"). Build trust before pushing process changes. Relationships outlast projects.

---

## Question 9 - Google X Failure Culture [analysis]
> Google X deliberately builds failure into its incentive system. Analyze why this works and what conditions make it possible.

> [!answer]- Show Answer
> At Google X, people come up with outlandish ideas and coworkers are **actively encouraged to shoot them down** as fast as possible. Individuals are rewarded (and compete) to see how many ideas they can **disprove or invalidate** in a fixed period. Only concepts that truly cannot be debunked proceed to prototype.
> **Why it works**: It embodies "fail fast" — killing bad ideas early saves enormous resources. It removes the **ego attachment** to ideas (disproving is valued, not just proving). It creates **psychological safety** around failure.
> **Conditions required**: A culture of **trust** (criticism targets ideas, not people), explicit **incentive alignment** (rewards for disproving, not just shipping), and **HRT** as a foundation. Without humility, respect, and trust, this system would collapse into personal attacks.

---

## Question 10 - Pair Programming Conflict [analysis]
> Two engineers with different working styles (one bottom-up, one top-down) find pair programming frustrating. Using the chapter's framework, analyze how they might resolve this.

> [!answer]- Show Answer
> This is the **Karl and the author** story from the chapter. The bottom-up engineer dives into the muck and tries things quickly; the top-down engineer wants to understand the full landscape before tackling the bug. Their resolution:
> 1. They had a longstanding history of **trust and respect** — this was the essential foundation
> 2. Combined with **patience**, they improvised a new method: sit together to identify the bug, then **split up** and attack from both directions (top-down and bottom-up), then come back with findings
> 3. This preserved both engineers' natural styles while maintaining collaboration
> **Analysis**: The solution worked because they didn't try to force one style on the other (humility), respected each other's approach (respect), and trusted each other to work independently (trust). The chapter's lesson: patience and willingness to improvise new working styles save both the project and the friendship.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Genius Myth | Team success ≠ **one** person |
> | Three Pillars | **Humility, Respect, Trust** |
> | Bus Factor | Redundancy = project **survival** |
> | Hiding | Increases **risk**, decreases speed |
> | "You are not your code" | Separate identity from **output** |
> | Constructive criticism | Focus on **code**, not person; use humility |
> | Postmortem | **Learn**, don't blame |
> | Googleyness | Explicit rubric prevents **bias** |
> | Fail fast | Kill bad ideas **early** = save resources |
> | Open to influence | Vulnerability = **strength** |

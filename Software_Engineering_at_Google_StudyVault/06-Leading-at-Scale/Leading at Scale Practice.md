---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: practice, leadership, scaling, delegation, trade-offs
---

# Leading at Scale Practice (10 questions)

#practice #leadership #culture

## Related Concepts
- [[Leading at Scale]]
- [[Leading Teams]]
- [[Knowledge Sharing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three Always | **Deciding, Leaving, Scaling** |
> | SPOF | **Single Point of Failure — eliminate yourself as one** |
> | Trade-off triangle | **Quality, Latency, Capacity** |
> | Team identity anchor | **Problem, not product** |
> | Cycle of Success | **Analysis → Struggle → Traction → Reward → Compression** |
> | Important vs Urgent | **Focus on important; delegate urgent** |

---

## Question 1 - Three Always [recall]
> What are the "three Always of leadership" described in Chapter 6?

> [!answer]- Show Answer
> **Always Be Deciding**, **Always Be Leaving**, and **Always Be Scaling**. These represent the core principles for leaders managing teams of teams: making iterative trade-off decisions, building self-sufficient organizations, and protecting personal resources as scope grows.

---

## Question 2 - Always Be Deciding Steps [recall]
> What are the three steps in the "Always Be Deciding" process?

> [!answer]- Show Answer
> 1. **Identify the blinders** — uncover assumptions people make without realizing
> 2. **Identify the key trade-offs** — there is no silver bullet; all solutions involve trade-offs
> 3. **Decide, then iterate** — make the best decision for the moment and revisit monthly

---

## Question 3 - Web Search Trade-offs [recall]
> In the Web Search latency case study, what three factors formed the "triangle of tension"?

> [!answer]- Show Answer
> **Quality** (Good), **Latency** (Fast), and **Capacity** (Cheap). Improving any one of these deliberately harms at least one of the others. The key insight was treating latency as a first-class goal alongside quality and capacity, rather than an accidental side effect.

---

## Question 4 - Analysis Paralysis [recall]
> How does the chapter recommend avoiding analysis paralysis when making decisions?

> [!answer]- Show Answer
> Frame the process as **continuous rebalancing of trade-offs**. Tell teams: "We're going to try this decision and see how it goes. Next month, we can undo the change or make a different decision." This lowers the stakes and keeps people in a learning state.

---

## Question 5 - Self-Driving Team [recall]
> What are the three parts of constructing a "self-driving" team?

> [!answer]- Show Answer
> 1. **Dividing the problem space** into subproblems
> 2. **Delegating subproblems** to trained leaders
> 3. **Iterating and adjusting** as needed
>
> The organization should be able to solve problems without the leader being present.

---

## Question 6 - Team Identity Anchor [recall]
> Why should a team's identity be anchored to a problem rather than a product?

> [!answer]- Show Answer
> A product is a **solution** to a problem, and solutions can be replaced by better ones. If the team identifies with a specific solution (e.g., "We manage Git repositories"), they will resist change even when it's best for the organization. Anchoring to a **problem** (e.g., "We provide version control") frees the team to experiment with different solutions over time.

---

## Question 7 - Delegation Scenario [application]
> You are a tech lead who notices a recurring infrastructure issue. You could fix it in 20 minutes, but an engineer on your team could handle it in about 2 hours. The issue is not time-sensitive. What should you do and why?

> [!answer]- Show Answer
> **Delegate the task** to the engineer. According to the "Always Be Leaving" principle, unless a task is truly time-sensitive, leaders should assign work to others to create growth opportunities. The short-term efficiency loss (2 hours vs. 20 minutes) is worth the long-term benefit of training leaders and removing yourself from the critical path. Ask yourself: "Am I really the only one who can do this work?"

---

## Question 8 - Important vs Urgent [application]
> Your calendar is full of meetings, your inbox has 200 messages, and you haven't spent time on your team's quarterly strategy in weeks. Using the chapter's framework, how should you restructure your approach?

> [!answer]- Show Answer
> Apply the **Important vs Urgent** framework and the **Marie Kondo** principle:
> 1. **Delegate** urgent-but-not-important tasks to sub-leaders
> 2. **Schedule dedicated time** (2+ hours blocks) for strategic work that only you can do
> 3. **Identify the top 20%** of truly critical tasks and focus only on those
> 4. Give yourself **permission to drop** the other 80% — sub-leaders will pick up the middle 60%, and truly critical items will come back
> The key insight is that 100% reactive mode means 0% strategic work.

---

## Question 9 - Compression Stage [analysis]
> A leader successfully solves Problem A and is rewarded with Problem B but no additional headcount. How does the "compression stage" work, and what organizational prerequisites must already be in place for it to succeed?

> [!answer]- Show Answer
> The **compression stage** means taking everything the team has been doing for Problem A and compressing it to require roughly half the resources, freeing the other half to tackle Problem B. This is the final step in the **Cycle of Success** (Analysis → Struggle → Traction → Reward → Compression).
>
> Prerequisites for success:
> - A **self-driving organization** must already exist for Problem A (strong sub-leaders, healthy processes, positive culture)
> - The leader must have practiced **"Always Be Leaving"** so they are not an SPOF
> - **Delegation** must be well-established so sub-leaders can handle Problem A independently
> - Without these, the leader becomes overwhelmed and both problems suffer.

---

## Question 10 - Energy Management [analysis]
> The chapter argues that managing energy is as important as managing time. Analyze why a leader's bad mood is described as especially dangerous, and connect this to the broader scaling principles.

> [!answer]- Show Answer
> A leader's mood **sets the tone for everyone around them**. A bad mood can lead to terrible decisions — harsh emails, overly critical judgments, demoralized teams. This connects to "Always Be Scaling" because as scope grows, the leader's emotional state has an **amplified blast radius**: it affects more people, more teams, and more decisions.
>
> The chapter recommends taking a **mental health day** rather than doing active damage. This is possible only if the leader has built a **self-driving organization** (Always Be Leaving) — if you can't step away for a day without things falling apart, you are an SPOF and cannot effectively protect your energy. All three "Always" principles are interconnected: you can only scale if you can leave, and you can only leave if you've been making good iterative decisions about team structure.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Three Always | **Deciding, Leaving, Scaling** |
> | Always Be Deciding steps | **Identify blinders → Identify trade-offs → Decide & iterate** |
> | Trade-off triangle | **Quality, Latency, Capacity** |
> | Self-driving team steps | **Divide problem space → Delegate → Iterate** |
> | Team identity | **Anchor to problem, not product** |
> | SPOF litmus test | **Can you disappear for a week?** |
> | Important vs Urgent | **Focus on important; delegate urgent** |
> | Marie Kondo principle | **Top 20% only; drop the 80%** |
> | Cycle of Success | **Analysis → Struggle → Traction → Reward → Compression** |
> | Energy management | **Real vacations, disconnect, mental health days** |

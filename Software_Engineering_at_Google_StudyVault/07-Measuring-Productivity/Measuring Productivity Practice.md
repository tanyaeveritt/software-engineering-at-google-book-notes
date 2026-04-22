---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: practice, productivity, metrics, QUANTS, GSM
---

# Measuring Productivity Practice (10 questions)

#practice #leadership #culture

## Related Concepts
- [[Measuring Productivity]]
- [[Leading at Scale]]
- [[Code Review]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | GSM | **Goals → Signals → Metrics** |
> | QUANTS | **Quality, Attention, iNtellectual complexity, Tempo, Satisfaction** |
> | Streetlight effect | **Measuring what's easy, not what matters** |
> | Triage question | **Will action be taken on the result?** |
> | Mixed methods | **Quantitative + qualitative to triangulate** |

---

## Question 1 - GSM Framework [recall]
> What does GSM stand for and what is the purpose of each component?

> [!answer]- Show Answer
> **Goals/Signals/Metrics**.
> - **Goal**: A desired end result, phrased without reference to specific measurements
> - **Signal**: How you might know you've achieved the goal (may not be directly measurable)
> - **Metric**: A measurable proxy for the signal (the thing you actually measure)
>
> The framework prevents the streetlight effect and metrics bias by ensuring metrics trace back to meaningful goals.

---

## Question 2 - QUANTS Components [recall]
> List all five components of the QUANTS framework for measuring productivity.

> [!answer]- Show Answer
> 1. **Quality** of the code
> 2. **Attention** from engineers (flow state, distractions)
> 3. **iNtellectual complexity** (cognitive load)
> 4. **Tempo and velocity** (speed of task completion, releases)
> 5. **Satisfaction** (happiness with tools, work, burnout)
>
> Teams should set goals in each component to avoid inadvertently improving one at the cost of another.

---

## Question 3 - Triage Questions [recall]
> What are the key triage questions Google asks before deciding to measure a software process?

> [!answer]- Show Answer
> 1. What result are you expecting, and why?
> 2. If the data supports your expected result, what action will be taken?
> 3. If you get a **negative result**, will appropriate action be taken?
> 4. Who is going to decide to take action, and when?
> 5. Does the decision maker trust the type of data you can provide?
>
> The third question stops most projects — decision makers are often interested but won't change course regardless of the result.

---

## Question 4 - Streetlight Effect [recall]
> What is the "streetlight effect" in the context of metrics, and how does GSM prevent it?

> [!answer]- Show Answer
> The streetlight effect is "looking for your keys under the streetlight" — using metrics that are **easily accessible and easy to measure** regardless of whether they suit your needs. GSM prevents this by requiring you to define **goals first**, then signals, and finally metrics. This forces you to think about which metrics will actually help achieve your goals, rather than simply using what's readily available.

---

## Question 5 - Quantitative vs Qualitative [recall]
> When quantitative and qualitative metrics disagree, which is typically wrong according to Google's experience?

> [!answer]- Show Answer
> The **quantitative metrics** are usually the ones that are wrong. Quantitative metrics provide power and scale but no context. They may not capture the full picture — for example, automated builds counted as "engineer builds" in logs data even though engineers weren't blocked on them. Qualitative studies provide the context needed to identify such gaps.

---

## Question 6 - When Not to Measure [recall]
> Give three reasons why measurement might not be worthwhile.

> [!answer]- Show Answer
> 1. **No action will be taken** regardless of the result (decision maker won't change course)
> 2. Results will only serve as **vanity metrics** to support a predetermined decision
> 3. Available metrics are **too imprecise** and can be confounded by other factors
>
> Other valid reasons: you can't afford to change the process right now, or results will be invalidated by upcoming factors (e.g., a reorg).

---

## Question 7 - Individual Measurement [recall]
> Why does Google advise against using productivity metrics to evaluate individual engineers?

> [!answer]- Show Answer
> If productivity metrics are used for **performance reviews**, engineers will **game the metrics**, and the metrics will no longer be useful for measuring and improving productivity across the organization. The only way to make these measurements work is to **measure the aggregate effect**, not individuals.

---

## Question 8 - Readability Measurement [application]
> Your team has a mandatory code review mentoring process that takes significant time. Some engineers call it outdated. Using the triage and GSM frameworks, outline how you would evaluate whether to keep it.

> [!answer]- Show Answer
> **Triage first**: Confirm that if data shows the process is not worthwhile, the team will actually **abolish it** (not just shelve the data). Confirm who the decision maker is and what data format they trust.
>
> **Apply GSM**:
> - **Goals** (using QUANTS): Quality — does code quality improve? Tempo — are reviews faster after certification? Satisfaction — do engineers find it worthwhile? Intellectual complexity — do engineers learn from it?
> - **Signals**: Engineers report learning; certified engineers' code reviews are faster; engineers view the process positively
> - **Metrics**: Survey data on satisfaction and perceived learning; logs data on median review time (certified vs. not); survey on whether process negatively impacts velocity
>
> Use **mixed methods** (surveys + logs) to triangulate, and present actionable recommendations built into the developer workflow.

---

## Question 9 - Metric Design Flaws [analysis]
> A team measures "lines of code per month" to track engineering productivity. Using concepts from Chapter 7, explain why this metric is problematic and propose a better approach.

> [!answer]- Show Answer
> **Lines of code (LOC) per month** is a deeply flawed metric because:
> 1. It encourages writing **verbose, insipid code** rather than concise solutions
> 2. It books LOC "on the wrong side of the ledger" — lines should be seen as "lines spent" not "lines produced" (Dijkstra)
> 3. It only measures **Tempo** from QUANTS while ignoring Quality, Attention, Intellectual complexity, and Satisfaction
> 4. It falls victim to the **streetlight effect** — it's easy to measure but doesn't capture actual productivity
>
> **Better approach**: Use the GSM framework. Define goals first (e.g., "engineers complete meaningful features efficiently"). Create signals (e.g., "features delivered per sprint," "code review turnaround time"). Select multiple metrics that triangulate: logs data for review velocity, surveys for satisfaction, and quality proxies like defect rates.

---

## Question 10 - QUANTS Trade-off [analysis]
> A team proposes removing all code review requirements to improve engineering velocity. Using the QUANTS framework, analyze the potential consequences of this decision.

> [!answer]- Show Answer
> This proposal optimizes for **Tempo/Velocity** while potentially destroying other QUANTS dimensions:
>
> - **Quality**: Without code review, defects slip through. No second pair of eyes checking correctness, no readability enforcement. Code quality degrades over time.
> - **Attention**: Engineers may spend less time on reviews but more time debugging production issues, leading to more context switching and interruptions.
> - **Intellectual complexity**: Knowledge sharing through reviews is lost. New engineers lose a key learning mechanism. Technical debt increases cognitive load.
> - **Satisfaction**: Initially, engineers may feel faster, but over time, a degraded codebase with more bugs and less knowledge sharing leads to frustration and burnout.
> - **Tempo**: Even velocity may ultimately decrease — faster submissions but more rollbacks, more debugging, more rework.
>
> The QUANTS framework exists precisely to prevent this kind of single-dimension optimization. The chapter's example makes the same point: "I can make your review velocity very fast: just remove code reviews entirely."

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | GSM | **Goals → Signals → Metrics** |
> | QUANTS | **Quality, Attention, iNtellectual complexity, Tempo, Satisfaction** |
> | Streetlight effect | **Measuring what's easy, not what matters** |
> | Metrics creep | **Adding metrics without principled approach; GSM prevents this** |
> | Triage gate | **"Will action be taken?" stops most measurement projects** |
> | Mixed methods | **Surveys + logs + qualitative to triangulate** |
> | Quant vs qual disagree | **Quantitative is usually wrong** |
> | Don't measure individuals | **Engineers will game metrics** |
> | Tool-driven recommendations | **Change tools, not just behavior** |
> | Readability result | **Worthwhile overall; engineers learned and were faster** |

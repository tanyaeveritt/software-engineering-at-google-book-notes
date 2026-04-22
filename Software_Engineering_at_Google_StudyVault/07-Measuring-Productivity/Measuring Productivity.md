---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: productivity, metrics, QUANTS, GSM framework, measurement, data-driven
---

# Measuring Engineering Productivity (Importance: ★★★)

#leadership #culture

*Reading time: ~4 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Why Measure | Scale org productivity without proportional communication overhead |
| Triage First | Ask if result is actionable before measuring |
| GSM Framework | Goals → Signals → Metrics (prevents streetlight effect) |
| QUANTS | Quality, Attention, iNtellectual complexity, Tempo, Satisfaction |
| Mixed Methods | Combine surveys, logs data, and qualitative studies |
| Readability Case | Measured whether readability process was worth the cost |

## Why Measure Engineering Productivity?
As organizations grow linearly, **communication costs grow quadratically** (Brooks). Instead of just adding people, you can make each individual more productive.
- Google created a dedicated research team including software engineers, cognitive psychologists, and behavioral economists
- Goal: take a **data-driven approach** to measuring and improving engineering productivity
- The improvement cycle itself must be efficient — don't spend 50 engineers to save 10

## Triage: Is It Even Worth Measuring?
Before measuring anything, ask five critical questions:
- **What result are you expecting, and why?** — Acknowledge preconceived notions to address bias
- **If data supports your expectation, what action will be taken?** — If no action, don't measure
- **If you get a negative result, will appropriate action be taken?** — This stops most projects; decision makers often won't change course regardless
- **Who will decide to take action, and when?** — Ensure the requester is empowered to act
- **Is the person convinced by the type of data you can provide?** — Know your audience's preferred evidence style

## When NOT to Measure
- You can't afford to change the process right now
- Results will be invalidated by other factors (e.g., upcoming reorg)
- Decision maker has **unwavering beliefs** that no data will change
- Results will only serve as **vanity metrics** for a predetermined decision
- Available metrics are **too imprecise** and would be confounded by other factors

## GSM Framework: Goals, Signals, Metrics
Google uses the **Goals/Signals/Metrics** framework to guide metric creation:
- **Goal**: desired end result, phrased without reference to specific measurements
- **Signal**: how you might know you've achieved the goal (may not be directly measurable)
- **Metric**: a measurable **proxy** for the signal (may require multiple metrics to triangulate)

**Benefits of GSM**:
- Prevents the **streetlight effect** (measuring only what's easy, not what matters)
- Prevents **metrics creep** and **metrics bias** by selecting metrics up-front with a principled approach
- Shows **measurement coverage gaps** — identifies what is not measurable

## QUANTS: Five Components of Productivity
To avoid improving one dimension while harming others, use the mnemonic **QUANTS**:
- **Quality** of the code — test quality, architecture resilience
- **Attention** from engineers — flow state, distractions, context switching
- **iNtellectual complexity** — cognitive load, unnecessary complexity
- **Tempo and velocity** — task completion speed, release frequency
- **Satisfaction** — happiness with tools, work, end product, burnout levels

## Using Data to Validate Metrics
Quantitative metrics provide **power and scale** but no context. Qualitative studies provide **context and narrative** but limited scale. Google uses **mixed methods** to triangulate.
- When quantitative and qualitative metrics **disagree**, the quantitative metrics are usually the ones that are wrong
- Experience sampling studies (interrupt engineers in context) help validate whether metrics capture real experience
- **Never use productivity metrics to evaluate individuals** — engineers will game them, making the metrics useless

## Case Study: Readability Process
The readability process required hundreds of engineers reviewing code to grant "readability" certification. The research team measured it using:
- **Readability-specific survey** (given after completing the process)
- **Quarterly survey** (tracking broader productivity, not readability-specific)
- **Logs data** (time to review, time to submit CLs)

**Result**: Readability was overall worthwhile — engineers were satisfied, learned from it, and had faster code reviews. Pain points were also identified and addressed.

## Taking Action and Tracking Results
The team always prepares **recommendations** after research. Recommendations should be **tool-driven** — assume engineers will make appropriate trade-offs if given proper tools and data.
- Don't just tell engineers to change their behavior; change the tools to support better behavior
- Build improvements into the **developer workflow and incentive structures**

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "GSM framework" | **Goals → Signals → Metrics** |
| "QUANTS" | **Quality, Attention, iNtellectual complexity, Tempo, Satisfaction** |
| "Streetlight effect" | **Measuring only what's easy, not what matters; GSM prevents this** |
| "When not to measure" | **No action will be taken, vanity metrics, imprecise proxies** |
| "Mixed methods" | **Combine quantitative (logs) + qualitative (surveys, interviews)** |
| "Quant vs qual disagree" | **Quantitative metrics are usually the ones that are wrong** |
| "Don't measure individuals" | **Engineers will game metrics; measure aggregate effect only** |

## Related Notes
- [[Leading at Scale]]
- [[Code Review]]
- [[Style Guides and Rules]]
- [[Knowledge Sharing]]
- [[Software Engineering Fundamentals]]

---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, testing, test-pyramid, beyonce-rule, flaky-tests
---

# Testing Overview Practice (10 questions)

#practice #testing #processes

## Related Concepts
- [[Testing Overview]]
- [[Unit Testing]]
- [[Larger Testing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Beyoncé Rule | **"If you liked it, you shoulda put a test on it"** |
> | Test pyramid ratio | **80% unit, 15% integration, 5% E2E** |
> | Small test constraint | **Single process**, no I/O, no network |
> | Flaky threshold | ~**1%** flakiness → tests lose value |
> | GWS impact | Emergency pushes dropped **50%** |

---

## Question 1 - Beyoncé Rule [recall]
> What is the Beyoncé Rule at Google, and who typically invokes it?

> [!answer]- Show Answer
> "If you liked it, then you shoulda put a test on it." It means you should test everything you don't want to break—including performance, correctness, accessibility, security, and failure handling. It is often invoked by **infrastructure teams** making cross-codebase changes: if their changes pass your tests but still break your product, your team is responsible for fixing it and adding the missing tests.

---

## Question 2 - Test Size Definitions [recall]
> What are the three test sizes at Google and what resource constraints does each impose?

> [!answer]- Show Answer
> - **Small**: must run in a single process (often single thread), no I/O, no sleep, no network or disk access. Fastest and most deterministic.
> - **Medium**: can span multiple processes on a **single machine**, can make network calls to localhost only.
> - **Large**: can span **multiple machines** and access the network freely. Reserved for full system/end-to-end tests.
> Size is determined by resource consumption and constraints, not lines of code.

---

## Question 3 - Test Pyramid [recall]
> What is Google's recommended test mix, and what two anti-patterns should be avoided?

> [!answer]- Show Answer
> Recommended mix: **80% unit tests, 15% integration tests, 5% end-to-end tests** (the "test pyramid").
> Anti-patterns:
> 1. **Ice cream cone**: too many end-to-end tests, few unit tests → slow, unreliable, difficult to work with. Common in prototypes rushed to production.
> 2. **Hourglass**: many E2E and unit tests but few integration tests → E2E failures that could have been caught by medium-scope tests. Occurs when tight coupling makes it hard to instantiate dependencies in isolation.

---

## Question 4 - GWS Story [recall]
> What problem did Google Web Server face, and what was the result of introducing automated testing?

> [!answer]- Show Answer
> By 2005, GWS had slowed dramatically in productivity. More than **80% of production pushes** contained user-affecting bugs that had to be rolled back. The tech lead instituted a policy requiring all new code changes to include tests, run continuously. Within one year, **emergency pushes dropped by half**, despite record numbers of new changes each quarter. Today GWS has tens of thousands of tests and releases almost daily.

---

## Question 5 - Flaky Tests Cost [recall]
> At what flakiness rate do tests begin to lose value, and what is Google's actual flake rate?

> [!answer]- Show Answer
> Tests begin to lose value at approximately **1% flakiness**. At that point, engineers stop trusting and reacting to test failures, eliminating the test suite's value. Google's flake rate hovers around **0.15%**, which still implies thousands of flakes per day. They actively invest engineering hours to keep flakiness in check. Rerunning flaky tests is only a stopgap that trades CPU for engineer time.

---

## Question 6 - Code Coverage Pitfall [application]
> A team sets an 80% code coverage requirement. Over time, engineers consistently land changes with exactly 80% coverage. What problem does Google warn about in this scenario?

> [!answer]- Show Answer
> Google warns that engineers will treat the coverage bar as a **ceiling** rather than a **floor**. Instead of striving for thorough testing, they do just enough to hit 80% and no more. Code coverage only measures that a line was invoked, not that anything useful was checked. A better approach is to think about **behaviors tested**: do you have confidence that everything customers expect to work will work? Can you catch breaking changes in dependencies? Coverage is a signal, not a substitute for critical thinking about test quality.

---

## Question 7 - Scope vs. Size [application]
> An engineer writes a narrow-scoped test for a single UI date picker component, but it requires launching a real browser. What size is this test, and why does this illustrate the difference between scope and size?

> [!answer]- Show Answer
> This test is **narrow-scoped** (unit test—validates a single component) but **medium-sized** (requires running a full browser, which means multiple processes on a single machine). This illustrates that scope (how much code is validated) and size (resource constraints) are **interrelated but distinct** concepts. A narrow-scoped test can still be medium or large if it needs heavyweight infrastructure. Google prefers to write the **smallest possible test** for a given piece of functionality.

---

## Question 8 - Testing Culture Initiatives [recall]
> Name the three key initiatives that helped establish testing culture at Google, and briefly describe each.

> [!answer]- Show Answer
> 1. **Orientation Classes** (from 2005): one-hour session during new hire orientation teaching the value of automated testing. New hires unknowingly became "trojan horses" bringing testing practices to their teams. Within 1-2 years, testing-aware engineers outnumbered the old guard.
> 2. **Test Certified**: a 5-level certification program with concrete actions per level (e.g., set up CI, track coverage, remove nondeterminism). Helped 1,500+ projects. Replaced by **Project Health (pH)** in 2015.
> 3. **Testing on the Toilet (TotT)** (from April 2006): one-page technical writeups posted in restroom stalls. Initially polarizing, it became a Google cultural staple. Longest-running and most impactful testing initiative.

---

## Question 9 - Why Not Mandate Testing [analysis]
> The Testing Grouplet considered asking senior leadership for a testing mandate but decided against it. Why? What does this reveal about Google's approach to cultural change?

> [!answer]- Show Answer
> A mandate on how to develop code would be **seriously counter to Google culture** and would likely slow progress regardless of the idea being mandated. The Testing Grouplet believed that **successful ideas would spread on their own**: if engineers chose to write tests voluntarily, it meant they had fully accepted the idea and were likely to continue doing the right thing even without external compulsion. This reveals Google's preference for **demonstrating value** over imposing top-down rules—cultural change is more durable when it comes from conviction rather than compliance.

---

## Question 10 - Designing the Test Suite [analysis]
> A startup is building a new product rapidly. They have no automated tests and rely on manual QA. Using Google's experience, explain what trajectory they are likely on and what they should do to correct it.

> [!answer]- Show Answer
> They are on the path toward the **ice cream cone anti-pattern**: manual tests accumulate and dominate the testing portfolio. When engineers eventually try to add automated tests, the code is often too difficult to unit test (tightly coupled, no seams for test doubles), so only end-to-end tests can be written—creating "legacy code within days." Google's recommendation: **move toward the test pyramid within the first few days of development** by building out unit tests first, then introduce automated integration tests, and progressively move away from manual end-to-end testing. Make unit tests a requirement for submission. The longer they wait, the more expensive it becomes to retrofit testability into the codebase.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Beyoncé Rule | **Test everything you don't want to break** |
> | Small test | Single process, no I/O, no network |
> | Medium test | Single machine, localhost network OK |
> | Large test | Multi-machine, full network access |
> | Test pyramid | 80% unit / 15% integration / 5% E2E |
> | Ice cream cone | Too many E2E → slow, unreliable |
> | Hourglass | Missing integration layer |
> | Flaky threshold | ~1% → tests lose value |
> | Google flake rate | ~0.15% |
> | Code coverage trap | Bar becomes ceiling not floor |
> | GWS impact | 50% reduction in emergency pushes |
> | TotT | Restroom flyers—longest-running initiative |

---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: testing, automation, test-size, test-scope, beyonce-rule, test-pyramid, flaky-tests
---

# Testing Overview (Importance: ★★★)

#testing #processes #beyonce-rule

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Why Test | Prevent bugs, support change, improve design, enable fast releases |
| Test Size | Small (single process), Medium (single machine), Large (multi-machine) |
| Test Scope | Narrow (unit), Medium (integration), Broad (end-to-end) |
| Test Pyramid | 80% unit, 15% integration, 5% end-to-end |
| Beyoncé Rule | "If you liked it, you shoulda put a test on it" |
| Flaky Tests | At ~1% flakiness, tests lose value; Google targets ~0.15% |
| Code Coverage | Useful signal but not a goal unto itself; treat 80% as floor not ceiling |
| GWS Story | Automated testing cut emergency pushes by 50% |

## Why Write Tests
Testing is foundational to enabling software to change. Automated tests catch bugs early and support refactoring with confidence.
- **Less debugging**: tested code has fewer defects throughout its lifetime
- **Increased confidence in changes**: teams can review/accept changes knowing behaviors are verified
- **Improved documentation**: clear tests function as executable documentation
- **Simpler reviews**: reviewers verify correctness via tests rather than mental walkthroughs
- **Fast, high-quality releases**: many Google projects release daily thanks to automated tests

## The GWS Catalyst
Google Web Server (GWS) was the watershed moment. By 2005, >80% of production pushes had user-affecting bugs.
- The tech lead mandated engineer-driven automated testing for all new changes
- Within one year, emergency pushes dropped by **50%** despite record new changes
- Key insight: you can't rely on programmer ability alone—even if each engineer writes one bug/month, a 100-person team produces **five bugs per workday**

## Test Size (Resource Constraints)
Google classifies every test by **size** based on resource consumption, not lines of code.
- **Small tests**: single process, single thread, no I/O, no sleep, no network/disk. Fast and deterministic.
- **Medium tests**: can span multiple processes on a single machine, can use localhost network. More flexible but less deterministic.
- **Large tests**: can span multiple machines and networks. Reserved for full system end-to-end validation and legacy components.

## Test Scope (Code Validated)
Scope refers to how much code a test is intended to validate—distinct from size.
- **Narrow-scoped (unit)**: single class or method
- **Medium-scoped (integration)**: interactions between a few components
- **Broad-scoped (end-to-end/system)**: multiple parts of the system together

## The Test Pyramid
Google recommends: **80% unit, 15% integration, 5% end-to-end**.
- Unit tests form the base: fast, stable, easy to diagnose
- **Anti-patterns**: "ice cream cone" (many E2E, few unit) and "hourglass" (many E2E + unit, few integration)
- Favoring unit tests gives high confidence quickly and early in development

## The Beyoncé Rule
"If you liked it, then you shoulda put a test on it." Test everything you don't want to break.
- Includes performance, correctness, accessibility, security, and **failure handling**
- Infrastructure teams invoke this rule: if their changes pass your tests but break your product, **you** are on the hook
- Write automated tests that simulate failures (exceptions, RPC errors, latency injection, Chaos Engineering)

## Flaky Tests Are Expensive
A test is **flaky** if it fails nondeterministically without code changes. Sources: clock time, thread scheduling, network latency.
- At 0.1% flake rate with 10,000 tests/day → 10 flakes to investigate daily
- At ~1% flakiness, tests begin to **lose value** entirely
- Google's flake rate hovers around **0.15%**—requires active investment to maintain
- Rerunning flaky tests trades CPU cycles for engineer time; only a stopgap

## Code Coverage
Code coverage measures which lines are exercised by tests—useful but **not a gold standard**.
- Coverage only measures invocation, not correctness of behavior
- Teams that set 80% as a bar often treat it as a **ceiling** rather than a floor
- Better approach: think about **behaviors tested**, not lines covered
- Recommend measuring coverage from **small tests only** to avoid inflation

## History of Testing at Google
Three initiatives drove the testing revolution (2005-2006):
- **Orientation Classes**: taught testing to all new hires; within 1-2 years, testing-aware engineers outnumbered the old guard
- **Test Certified**: 5-level certification program for teams; helped 1,500+ projects; replaced by **Project Health (pH)** in 2015
- **Testing on the Toilet (TotT)**: one-page flyers in restroom stalls; longest-running and most impactful initiative

## The Limits of Automated Testing
Not all testing can be automated. Human judgment is still needed for:
- Search quality evaluation (Search Quality Raters)
- Audio/video quality assessment
- Complex security vulnerability discovery
- **Exploratory Testing**: treating the app as a puzzle, finding unanticipated behaviors

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Beyoncé Rule" | **"If you liked it, you shoulda put a test on it"**—test everything you don't want to break |
| Small test constraint | **Single process**, no I/O, no sleep, no network |
| Medium test constraint | Single **machine**, can use localhost network |
| Google's recommended test mix | **80% unit, 15% integration, 5% E2E** |
| "Ice cream cone" anti-pattern | Too many **E2E tests**, few unit tests—slow, unreliable |
| Flaky test threshold | Tests begin losing value at **~1%** flakiness |
| Code coverage pitfall | Engineers treat the bar as a **ceiling**, not a floor |
| GWS result | Emergency pushes dropped **50%** after mandating tests |
| Test Certified replacement | **Project Health (pH)** tool, scale 1-5 |
| TotT | **Testing on the Toilet**—one-page flyers in restrooms |

## Related Notes
- [[Unit Testing]]
- [[Test Doubles]]
- [[Larger Testing]]
- [[Deprecation]]
- [[Software Engineering Fundamentals]]
- [[Code Review]]
- [[Continuous Integration]]

---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: continuous integration, CI, TAP, presubmit, post-submit, hermetic testing, Build Cop
---

# Continuous Integration (Importance: ★★★)

#continuous-integration #tools

*Reading time: ~5 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| CI Definition | Continuous assembling and testing of the entire evolving ecosystem |
| Fast Feedback Loops | Catch problems early; cost grows exponentially the later a bug is found |
| Continuous Build (CB) | Integrates latest code at head; runs automated build and tests |
| Continuous Delivery (CD) | Assembles release candidates; promotes through environments |
| Continuous Testing (CT) | Tests at every stage: presubmit → post-submit → RC → production |
| TAP | Google's Test Automation Platform; 50,000+ changes/day, 4B+ test cases |
| Hermetic Testing | Self-contained test environment; no external dependencies |
| CI Is Alerting | CI and SRE monitoring share the same conceptual framework |

## CI Definition
**Continuous Integration (CI)**: the continuous assembling and testing of our entire complex and rapidly evolving ecosystem. The fundamental goal is to **catch problematic changes as early as possible**.
- Modern CI goes beyond code: upstream microservices, data ingestion, ML models, OS, runtimes, and devices are all "dependencies" to integrate
- CI informs: **which tests** to run **when** in the workflow, and how to compose the **system under test (SUT)**

## Fast Feedback Loops
The cost of a bug grows **almost exponentially** the later it is caught. CI encourages fast feedback loops, from fastest to slowest:
- Edit-compile-debug (local development)
- Automated test results on **presubmit**
- Integration error on **post-submit** between two projects
- Incompatibility with upstream microservice in **staging**
- Bug reports by internal users
- Bug reports by external users or the press
- **Canarying** (deploying to a small percentage first) mitigates production issues

## Continuous Build (CB)
Integrates the latest code changes at **head** and runs automated build and tests.
- Introduces two versions of head: **true head** (latest commit) and **green head** (latest CB-verified change)
- Engineers sync to green head for stability; submit against true head
- "Breaking the build" includes breaking tests, not just compilation

## Continuous Delivery (CD)
**Release candidate (RC)**: a cohesive, deployable unit assembled from code, configuration, and dependencies that passed CB.
- CD continuously assembles RCs, then promotes and tests them through **a series of environments** (dev → staging → production)
- **Configuration must be included** in the RC — large percentage of production bugs are "silly" configuration problems
- **Version skew** is often caught during RC promotion

## Continuous Testing (CT)

### Presubmit vs. Post-submit
- **Presubmit**: Run only **fast, reliable** tests (typically unit tests). Accept some loss of coverage. Average wait time at Google: ~11 minutes.
- **Post-submit**: Run all potentially affected tests asynchronously. Accept longer times and some instability.
- Running all tests on presubmit is **too expensive** — blocks engineer productivity, suffers from instability, and risks **mid-air collisions**

### Release Candidate Testing
Run comprehensive automated tests against the RC, even if CB already ran them. Reasons: **sanity check**, auditability, cherry-pick divergence, emergency pushes.

### Production Testing
Run the same test suite against **production** (probers) to verify working state and test relevance.

## CI Is Alerting
CI and SRE monitoring share the same conceptual framework — both **identify problems automatically as soon as possible**.
- CI catches problems by surfacing **test failures**; alerting catches problems by monitoring **metrics**
- A flaky test is analogous to a **spurious alert** — both waste time and erode confidence
- Localized signals (unit tests / cause-based alerts) vs. cross-dependency signals (integration tests / black-box probing)
- **100% green CI** is as expensive as 100% uptime — apply **error-budget** thinking
- Not all test failures indicate upcoming production issues; be liberal in disabling irrelevant failing tests

## CI Challenges
- **Presubmit optimization**: Which tests to run, balancing coverage vs. speed
- **Culprit finding**: Identifying which change broke a test; complicated by batching and flakes
- **Resource constraints**: Large tests are expensive; infrastructure costs are considerable
- **Failure management**: Flaky tests erode confidence; end-to-end tests frequently break; need mechanisms to temporarily disable and track

## Hermetic Testing
**Hermetic tests**: run against a self-contained test environment with no external dependencies. Two key properties:
- **Determinism** (stability): input doesn't change based on outside deps; same code = same results
- **Isolation**: production problems don't affect tests; test problems don't affect production
- Types: **fakes** (cheaper but limited fidelity), **fully sandboxed stacks**, **record/replay** (caches backend responses)
- Record/replay trade-off: false positives (hitting cache too much) vs. false negatives (hitting cache too little)

### Google Assistant Case Study
Made presubmit **fully hermetic**: cut runtime by 14x, virtually no flakiness. Previously, 50+ changes/day bypassed non-hermetic test results. Post-submit uses **hotswapping** to isolate failures to individual microservices at O(N) cost instead of O(N²).

## TAP: Google's Global Continuous Build
The **Test Automation Platform (TAP)** handles 50,000+ unique changes and 4B+ test cases per day.
- **Presubmit optimization**: Teams create fast test subsets; changes passing presubmit have 95%+ chance of passing all tests
- **Culprit finding**: TAP splits failing batches into individual changes; binary search tools available
- **Failure management**: Build Cop drops everything to fix breaks; **rollback** is preferred (two clicks at Google); TAP can auto-rollback
- **Resource constraints**: Forge (distributed build-and-test) runs tests; TAP uses dependency graph analysis to run **minimal set** of affected tests

## CI Case Study: Google Takeout
Takeout grew from a backup tool to a platform for 90+ Google products. Four CI evolution scenarios:
1. **Broken dev deploys**: Sandboxed presubmit environments prevented 95% of bad-config broken servers, reduced deploy failures by 50%
2. **Indecipherable test logs**: Dynamic parameterized test suite with friendly UI; reduced team debugging involvement by 35%
3. **Debugging "all of Google"**: Running same tests against production isolates Takeout failures from upstream failures
4. **Keeping it green**: Bug-tagging to suppress known failures; automated cleanup when bugs are fixed; feature-flag-aware test expectations → self-maintaining test suite

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Presubmit vs post-submit" | Presubmit = **fast, reliable** only; post-submit = **comprehensive** |
| "Green head vs true head" | Green = **CB-verified**; true = latest commit |
| "Mid-air collision" | Two unrelated changes conflict; rare but daily at Google's scale |
| "Hermetic testing" | Self-contained, no external deps; **deterministic + isolated** |
| "Record/replay" | Cache backend responses; trade-off: **false positives vs false negatives** |
| "CI is alerting" | Same framework as SRE monitoring; apply **error-budget** thinking |
| "Build Cop" | Drops everything to fix breaks; **rollback** is preferred action |
| "TAP" | 50K+ changes/day, 4B+ tests; dependency graph for **minimal test set** |
| "Flaky tests" | Erode confidence like spurious alerts; remove from presubmit temporarily |

## Related Notes
- [[Large-Scale Changes]]
- [[Continuous Delivery]]
- [[Testing Overview]]
- [[Version Control]]
- [[Build Systems]]
- [[Dependency Management]]
- [[Software Engineering Fundamentals]]

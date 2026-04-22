---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: larger-testing, integration-tests, fidelity, SUT, hermetic-testing, end-to-end, flakiness
---

# Larger Testing (Importance: ★★☆)

#larger-testing #testing #processes #hermetic-testing

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Why Larger Tests | Address **fidelity gaps** that unit tests cannot cover |
| Fidelity | How reflective the test is of real production behavior |
| SUT (System Under Test) | The system being tested; scope drives test scope |
| Hermetic SUT | Isolated from other tests/traffic—most reliable |
| Common Gaps in Unit Tests | Unfaithful doubles, config issues, load issues, emergent behaviors |
| Test Pyramid Position | Top 20%: integration (15%) + E2E (5%) |
| Key Challenge | Slower, flakier, harder to own and standardize than unit tests |
| Chained Tests | Smaller pairwise integration tests representing an overall scenario |
| Record/Replay | Generate smaller tests by recording traffic from larger tests |
| A/B Diff | Compare responses between old and new versions; most common large test at Google |

## Why Larger Tests Exist
Unit tests can't cover everything. Larger tests address **fidelity**—how reflective a test is of real production behavior.
- Unit tests run in isolation (the "vacuum effect") and miss cross-component interactions
- Larger tests provide confidence that the **overall system** works as intended
- Automated larger tests scale in ways **manual testing does not**

## Common Gaps in Unit Tests
- **Unfaithful doubles**: mocks created by the engineer writing the test, who may be misinformed about actual behavior of the mocked dependency; mocks become stale when real implementations change
- **Configuration issues**: deployment configs, starter scripts, config databases—unit tests can't verify binary-config compatibility. **Config changes are Google's #1 cause of major outages**
- **Issues under load**: performance, load, and stress testing require large traffic volumes that unit tests can't generate
- **Unanticipated behaviors**: unit tests can only test anticipated behaviors; real users find mostly unanticipated issues (Hyrum's Law)
- **Emergent behaviors ("vacuum effect")**: unit tests deliberately eliminate real-world chaos; they miss defects from inter-component interactions

## Challenges of Larger Tests
Larger tests violate the properties of developer-friendly tests:
- **Not fast**: timeouts of 15 minutes to hours or even days
- **Not hermetic**: may share resources with other tests and traffic
- **Not deterministic**: shared environments make determinism nearly impossible
- **Ownership problems**: tests span multiple teams; without clear ownership, tests rot
- **Lack of standardization**: unlike unit tests, no standard infrastructure—different platforms, languages, frameworks per team

## SUT Scope (System Under Test)
The SUT is the primary driver of test scope. Forms ranked by hermeticity/fidelity trade-off:
- **Single-process SUT**: entire system in one binary; least faithful but can be "small" test
- **Single-machine SUT**: separate binaries on one machine; "medium" tests
- **Multimachine SUT**: distributed across machines like production; "large" tests, more flaky
- **Shared environments** (staging/production): lowest cost but highest conflict risk
- **Hybrids**: run some SUT locally, interact with shared backends

## Benefits of Hermetic SUTs
- Production tests can't block releases (too late in the pipeline)
- Shared staging environments don't scale with growing teams
- Cloud-isolated or machine-hermetic SUTs avoid **reservation conflicts** and allow earlier test execution

## The Smallest Possible Test
Even for integration tests, **smaller is better**. Techniques:
- **Chain tests**: create multiple smaller pairwise integration tests; output of one becomes input to the next via a data repository
- **Reduce SUT size** at problem boundaries: split UI and backend tests at the API boundary
- Replace databases with in-memory versions; remove out-of-scope servers

## Record/Replay Proxies
A larger "Record Mode" test runs post-submit and records traffic to external services. A smaller "Replay Mode" test replays that traffic during development/presubmit.
- Uses **matchers** to match requests to recorded responses (similar to stubs/mocks)
- When client behavior changes significantly, engineer must re-record

## Types of Larger Tests
| Type | SUT | Data | Verification |
|------|-----|------|-------------|
| Functional testing | Single-machine hermetic / cloud-isolated | Handcrafted | Assertions |
| Browser/device testing | Hermetic with frontend | Handcrafted | Assertions |
| Performance/load/stress | Cloud-deployed isolated | Handcrafted / production multiplexed | Diff (metrics) |
| Deployment config testing | Hermetic or cloud-isolated | None | Assertions (doesn't crash) |
| Exploratory testing | Production / staging | Production data | Manual |
| A/B diff regression | Two cloud environments | Production multiplexed / sampled | A/B diff comparison |
| UAT | Hermetic or cloud-isolated | Handcrafted | Assertions |
| Probers/canary analysis | Production | Production | Assertions + A/B diff (metrics) |
| Disaster recovery / chaos | Production | Production + fault injection | Manual + A/B diff (metrics) |

## A/B Diff Testing
The **most common form of larger testing at Google**, dating to 2001.
- Send traffic to both old and new versions; compare responses
- Deviations must be reconciled as anticipated or unanticipated (regressions)
- Variants: A-A testing (detect nondeterminism), A-B-C testing (production + baseline + pending change)
- Limitations: requires manual approval of diffs, noise management, sufficient traffic coverage

## Disaster Recovery and Chaos Engineering
- Google runs annual **DiRT** (Disaster Recovery Testing): inject faults at planetary scale (datacenter fires, malicious attacks, earthquake simulation)
- **Chaos engineering** (inspired by Netflix, Google uses "Catzilla"): continuous background fault injection; thousands of chaos tests per week
- Designed to break assumptions of stability and build resiliency

## Large Tests and the Developer Workflow
- Integrate into presubmit and post-submit pipelines even if not in TAP
- **A/B diff**: require approval of diffs as part of code review
- **Speed up**: reduce scope, replace sleeps with polling/event handlers, lower internal timeouts for test environments, optimize build time
- **Drive out flakiness**: use hermetic SUTs, avoid time-based sleeps, make failure modes obvious
- **Clear ownership**: without owners, tests rot; use code ownership, per-test annotations

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| Why larger tests exist | Address **fidelity gaps** unit tests cannot cover |
| Google's #1 outage cause | **Configuration changes** |
| Hermetic SUT benefit | **Isolated** from other tests; most reliable |
| Smallest possible test | **Chain** pairwise integration tests; reduce SUT scope |
| Most common large test at Google | **A/B diff regression** testing |
| Record/replay purpose | Generate **smaller replay tests** from larger record tests |
| DiRT | **Disaster Recovery Testing**—annual, planetary-scale fault injection |
| Chaos engineering at Google | **Catzilla**—thousands of chaos tests per week |
| Flakiness mitigation | Hermetic SUT, polling over sleeping, event handlers |
| Webdriver Torso | Risk of production test data being **discovered by end users** |

## Related Notes
- [[Testing Overview]]
- [[Unit Testing]]
- [[Test Doubles]]
- [[Deprecation]]
- [[Continuous Integration]]
- [[Software Engineering Fundamentals]]

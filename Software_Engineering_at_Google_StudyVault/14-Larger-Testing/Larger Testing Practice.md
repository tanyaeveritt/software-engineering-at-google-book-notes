---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, larger-testing, fidelity, hermetic, SUT, A/B-diff
---

# Larger Testing Practice (10 questions)

#practice #larger-testing #testing

## Related Concepts
- [[Larger Testing]]
- [[Testing Overview]]
- [[Test Doubles]]
- [[Unit Testing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Why larger tests | Address **fidelity gaps** |
> | #1 outage cause | **Configuration changes** |
> | Hermetic SUT | Isolated, most reliable |
> | Most common large test | **A/B diff** regression |
> | DiRT | Annual disaster recovery testing |

---

## Question 1 - Fidelity [recall]
> What is "fidelity" in the context of larger testing, and why is it the primary reason larger tests exist?

> [!answer]- Show Answer
> **Fidelity** is the property by which a test is reflective of the real behavior of the system under test. It is the primary reason larger tests exist because unit tests, while fast and reliable, bundle test and code in a way that is very different from how production code runs. Higher-fidelity environments (integration, staging, production) more closely reflect reality. Unit tests operate in a "vacuum"—deliberately eliminating real dependencies, network chaos, and data—which means they miss entire categories of defects.

---

## Question 2 - Common Gaps [recall]
> Name five specific gaps in unit test coverage that larger tests can address.

> [!answer]- Show Answer
> 1. **Unfaithful doubles**: mocks written by engineers unfamiliar with the actual behavior of the dependency; mocks become stale over time
> 2. **Configuration issues**: deployment configs, database configs, starter scripts—unit tests can't verify binary-config compatibility
> 3. **Issues under load**: performance, stress, and load testing require volumes impossible in unit tests
> 4. **Unanticipated behaviors/inputs**: unit tests only cover anticipated behaviors; real users find mostly unanticipated issues (Hyrum's Law)
> 5. **Emergent behaviors and the "vacuum effect"**: unit tests eliminate real-world chaos (network, data, dependencies), missing defects from cross-component interactions

---

## Question 3 - SUT Types [recall]
> Describe the five forms of SUT (System Under Test) at Google, from most hermetic to least.

> [!answer]- Show Answer
> 1. **Single-process SUT**: entire system packaged into one binary; can be "small" test; least faithful to production topology
> 2. **Single-machine SUT**: separate binaries on one machine, like production; "medium" tests; higher fidelity
> 3. **Multimachine SUT**: distributed across multiple machines like production cloud deployment; "large" tests; susceptible to network/machine flakiness
> 4. **Shared environments** (staging/production): lowest cost (environments already exist) but tests conflict with other uses; highest risk of end-user impact
> 5. **Hybrids**: mix of locally-run SUT with shared backends; practical necessity at Google's scale where running all services is impossible

---

## Question 4 - Configuration Outages [recall]
> What does Google identify as their number one reason for major outages, and why are unit tests insufficient for this problem?

> [!answer]- Show Answer
> **Configuration changes** are Google's number one reason for major outages. Unit tests are insufficient because they cover code within a binary but cannot verify compatibility between the binary and its deployment configuration (config files, databases, option definitions). Configs are often written in different languages than production code, have faster rollout cycles, and are more difficult to test. In 2013, a global Google outage resulted from a bad network configuration push that was never tested. Larger tests that exercise the actual binary with its configuration are needed.

---

## Question 5 - Chained Tests [application]
> A user journey involves five internal services in sequence. An end-to-end test covering all five is slow and flaky. How would you apply Google's "smallest possible test" principle?

> [!answer]- Show Answer
> Apply **chained tests**: instead of one enormous end-to-end test, create **five smaller pairwise integration tests**, one for each service-to-service interaction. Ensure the output of one test is persisted to a data repository and used as the input to the next test. This breaks the massive test into manageable pieces that are:
> - Faster to run (smaller SUT per test)
> - Easier to debug (failures localized to specific service pairs)
> - Less flaky (fewer moving parts per test)
> Additionally, reduce SUT size at **problem boundaries**: split frontend/backend tests at the API boundary, replace databases with in-memory versions, and remove out-of-scope servers.

---

## Question 6 - A/B Diff Testing [application]
> Your team is migrating a search service to a new backend. The old API has thousands of implicit behaviors users depend on (Hyrum's Law). What testing approach would you use to catch unanticipated regressions?

> [!answer]- Show Answer
> Use **A/B diff regression testing**—the most common form of larger testing at Google for this exact situation. Deploy two SUTs: one running the old backend (baseline) and one running the new backend (candidate). Send the same production-multiplexed or sampled traffic to both and compare responses. Any deviations must be manually reconciled as anticipated or unanticipated (regressions).
> Consider also **A-A testing** (comparing the system to itself) first to identify nondeterministic behavior and noise that should be excluded from the A-B diff. You could even use **A-B-C testing** (production + baseline + pending change) to see both the immediate impact and cumulative impact of the migration.

---

## Question 7 - Webdriver Torso [application]
> What lesson does the Webdriver Torso incident teach about testing in production?

> [!answer]- Show Answer
> Google created automated scripts to generate test videos, upload them to YouTube, and verify rendering quality. The channel (Webdriver Torso) was public, and the test videos were discovered by journalists and media, becoming a viral mystery. The lesson: when testing in production, you must **think about the possibility of end-user discovery** of any test data you include and be prepared for it. Production testing carries inherent risks including user-visible side effects, nondeterminism from real data changes, and the possibility that test artifacts become public. It's another reason to prefer **hermetic SUTs** when possible.

---

## Question 8 - Flakiness in Large Tests [recall]
> What are the main strategies for driving out flakiness in larger tests?

> [!answer]- Show Answer
> 1. **Reduce scope**: use hermetic SUT (avoids multi-user and real-world flakiness) and single-machine hermetic SUT (avoids network/deployment flakiness)
> 2. **Replace sleeps with reactive approaches**: polling for state transitions at microsecond frequency, implementing event handlers, subscribing to notification systems
> 3. **Tune internal timeouts**: increase them in test environments to account for slower test infrastructure, but make timeouts configurable rather than hardcoded
> 4. **Make failure modes obvious**: production systems may gracefully hide failures (e.g., not serving an ad vs. returning a 500), which confuses test runners—make the failure mode explicit
> 5. **Balance speed vs. reliability**: accept trade-offs between test execution speed and flakiness

---

## Question 9 - Ownership [analysis]
> A large integration test spanning three teams' services starts failing intermittently. No team claims ownership. Over time, fewer engineers investigate failures. Analyze this situation using Google's guidance on large test ownership.

> [!answer]- Show Answer
> This is the **ownership problem** that Google identifies as a primary challenge for larger tests. Without clear ownership:
> - It becomes more difficult for contributors to **modify and update** the test
> - It takes longer to **resolve failures**
> - The test **rots** and eventually provides zero value (or gets deleted, creating a coverage gap)
> Google's solution: larger tests **must have documented owners**. Integration tests of components within a project should be owned by the **project lead**. Feature-focused tests across services should be owned by a **feature owner** (engineer, PM, or test engineer). Owners must be empowered with both the **ability to maintain** the test and the **incentives to do so**. Google uses OWNERS files and per-test annotations to make ownership discoverable by automation.

---

## Question 10 - Larger Tests and Time [analysis]
> Google observes that larger tests are valuable for longer-lived software, but the "ice cream cone" anti-pattern often develops in early-stage projects. Explain why this happens and how to prevent it.

> [!answer]- Show Answer
> The ice cream cone develops because development typically **starts with manual testing**—engineers hack on code and test by running it manually. This is natural for code expected to last only minutes. But as the prototype becomes functional and is shared, manual tests accumulate and dominate. If the code was built without testability (no seams for test doubles, tight coupling), the only automated tests possible are end-to-end tests, creating **legacy code within days**.
> Prevention: **Move toward the test pyramid within the first few days** of development. Build unit tests first (make them a submission requirement), then add integration tests, and progressively replace manual end-to-end testing with automated tests. The longer you wait, the harder and more expensive it becomes. Design for **testability** from the start—it's much cheaper than retrofitting.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Fidelity | How closely test reflects **real production behavior** |
> | #1 outage cause | **Configuration changes** |
> | Hermetic SUT | Isolated from other tests; most reliable |
> | SUT types (5) | Single-process, single-machine, multi-machine, shared, hybrid |
> | Chained tests | Pairwise integration tests with persisted output |
> | Most common large test | **A/B diff** regression testing |
> | A-A testing | Detect **nondeterminism** before A/B diff |
> | DiRT | Annual planetary-scale **disaster recovery testing** |
> | Catzilla | Google's continuous **chaos engineering** system |
> | Flakiness fix | Hermetic SUT + polling + tunable timeouts |
> | Ownership requirement | Documented owners with **incentive and ability** |

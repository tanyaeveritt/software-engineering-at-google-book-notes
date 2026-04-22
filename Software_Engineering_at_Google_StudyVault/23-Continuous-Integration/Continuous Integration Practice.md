---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, continuous integration, TAP, presubmit, hermetic testing, Build Cop
---

# Continuous Integration Practice (10 questions)

#practice #continuous-integration

## Related Concepts
- [[Continuous Integration]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Presubmit | **Fast, reliable** tests only (unit tests); ~11 min at Google |
> | Post-submit | **All** potentially affected tests; async |
> | Green head | Latest CB-verified change; **not** true head |
> | Hermetic test | Self-contained; **deterministic + isolated** |
> | Build Cop | Responsible for fixing breaks; **rollback** preferred |
> | CI is alerting | Same framework as SRE; apply **error-budget** thinking |

---

## Question 1 - CI Definition [recall]
> What is the modern definition of Continuous Integration, and how does it differ from the traditional definition?

> [!answer]- Show Answer
> Traditional: "Members of a team integrate their work frequently, verified by automated build and test."
>
> Modern (at scale): **The continuous assembling and testing of our entire complex and rapidly evolving ecosystem.** This goes beyond code changes to include upstream microservices, data ingestion, ML models, operating systems, runtimes, and devices — all considered dependencies that should be "continuously integrated."

---

## Question 2 - Green Head vs True Head [recall]
> What is the difference between "green head" and "true head," and why does this distinction matter?

> [!answer]- Show Answer
> - **True head**: The very latest change committed to the repository
> - **Green head**: The latest change that the **Continuous Build (CB) has verified** as passing all tests
>
> This matters because engineers sync to **green head** for a stable development environment. Changes must sync to **true head** before submission. The gap between them represents commits that haven't been fully validated yet, enabling velocity while maintaining a known-good baseline.

---

## Question 3 - Presubmit vs Post-submit [recall]
> What is Google's general rule for which tests to run on presubmit vs. post-submit?

> [!answer]- Show Answer
> **Presubmit**: Only **fast, reliable** tests — typically unit tests scoped to the changed project. Accept some loss of coverage. Average wait: ~11 minutes at Google, run in background.
>
> **Post-submit**: Run **all potentially affected tests** asynchronously. Accept longer times and some instability. Use proper mechanisms (Build Cop, rollbacks) to deal with failures.
>
> Running all tests on presubmit is too expensive: blocks productivity, flaky tests disrupt unrelated engineers, and **mid-air collisions** (incompatible concurrent changes) happen daily at scale.

---

## Question 4 - Hermetic Testing Properties [recall]
> What are the two key properties of hermetic tests, and what types of hermetic backends exist?

> [!answer]- Show Answer
> Two properties:
> 1. **Determinism (stability)**: Input doesn't change from outside deps; same code and tests produce same results
> 2. **Isolation**: Production problems don't affect tests; test problems don't affect production
>
> Types of hermetic backends:
> - **Fakes**: Cheaper but limited fidelity; require maintenance
> - **Fully sandboxed stacks**: Cleanest option; start the entire stack in a sandbox (DisplayAds runs ~400 servers per presubmit)
> - **Record/replay**: Cache live backend responses and replay them; trade-off between false positives (cache hit too much) and false negatives (cache miss too much)

---

## Question 5 - Build Cop Role [recall]
> What is a Build Cop, and what is their most effective tool?

> [!answer]- Show Answer
> A **Build Cop** is responsible for keeping all tests passing in their project, regardless of who breaks them. When notified of a failing test, they **drop whatever they are doing** and fix the build.
>
> Their most effective tool is the **rollback** — the fastest and safest way to restore a known good state. At Google, any change can be rolled back with **two clicks**. TAP has been upgraded to **automatically roll back** changes when it has high confidence they are the culprit. Tests give confidence to change; rollbacks give confidence to undo.

---

## Question 6 - Takeout CI Scenario [application]
> Your team's end-to-end tests break nightly deploys, but they can't run on presubmit due to security constraints. Using the Takeout CI case study, describe a strategy to get faster feedback.

> [!answer]- Show Answer
> Follow Takeout's Scenario #1 strategy:
> 1. Create **sandboxed mini-environments** that run on presubmit to test server startup health — this prevents 95% of broken servers from bad configuration
> 2. For end-to-end tests that can't run on presubmit: create a **post-submit CI** running every 2 hours (not nightly), grabbing latest code from green head, creating an RC, and running end-to-end tests
> 3. Post-submit is **security-compliant** (code is already approved), so test accounts can be used
> 4. This cuts the "culprit set" by **12x** (2-hour window vs. 24-hour window)
>
> The key lesson: **Faster feedback loops prevent problems** — move tests as far left as possible, even if not all the way to presubmit.

---

## Question 7 - CI Is Alerting [application]
> Apply the "CI is alerting" insight to argue why a policy of "nobody can commit if CI isn't green" is problematic.

> [!answer]- Show Answer
> In SRE practice, aiming for **100% uptime** is recognized as prohibitively expensive — teams use **error budgets** instead. The same logic applies to CI:
>
> 1. Having **100% green CI** is awfully expensive; the biggest problem would be race conditions between testing and submission
> 2. Not all test failures indicate production issues — just as spurious alerts are silenced in production, irrelevant test failures should be investigated but not necessarily block commits
> 3. If root cause is well understood and clearly won't affect production, **blocking commits is unreasonable**
> 4. Flaky tests are analogous to **spurious alerts** — they erode confidence but don't represent real problems
>
> The policy should be: investigate failures before compounding them, but use judgment about whether they truly warrant blocking all development.

---

## Question 8 - TAP Resource Management [recall]
> How does TAP manage the massive resource requirements of testing 50,000+ changes per day?

> [!answer]- Show Answer
> TAP uses several strategies:
> 1. **Dependency graph analysis**: Uses the near-real-time global dependency graph (from Forge and Blaze) to run only the **minimal set** of downstream affected tests for each change
> 2. **Batching**: Groups related changes together, reducing total unique tests to run
> 3. **Speed bias**: Changes with fewer tests can run sooner, encouraging engineers to write **small, focused changes**
> 4. **Presubmit optimization**: Only fast unit test subsets on presubmit; comprehensive tests on post-submit
> 5. **Distributed execution**: Tests run on Forge (distributed build-and-test in Google's datacenters) maximizing parallelism

---

## Question 9 - Record/Replay Trade-offs [analysis]
> A team is implementing record/replay for their hermetic tests. Analyze the false positive vs. false negative trade-off and propose an ideal behavior.

> [!answer]- Show Answer
> **False positives** (test passes when it shouldn't): Occur when hitting the cache **too much**. The test misses real problems that would surface when capturing a fresh response. Risk: bugs slip through to production.
>
> **False negatives** (test fails when it shouldn't): Occur when hitting the cache **too little**. Requires response updates, which are time-consuming and often submit-blocking. Risk: developer friction and wasted debugging time.
>
> **Ideal behavior**: The system should detect only **problematic changes** and cache-miss only when a request has changed **in a meaningful way**. If a change causes a problem, the author reruns with an updated response and sees the test still failing — alerting them to a real issue. In practice, determining "meaningful change" is incredibly difficult in a large, ever-changing system, making this an ongoing research problem.

---

## Question 10 - Keeping Tests Green [analysis]
> The Takeout team had a test suite that was nearly always broken due to 90+ product plug-in failures. Analyze their multi-layered strategy for achieving and maintaining a green test suite.

> [!answer]- Show Answer
> Takeout's strategy had four layers:
>
> 1. **Bug-tagging**: Failing tests were tagged with a bug and assigned to the responsible team. The framework **suppressed tagged failures**, keeping the suite green while tracking known issues.
>
> 2. **Feature-flag awareness**: Engineers could specify feature flags along with expected output both with and without the feature. Tests queried the environment to determine which output to verify, eliminating rollout-related failures.
>
> 3. **Automated cleanup**: Tests checked whether tagged bugs were closed. If a tagged-failing test was passing for longer than a configured limit, it prompted tag cleanup. Exception: flaky tests kept their tags without prompting.
>
> 4. **Result**: A **mostly self-maintaining** test suite that maintained confidence ("everything besides known issues is passing"), tracked accountability (filed bugs), and prevented technical debt accumulation.
>
> Key metric: **MTTCU** (mean time to clean up) — the DevOps-style metric for how quickly fixed bugs have their test tags removed.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Presubmit | **Fast, reliable** only; ~11 min; 95%+ pass rate predicts full pass |
> | Post-submit | All affected tests, async; Build Cop manages failures |
> | Green head | Latest **CB-verified** change |
> | Hermetic | Self-contained; **deterministic + isolated** |
> | Record/replay | Cache responses; balance **false positives vs. false negatives** |
> | CI is alerting | Same framework as SRE; **error budgets** apply |
> | Build Cop | Fix breaks immediately; **rollback** = preferred tool |
> | TAP | Dependency graph → **minimal test set**; 50K changes/day |
> | Culprit finding | Split batches + binary search; **auto-rollback** |
> | Keeping green | Bug-tag, suppress, auto-clean → **self-maintaining** suite |

---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, continuous delivery, feature flags, release train, A/B testing, deployment
---

# Continuous Delivery Practice (10 questions)

#practice #continuous-delivery

## Related Concepts
- [[Continuous Delivery]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Faster is safer | Smaller batches = **higher quality** |
> | Feature flag | Decouple feature launch from **binary release** |
> | Release train | Fixed cadence; miss it → hours, **not days** |
> | A/B deployment | Update + placebo comparison for **statistical proof** |
> | Ship what's used | Dynamic deployment + A/B for **cost vs. value** trade-off |
> | Microservice migration | Best ROI for **long-term velocity** at scale |

---

## Question 1 - Core Tenet [recall]
> What is the core tenet of Continuous Delivery, and why does it seem counterintuitive?

> [!answer]- Show Answer
> The core tenet: **faster is safer** — over time, smaller batches of changes result in higher quality. This seems counterintuitive because teams instinctively slow down releases to reduce risk. However, frequent small releases mean:
> - Each release has **fewer changes** to troubleshoot
> - Faster adaptation to **bugs and market shifts**
> - Lower per-release cost (driven down by repetition)
> - Time between "code complete" and **user feedback** is minimized
> Having CD structures in place generates the majority of the value even without pushing every release.

---

## Question 2 - Six Idioms of CD [recall]
> List and briefly define the six aspects of Continuous Delivery at Google.

> [!answer]- Show Answer
> 1. **Agility**: Release frequently and in small batches
> 2. **Automation**: Reduce or remove repetitive overhead of frequent releases
> 3. **Isolation**: Modular architecture to isolate changes and simplify troubleshooting
> 4. **Reliability**: Measure key health indicators (crashes, latency) and continuously improve them
> 5. **Data-driven decision making**: Use A/B testing on health metrics to ensure quality
> 6. **Phased rollout**: Roll out changes to a few users before shipping to everyone

---

## Question 3 - Feature Flags Purpose [recall]
> What is flag-guarding and what three problems does it solve?

> [!answer]- Show Answer
> Flag-guarding means wrapping all new features in a **feature flag** that can be dynamically toggled. It solves:
> 1. **Risk isolation**: If new code has a problem, the flag can be disabled independently from the binary release via dynamic config update, without a new deployment
> 2. **Press/launch timing**: Features can be turned on immediately before a press release, minimizing the risk of early discovery
> 3. **Coexistence**: Multiple features at different development stages can coexist in the same binary — development builds enable in-progress features while release builds don't
>
> Caveat: flag configuration changes must still be rolled out carefully (not 100% at once); a configuration service for safe rollouts is a good investment.

---

## Question 4 - Release Train at Google Search [recall]
> How did Google Search evolve its release process, and what two principles governed their trade-offs?

> [!answer]- Show Answer
> Evolution: Search went from releasing **once per week** (rarely hitting that target) to a continuous process releasing **every other day**. A dedicated group automated what they could, set feature deadlines, and simplified integration.
>
> Two principles:
> 1. **No binary is perfect**: Trade-offs are inevitable. KPI metrics with clear thresholds allow features to launch despite imperfections. Even edge-case bugs (like a rare Philippine dialect showing blank pages) must be carefully evaluated.
> 2. **Meet your release deadline**: If you're late for the release train, it leaves without you. With regular releases, the next train is only **hours** away, limiting developer panic and improving release engineer work-life balance.

---

## Question 5 - YouTube Antipattern [application]
> YouTube has a monolithic Python application with laborious releases, 50-hour manual QA, and nearly every release requiring cherry-picks. Applying CD principles, what approach has the best long-term return?

> [!answer]- Show Answer
> The investment with the best return is **migrating to a microservice architecture** — potentially even rewriting from scratch. Although either option takes months and is painful short-term, the value in terms of:
> - **Operational cost reduction**: Each service is independently deployable
> - **Cognitive simplicity**: Teams own smaller, well-bounded services
> - **Modularity**: Isolates failures and allows independent release cadences
>
> The wrong approach: reverting to traditional planning, adding more governance, implementing risk reviews, or rewarding only low-risk (and often low-value) features. These traditional operational fixes provide only **short-term stability** and slow velocity over time.

---

## Question 6 - A/B Deployments [recall]
> How does Google A/B test deployments (not just features), and why is this necessary?

> [!answer]- Show Answer
> Google sends out **two versions**: the desired update and a **placebo** (the old version re-shipped). Both roll out simultaneously to a large enough base of similar users. This allows comparing the new release against the old to determine if it's actually an improvement.
>
> Why necessary: On Android, simply pushing an update causes a **statistically significant change in user metrics** even with no code changes. So canarying alone tells you about crashes/stability but not whether the newer version is **actually better**. A/B deployment provides statistical proof of quality improvement, with results available within **days or even hours** for large userbases.
>
> For smaller userbases: use **change-neutral releases** where all new features are flag-guarded, testing only deployment stability.

---

## Question 7 - Ship Only What Gets Used [recall]
> What is the concept of "Ship Only What Gets Used" and how does it address software bloat?

> [!answer]- Show Answer
> **Bloat** is a side effect of most development lifecycles, especially for mobile apps where users pay in space, download, and data costs for unused features. "Ship Only What Gets Used" means:
> - Use **dynamic, configurable deployments** so apps maintain small sizes while shipping only code that provides user value
> - Use **A/B experiments** to intentionally trade off between a feature's cost and its value
> - Monitor the **cost and value** of every feature in the wild to know if it's still relevant
> - For mobile apps: separate how often a viable release is **created** from how often a user **receives** it
> This requires modular architecture and dedicated teams to improve product efficiency on an ongoing basis.

---

## Question 8 - Shifting Left [application]
> Your team develops for 2 billion+ diverse Android devices. Comprehensive release testing is infeasible. Applying Google's "shifting left" approach, design a release qualification strategy.

> [!answer]- Show Answer
> Accept device diversity as a **fact**, not a problem, and shift to:
> 1. **Representative testing**: Instead of testing every device, select representative device clusters
> 2. **Specialized testing tracks**: Use the Play Store's unlimited testing tracks to set up QA in each target country for overnight turnaround
> 3. **Staged rollouts**: Deploy to increasing percentages (1% → 5% → 25% → 100%), monitoring for issues at each stage
> 4. **Automated A/B releases**: Deploy update + placebo to statistically prove quality without humans watching dashboards
> 5. **Automated metrics pipeline**: Push forward to more traffic as soon as guardrail metrics have enough data, enabling the **fastest possible release**
>
> Key shift: from "test everything before release" to "release incrementally with real-world feedback loops."

---

## Question 9 - Protect Product from Developers [analysis]
> Google Maps says "One release responsibility is to protect the product from the developers." Analyze what this means in the context of CD and how it's implemented.

> [!answer]- Show Answer
> This principle recognizes that developer passion and urgency for launching features can **never trump user experience** with the existing product. Implementation:
>
> 1. **Interfaces with strong contracts**: New features must be isolated from other components
> 2. **Separation of concerns**: Changes in one feature shouldn't cascade to others
> 3. **Rigorous testing**: Automated tests verify that new features don't regress existing functionality
> 4. **Early communication**: Teams communicate feature plans ahead of release deadlines
> 5. **Conventions for acceptance**: Clear criteria for when a feature is ready for a release train
>
> The tension: features are very important, but very seldom is any feature so important that a release should be **held** for it. If releases are frequent, missing one causes small pain; delaying a release causes pain for **all** features in that release and for users waiting for fixes.

---

## Question 10 - CD Value Even Without Deploying [analysis]
> Google says "Simply having the structures in place that enable continuous deployment generates the majority of the value, even if you don't actually push those releases out to users." Analyze why this is true.

> [!answer]- Show Answer
> Having CD structures creates value because the **prerequisites** for CD are valuable in themselves:
>
> 1. **Robust, well-documented deployment process**: Forces teams to codify and automate what was previously tribal knowledge
> 2. **Real-time metrics**: Accurate, real-time metrics on user satisfaction and product health are valuable regardless of release frequency
> 3. **Coordinated team with clear policies**: Clarity on what makes it in or out of a release and why improves decision-making
> 4. **Configurable binaries**: Production-configurable binaries and config managed as code enable rapid response to issues
> 5. **Safety mechanisms**: Dry-run verification, rollback/rollforward, and reliable patching are independently valuable
>
> Google doesn't release wildly different versions of Search, Maps, or YouTube daily — but being **able to** requires all these structures, which make the organization more agile, responsive, and resilient to problems regardless of actual release cadence.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Faster is safer | Smaller batches = **higher quality**; faster market adaptation |
> | Feature flag | Decouple feature from release; **dynamic config update** |
> | Release train | Fixed cadence; **hours** between trains, not days |
> | No binary is perfect | KPI thresholds for launch decisions; **trade-offs inevitable** |
> | Microservice architecture | Best ROI for **long-term velocity** |
> | A/B deployment | Update + placebo → **statistical proof** of improvement |
> | Ship what's used | Dynamic deployments; monitor **cost vs. value** in the wild |
> | Protect from developers | User experience > developer **feature urgency** |
> | CD value | Structures alone generate most value; **readiness** matters |

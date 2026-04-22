---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: continuous delivery, CD, release trains, feature flags, A/B testing, deployment, velocity
---

# Continuous Delivery (Importance: ★★★)

#continuous-delivery #tools

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Core Tenet | Faster is safer — smaller batches = higher quality |
| Velocity | Bottlenecked by time to deployment; competitive advantage |
| Agility | Release frequently in small batches |
| Feature Flags | Flag-guard all changes to isolate features from release |
| Release Train | Fixed-cadence deployments; miss it, catch the next one |
| Phased Rollout | Deploy to small percentage first; staged rollouts |
| A/B Testing | Test deployments themselves with placebo control groups |
| Modularity | Microservice architecture enables independent releases |

## Faster Is Safer
The core tenet of CD and Agile: over time, **smaller batches of changes result in higher quality**. Counterintuitively, **faster is safer**.
- Products that release more frequently have **better quality outcomes**
- They adapt faster to bugs and market shifts
- Faster is also **cheaper**: predictable, frequent releases drive down per-release cost
- Simply having CD structures in place generates the majority of the value, **even without actually pushing every release**

## Idioms of Continuous Delivery at Google
Six aspects that deliver value independently en route to full CD:
- **Agility**: Release frequently and in small batches
- **Automation**: Reduce or remove repetitive overhead of frequent releases
- **Isolation**: Modular architecture to isolate changes and simplify troubleshooting
- **Reliability**: Measure key health indicators (crashes, latency) and keep improving them
- **Data-driven decision making**: Use A/B testing on health metrics to ensure quality
- **Phased rollout**: Roll out to a few users before shipping to everyone

## Velocity Is a Team Sport
As teams grow, an **antipattern** emerges: subteams branch off code, then struggle with integration and culprit finding. Google prefers **developing at head** in the shared codebase with CI testing, automatic rollbacks, and culprit finding.
- YouTube example: Monolithic Python app with laborious release process, 50-hour manual regression cycle, cherry-picks on almost every release
- The investment with the best return: **migrating to a microservice architecture** — sometimes rewriting from scratch
- This empowers large teams to remain **scrappy and innovative** while reducing risk

## Evaluating Changes in Isolation: Feature Flags
A key to reliable continuous releases: **flag guard all changes**.
- Multiple features at various development stages coexist in a binary
- New code lives alongside old code; if the new code works, remove the old codepath; if not, disable the flag independently from the binary release via **dynamic config update**
- Feature flags enable **decoupling** a feature's destiny from the overall product release
- Flags can be updated just before a press release, minimizing leak risk
- Caveat: flag configuration changes must still be rolled out carefully (not 100% at once)

## Release Trains
Google Search moved from weekly releases (rarely on time) to a continuous release process over several years.
- Streamlined everything: automated what possible, set deadlines for feature submission, simplified plug-in integration
- Result: consistently release a new Search binary **every other day**

### No Binary Is Perfect
Every release involves trade-offs. KPI metrics with **clear thresholds** allow features to launch even if imperfect and create clarity in contentious launch decisions.
- Even tiny user populations deserve reliable results (the Philippine island dialect story)

### Meet Your Release Deadline
If you're late for the release train, **it will leave without you**. Regular releases mean the next train is hours away, not days.
- Limits developer panic and improves **work-life balance** for release engineers
- Rare exceptions exist but are costly

## Quality and User-Focus: Ship Only What Gets Used
**Bloat** is an unfortunate side effect of most development cycles. Dynamic deployments allow:
- Ship only code that brings users value (no unused translations or architectures)
- **A/B experiments** allow intentional trade-offs between feature cost and user value
- For mobile apps: trade-off between native performance and update frequency
- Separating how often a viable release is **created** from how often a user **receives** it

## Shifting Left: Data-Driven Decisions Earlier
With diverse client markets (billions of Android devices), comprehensive testing is infeasible.
- Aim for **representative testing** instead
- **Staged rollouts** to increasing percentages allow fast fixes
- **Automated A/B releases**: send update + placebo (old version reshipped) to statistically compare
- For large enough userbases, statistically significant results within **days or hours**
- For smaller userbases: aim for **change-neutral releases** (all features flag-guarded)

## Changing Team Culture
As teams scale, release complexity increases superlinearly. "Always Be Deploying" returns development to effective form.
- Features are important, but seldom so important that a release should be held for one
- **One release responsibility is to protect the product from the developers** — new features must be isolated via interfaces with strong contracts
- Conventions for new feature acceptance, rigorous testing, early communication

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Faster is safer" | Smaller batches = higher quality; adapt faster to bugs and market |
| "Feature flag" | Guard all changes; decouple feature launch from **binary release** |
| "Release train" | Fixed cadence; miss it → catch the next in **hours, not days** |
| "No binary is perfect" | KPI thresholds enable launch decisions; trade-offs are **inevitable** |
| "Velocity is a team sport" | Develop at head; **microservice architecture** for modularity |
| "A/B deployment" | Send update + placebo to statistically compare; **days** for results |
| "Ship only what gets used" | Dynamic deployments; A/B experiments on **feature cost vs. value** |
| "Always Be Deploying" | Having CD structures in place generates **most of the value** |

## Related Notes
- [[Continuous Integration]]
- [[Large-Scale Changes]]
- [[Testing Overview]]
- [[Version Control]]
- [[Dependency Management]]
- [[Software Engineering Fundamentals]]

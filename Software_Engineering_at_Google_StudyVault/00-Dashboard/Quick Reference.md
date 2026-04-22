---
source_pdf: swe_at_google.2.pdf
part: all
keywords: quick reference, cheat sheet, software engineering
---

# Quick Reference — Software Engineering at Google

#dashboard #quick-reference #swe-at-google

## Part I — Thesis → [[Software Engineering Fundamentals]]

| Concept | Key Point |
|---------|-----------|
| SW Engineering vs Programming | Engineering = programming **integrated over time** |
| Hyrum's Law | With enough users, all observable behaviors will be depended on |
| Beyoncé Rule | "If you liked it, you should have put a CI test on it" |
| Shifting Left | Finding problems earlier in the workflow reduces costs |
| Sustainability | Capable of reacting to valuable change over the software's life span |
| Trade-off dimensions | Financial, resource, personnel, transaction, opportunity, societal costs |

## Part II — Culture

### Teams → [[Team Culture and Collaboration]]
| Concept | Key Point |
|---------|-----------|
| Genius Myth | Solo-hero culture is harmful; software is a team effort |
| Bus Factor | Number of people who need to disappear before project stalls |
| Three Pillars | **Humility, Respect, Trust** (HRT) |
| Blameless Post-Mortem | Summary, timeline, cause, impact, action items, lessons learned |
| Googleyness | Thrives in ambiguity, values feedback, challenges status quo, user-first, team-cares, does the right thing |

### Knowledge Sharing → [[Knowledge Sharing]]
| Concept | Key Point |
|---------|-----------|
| Psychological Safety | Most important factor for effective teams |
| Information Islands | Fragmentation, duplication, skew across isolated groups |
| SPOF | Single point of failure when only one person has knowledge |
| Chesterton's Fence | Understand why something exists before removing it |
| Readability Process | Google's code review certification for language best practices |
| Scaling mechanisms | Group chats → mailing lists → YAQS → tech talks → documentation → code |

### Equity → [[Equity in Engineering]]
| Concept | Key Point |
|---------|-----------|
| Bias as Default | Without intentional design, systems inherit existing biases |
| Multicultural Capacity | Ability to work effectively across diverse groups |

### Leading Teams → [[Leading Teams]]
| Concept | Key Point |
|---------|-----------|
| Manager vs TL vs TLM | Three distinct leadership roles with different focus areas |
| Servant Leadership | Leader exists to serve the team, remove obstacles |
| Antipatterns | Hire pushovers, ignore low performers, be everyone's friend, compromise hiring bar |

### Leading at Scale → [[Leading at Scale]]
| Concept | Key Point |
|---------|-----------|
| Always Be Deciding | Identify blinders, identify trade-offs, iterate on decisions |
| Always Be Leaving | Build self-driving teams, divide problem space, delegate subproblems |
| Always Be Scaling | Manage cycle of success, protect energy, distinguish important vs urgent |

### Measuring Productivity → [[Measuring Productivity]]
| Concept | Key Point |
|---------|-----------|
| GSM Framework | **Goals → Signals → Metrics** for measuring engineering productivity |
| Triage first | Ask "is it even worth measuring?" before investing in metrics |

## Part III — Processes

### Style Guides → [[Style Guides and Rules]]
| Concept | Key Point |
|---------|-----------|
| Rules vs Guidance | Rules = mandatory, guidance = best practices/recommendations |
| Guiding principles | Optimize for the reader, be consistent, avoid error-prone constructs, concede to practicality |

### Code Review → [[Code Review]]
| Concept | Key Point |
|---------|-----------|
| Three approvals | Correctness (LGTM), code ownership (approval), readability |
| Benefits | Correctness, comprehension, consistency, culture, knowledge sharing |
| Best practices | Small changes, good descriptions, minimize reviewers, be polite |

### Documentation → [[Documentation]]
| Concept | Key Point |
|---------|-----------|
| Docs as code | Treat documentation like code: versioned, reviewed, maintained |
| WHO/WHAT/WHEN/WHERE/WHY | Every document should answer these five questions |
| Types | Reference, design docs, tutorials, conceptual, landing pages |

### Testing Overview → [[Testing Overview]]
| Concept | Key Point |
|---------|-----------|
| Test Size | Small (single-process), Medium (single-machine), Large (multi-machine) |
| Test Scope | Unit (single class/method), Integration (few components), End-to-end (whole system) |
| Test Certified | Google's internal testing maturity program (levels 1-5) |
| Testing on the Toilet | One-page flyers in bathroom stalls to spread testing knowledge |

### Unit Testing → [[Unit Testing]]
| Concept | Key Point |
|---------|-----------|
| DAMP not DRY | **Descriptive And Meaningful Phrases** — clarity over deduplication in tests |
| Test via public APIs | Don't test private methods directly |
| Test state, not interactions | Verify what happened, not how it happened |
| Unchanging tests | Tests should rarely need to change once written |

### Test Doubles → [[Test Doubles]]
| Concept | Key Point |
|---------|-----------|
| Prefer real implementations | Use fakes only when real objects are too slow/non-deterministic |
| Faking > Stubbing > Mocking | Fakes provide realistic behavior; stubbing is brittle; mocking over-specifies |
| Seams | Points where test doubles can be injected |

### Larger Testing → [[Larger Testing]]
| Concept | Key Point |
|---------|-----------|
| SUT | System Under Test — the thing being tested at larger scope |
| Types | Functional, browser/device, performance, deployment config, A/B diff, chaos engineering |
| Fidelity | Larger tests = higher fidelity but slower and less deterministic |

### Deprecation → [[Deprecation]]
| Concept | Key Point |
|---------|-----------|
| Advisory vs Compulsory | Advisory = gentle warnings; Compulsory = enforced deadline with removal |
| Design for deprecation | Plan removal from the start of system design |

## Part IV — Tools

### Version Control → [[Version Control]]
| Concept | Key Point |
|---------|-----------|
| One-Version Rule | For every dependency, there must be exactly one version in the codebase |
| Trunk-Based Development | No long-lived feature branches; merge to main frequently |
| Monorepo | Google uses a single repository for most code (advantages: visibility, consistency) |

### Code Search → [[Code Search]]
| Concept | Key Point |
|---------|-----------|
| Six questions | Where, what, how, why, who, when — all answered by code search |
| Zero setup | No checkout needed; search the entire codebase from a browser |

### Build Systems → [[Build Systems]]
| Concept | Key Point |
|---------|-----------|
| Task-based vs Artifact-based | Task-based (Make) vs artifact-based (Bazel) build systems |
| 1:1:1 Rule | One directory, one package, one build target |
| Distributed builds | Remote execution for scalability (Google's build system) |

### Critique → [[Critique Tool]]
| Concept | Key Point |
|---------|-----------|
| Flow stages | Create → Review → Approve → Commit → Post-commit |
| Integration | Analysis results, comments, notifications, change history all in one tool |

### Static Analysis → [[Static Analysis]]
| Concept | Key Point |
|---------|-----------|
| Developer happiness | If analysis tools annoy developers, they won't be used |
| Tricorder | Google's platform for pluggable static analysis checks |
| Fix-it suggestions | Automated fixes alongside warnings increase adoption |

### Dependency Management → [[Dependency Management]]
| Concept | Key Point |
|---------|-----------|
| Diamond dependency | A depends on B and C; B and C depend on different versions of D |
| SemVer limitations | Version numbers can't fully express compatibility; humans make mistakes |
| Live at Head | Always depend on latest version; push responsibility to providers |
| Minimum Version Selection | Prefer oldest compatible version (opposite of typical resolution) |

### Large-Scale Changes → [[Large-Scale Changes]]
| Concept | Key Point |
|---------|-----------|
| LSC | Changes that touch hundreds/thousands of files across the codebase |
| Barriers | Technical debt, merge conflicts, haunted graveyards, heterogeneity |
| Process | Authorization → creation → sharding → testing → submitting → cleanup |

### CI → [[Continuous Integration]]
| Concept | Key Point |
|---------|-----------|
| Hermetic testing | Tests isolated from external environment changes |
| Fast feedback | CI value is proportional to feedback speed |

### CD → [[Continuous Delivery]]
| Concept | Key Point |
|---------|-----------|
| Release trains | Regular, scheduled releases instead of ad-hoc deployment |
| Flag-guarding | Ship code behind feature flags; decouple deploy from release |
| Shifting left on quality | Catch issues earlier in the pipeline |

### Compute → [[Compute as a Service]]
| Concept | Key Point |
|---------|-----------|
| Containerization | Abstract away machine details; schedule workloads centrally |
| Borg → Kubernetes | Google's internal compute orchestration → open-source equivalent |
| Managed compute | Centralized infrastructure with multitenancy |

## Must-Know Concepts
| Concept | Definition | Reference |
|---------|-----------|-----------|
| Hyrum's Law | All observable behaviors will be depended on | → [[Software Engineering Fundamentals]] |
| Beyoncé Rule | If you liked it, put a CI test on it | → [[Software Engineering Fundamentals]] |
| DAMP not DRY | Tests prefer clarity over deduplication | → [[Unit Testing]] |
| Shifting Left | Earlier detection = cheaper fixes | → [[Software Engineering Fundamentals]] |
| One-Version Rule | Exactly one version of every dependency | → [[Version Control]] |
| Bus Factor | Team resilience to member loss | → [[Team Culture and Collaboration]] |
| GSM Framework | Goals → Signals → Metrics | → [[Measuring Productivity]] |
| Chesterton's Fence | Understand before removing | → [[Knowledge Sharing]] |
| Live at Head | Always use latest; providers ensure compatibility | → [[Dependency Management]] |

## Related Notes
- [[MOC - Software Engineering at Google]]
- [[testing_tutor/Software_Engineering_at_Google_StudyVault/00-Dashboard/Exam Traps]]

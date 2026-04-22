---
source_pdf: swe_at_google.2.pdf
part: all
keywords: MOC, study map, software engineering at google
---

# Software Engineering at Google Study Map

#dashboard #swe-at-google

## Overview
- **Source**: *Software Engineering at Google: Lessons Learned from Programming Over Time* (Titus Winters, Tom Manshreck, Hyrum Wright — O'Reilly 2020)
- **Scope**: 25 chapters across 5 parts covering Culture, Processes, and Tools
- **Core thesis**: Software engineering = programming integrated over time. Three pillars: **Time & Change**, **Scale & Growth**, **Trade-offs & Costs**

## Topic Map

### Part I — Thesis
| Section | Source | Notes | Status |
|---------|--------|-------|--------|
| What Is Software Engineering? | Ch 1 | [[Software Engineering Fundamentals]] | [ ] |

### Part II — Culture
| Section | Source | Notes | Status |
|---------|--------|-------|--------|
| How to Work Well on Teams | Ch 2 | [[Team Culture and Collaboration]] | [ ] |
| Knowledge Sharing | Ch 3 | [[Knowledge Sharing]] | [ ] |
| Engineering for Equity | Ch 4 | [[Equity in Engineering]] | [ ] |
| How to Lead a Team | Ch 5 | [[Leading Teams]] | [ ] |
| Leading at Scale | Ch 6 | [[Leading at Scale]] | [ ] |
| Measuring Engineering Productivity | Ch 7 | [[Measuring Productivity]] | [ ] |

### Part III — Processes
| Section | Source | Notes | Status |
|---------|--------|-------|--------|
| Style Guides and Rules | Ch 8 | [[Style Guides and Rules]] | [ ] |
| Code Review | Ch 9 | [[Code Review]] | [ ] |
| Documentation | Ch 10 | [[Documentation]] | [ ] |
| Testing Overview | Ch 11 | [[Testing Overview]] | [ ] |
| Unit Testing | Ch 12 | [[Unit Testing]] | [ ] |
| Test Doubles | Ch 13 | [[Test Doubles]] | [ ] |
| Larger Testing | Ch 14 | [[Larger Testing]] | [ ] |
| Deprecation | Ch 15 | [[Deprecation]] | [ ] |

### Part IV — Tools
| Section | Source | Notes | Status |
|---------|--------|-------|--------|
| Version Control and Branch Management | Ch 16 | [[Version Control]] | [ ] |
| Code Search | Ch 17 | [[Code Search]] | [ ] |
| Build Systems and Build Philosophy | Ch 18 | [[Build Systems]] | [ ] |
| Critique: Google's Code Review Tool | Ch 19 | [[Critique Tool]] | [ ] |
| Static Analysis | Ch 20 | [[Static Analysis]] | [ ] |
| Dependency Management | Ch 21 | [[Dependency Management]] | [ ] |
| Large-Scale Changes | Ch 22 | [[Large-Scale Changes]] | [ ] |
| Continuous Integration | Ch 23 | [[Continuous Integration]] | [ ] |
| Continuous Delivery | Ch 24 | [[Continuous Delivery]] | [ ] |
| Compute as a Service | Ch 25 | [[Compute as a Service]] | [ ] |

## Practice Notes
| Problem Set | Questions | Link |
|-------------|-----------|------|
| Software Engineering Fundamentals | 8+ | [[Software Engineering Fundamentals Practice]] |
| Team Culture | 8+ | [[Team Culture Practice]] |
| Knowledge Sharing | 8+ | [[Knowledge Sharing Practice]] |
| Equity in Engineering | 8+ | [[Equity in Engineering Practice]] |
| Leading Teams | 8+ | [[Leading Teams Practice]] |
| Leading at Scale | 8+ | [[Leading at Scale Practice]] |
| Measuring Productivity | 8+ | [[Measuring Productivity Practice]] |
| Style Guides and Rules | 8+ | [[Style Guides and Rules Practice]] |
| Code Review | 8+ | [[Code Review Practice]] |
| Documentation | 8+ | [[Documentation Practice]] |
| Testing Overview | 8+ | [[Testing Overview Practice]] |
| Unit Testing | 8+ | [[Unit Testing Practice]] |
| Test Doubles | 8+ | [[Test Doubles Practice]] |
| Larger Testing | 8+ | [[Larger Testing Practice]] |
| Deprecation | 8+ | [[Deprecation Practice]] |
| Version Control | 8+ | [[Version Control Practice]] |
| Code Search | 8+ | [[Code Search Practice]] |
| Build Systems | 8+ | [[Build Systems Practice]] |
| Critique Tool | 8+ | [[Critique Tool Practice]] |
| Static Analysis | 8+ | [[Static Analysis Practice]] |
| Dependency Management | 8+ | [[Dependency Management Practice]] |
| Large-Scale Changes | 8+ | [[Large-Scale Changes Practice]] |
| Continuous Integration | 8+ | [[Continuous Integration Practice]] |
| Continuous Delivery | 8+ | [[Continuous Delivery Practice]] |
| Compute as a Service | 8+ | [[Compute as a Service Practice]] |

## Study Tools
| Tool | Description | Link |
|------|-------------|------|
| Exam Traps | Common mistakes and confusing points | [[testing_tutor/Software_Engineering_at_Google_StudyVault/00-Dashboard/Exam Traps]] |
| Quick Reference | Full cheat sheet of key concepts | [[testing_tutor/Software_Engineering_at_Google_StudyVault/00-Dashboard/Quick Reference]] |

## Tag Index
| Tag | Related Topics | Level |
|-----|----------------|-------|
| `#swe-at-google` | All topics | Top-level |
| `#culture` | Teams, Knowledge, Equity, Leadership | Domain |
| `#processes` | Style, Review, Docs, Testing, Deprecation | Domain |
| `#tools` | VCS, Search, Build, Analysis, CI/CD | Domain |
| `#testing` | Unit, Doubles, Larger, Overview | Domain |
| `#leadership` | Leading Teams, Leading at Scale | Domain |
| `#hyrums-law` | Ch 1 — implicit dependencies break | Concept |
| `#beyonce-rule` | Ch 1, 11 — CI tests as contract | Concept |
| `#damp-not-dry` | Ch 12 — test clarity over DRY | Concept |
| `#one-version-rule` | Ch 16, 21 — single source of truth | Concept |
| `#shifting-left` | Ch 1, 20 — find problems earlier | Concept |
| `#trunk-based-dev` | Ch 16 — no long-lived branches | Concept |
| `#monorepo` | Ch 16 — single repository model | Concept |
| `#live-at-head` | Ch 21 — always depend on latest | Concept |
| `#semver` | Ch 21 — semantic versioning limits | Concept |

> **Tag rule**: Detail tags always co-attach their parent domain tag. E.g., `#unit-testing #testing`.

## Weak Areas
- [ ] Area needing review → [[Relevant Note]] → [[testing_tutor/Software_Engineering_at_Google_StudyVault/00-Dashboard/Exam Traps]]

## Non-core Topic Policy
| Source | Content | Handling |
|--------|---------|----------|
| swe_at_google.2.pdf | Foreword, Preface, Acknowledgments | **Excluded** — introductory/meta content, no testable material |
| swe_at_google.2.pdf | Afterword (Part V) | **Excluded** — summary/reflection, covered by chapter notes |

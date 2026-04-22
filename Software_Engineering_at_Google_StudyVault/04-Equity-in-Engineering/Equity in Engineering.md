---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: equity, diversity, inclusion, bias, multicultural-capacity, underrepresented-groups
---

# Equity in Engineering (Importance: ★★☆)

#equity #culture

*Reading time: ~5 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Core Thesis | Bias is the **default**; diversity is necessary for equitable products |
| Unconscious Bias | Insidious, unintentional -- harder to mitigate than intentional exclusion |
| Google Photos Case | Image recognition classified Black users as "gorillas" -- incomplete training data + lack of representation |
| Multicultural Capacity | Engineers must understand how products advantage/disadvantage different groups |
| Inclusive Design | Build **with** everyone, not just **for** everyone; design for the hardest use case first |
| Reject Singular Approaches | Fixing hiring pipeline alone is insufficient; must address retention and progression too |
| Values vs Outcomes | Good values do not equal good outcomes; implementation-level failures persist despite policies |
| Measure Equity | Don't assume equity; actively measure it throughout systems |

## Bias Is the Default
When engineers don't focus on users of different nationalities, ethnicities, races, genders, ages, abilities, and belief systems, even talented staff will **inadvertently fail** their users.
- Most people exhibit **unconscious bias** -- enforcing existing stereotypes without realizing it
- Unconscious bias is more difficult to mitigate than intentional exclusion
- Google's engineering population has been mostly male, mostly White or Asian -- not representative of all users
- Lack of representation means lacking the requisite **diversity to understand** product impact on vulnerable users

## Case Study: Google Photos (Racial Inclusion)
In 2015, Google Photos classified a Black software engineer's friends as "gorillas."
- **Root causes**: (1) Training data was incomplete and didn't represent the population; (2) Google/tech industry lacked Black representation, affecting design decisions; (3) Target market didn't adequately include underrepresented groups; (4) Tests didn't catch the error -- users did
- As late as 2018, the underlying problem was still not adequately addressed
- Similar failures: offensive autocomplete results, manipulable ad systems, hate speech on YouTube
- The technology itself isn't to blame -- it wasn't **resilient enough** in design to exclude discriminatory outputs

## Understanding the Need for Diversity
Being an exceptional engineer requires bringing **diverse perspectives** into product design and implementation.
- A CS degree alone does not make you an engineer -- broader understanding is needed
- Engineers should understand the **population demographics** of their users
- Focus on people who are **different** from yourself, especially those who might misuse your products
- Engineering teams need to be **representative** of their existing and future users
- In absence of diverse teams, individual engineers must learn to build for all users

## Building Multicultural Capacity
Exceptional engineers understand how products can **advantage and disadvantage** different groups.
- Engineers wield power to literally **change society** -- must exercise it without causing harm
- Recognize the default state of your bias caused by societal and educational factors
- AI/ML moves fast -- but we must pause to consider whether systems eliminate shared prosperity
- Facial recognition continues to **disadvantage people of color** due to incomplete research and narrow skin tone ranges
- Google now offers **statistical training within AI** to reduce intrinsic dataset bias
- Professional development must be **comprehensive and multidisciplinary**, not just technical

## Making Diversity Actionable
Systemic equity is attainable if we accept **accountability** for systemic discrimination.
- Don't defer to "hundreds of years of historical discrimination" as an excuse for inaction
- Focus on **quantifiable, actionable steps**: balanced candidate slates, equitable growth opportunities
- Every tech lead or engineering manager has the means to augment equity on their teams
- "We are all part of the system. It is our problem to fix."

## Reject Singular Approaches
No single solution fixes inequity -- problems are **complex and multifactorial**.
- Fixing the hiring pipeline alone is insufficient -- must also address **retention and progression**
- Attrition among Black+ Google employees outpaces all other groups
- "Build for the majority use case first" is flawed -- it gives already-advantaged users a head start, increasing inequity
- **Design for the user least like you** -- building for those with additional challenges makes the product better for everyone
- Do more comprehensive **user-experience research**: multilingual, multicultural, spanning countries, classes, abilities, ages

## Challenge Established Processes
Building equitable systems means questioning processes that drive **invalid results**.
- Case study: A hiring system proposed highlighting low performance ratings for internal transfers
- Investigation found: poor past ratings were **not predictive** of future performance -- candidates often thrived on new teams
- Performance ratings are indicative only of how a person performs in their **current role at the time**
- This analysis took significant project time but produced a **more equitable** internal mobility process

## Values Versus Outcomes
Google's core values support diversity, yet hiring a representative workforce remains elusive.
- The failure point is not in values or investments, but in **application at the implementation level**
- Five actionable steps:
  1. **Take a hard look in the mirror** -- acknowledge public failures
  2. **Build with everyone**, not just for everyone -- engage users across the spectrum of humanity
  3. **Design for the most difficult user** -- don't trade equity for short-term velocity
  4. **Measure equity** -- don't assume it; recognize decision-makers are also subject to bias
  5. **Change is possible** -- we need new approaches, not failed ones from the past

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Bias is the default" | Unconscious bias is **insidious** and unintentional |
| "Google Photos gorillas" | Incomplete **training data** + lack of **representation** |
| "Multicultural capacity" | Understand how products **advantage/disadvantage** groups |
| "Build with everyone" | Engage users across **spectrum of humanity**, not build in a vacuum |
| "Design for least like you" | Build for **hardest** use case first = better for all |
| "Reject singular approaches" | Hiring pipeline alone is **insufficient** |
| "Performance ratings" | **Not predictive** of future performance on new teams |
| "Values vs outcomes" | Good values do not equal good outcomes; failure at **implementation** level |
| "Measure equity" | Don't **assume** equity; actively measure throughout systems |

## Related Notes
- [[Software Engineering Fundamentals]]
- [[Team Culture and Collaboration]]
- [[Knowledge Sharing]]
- [[Leading Teams]]

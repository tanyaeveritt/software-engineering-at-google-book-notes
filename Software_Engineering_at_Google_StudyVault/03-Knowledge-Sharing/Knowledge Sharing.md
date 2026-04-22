---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: knowledge-sharing, psychological-safety, mentorship, documentation, readability
---

# Knowledge Sharing (Importance: ★★★)

#knowledge-sharing #culture

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Core Principle | Organization should be able to answer most of its own questions |
| Psychological Safety | Foundation — must feel safe to admit ignorance and ask questions |
| Challenges | Info islands, SPOF, all-or-nothing expertise, parroting, haunted graveyards |
| Mechanisms | Mentorship, group chats, mailing lists, YAQS, office hours, tech talks, docs |
| Tribal vs Written | Complement each other; neither alone is sufficient |
| Readability Process | Standardized mentorship via code review — 20% of Google engineers participate |
| Culture | Respect, incentives, recognition drive knowledge-sharing behavior |
| Canonical Sources | Centralized, org-wide info — style guides, developer guides, go/ links, codelabs |

## Challenges to Learning
Six challenges that arise without a strong learning culture:
- **Lack of psychological safety**: fear of punishment for mistakes or ignorance; culture of fear
- **Information islands**: knowledge fragmentation across groups that don't communicate — leads to fragmentation, duplication, and skew
- **Single point of failure (SPOF)**: critical info available from only one person; related to bus factor. "Let me take care of that for you" optimizes short-term at cost of long-term scalability
- **All-or-nothing expertise**: split between people who know "everything" and novices with no middle ground; experts accumulate all responsibilities
- **Parroting**: mimicry without understanding; mindlessly copying patterns or code without knowing their purpose
- **Haunted graveyards**: places in code people avoid touching out of **fear and superstition**, not understanding

## Psychological Safety
The **most important** part of an effective team (per Google's research).
- To learn, you must first acknowledge that there are things you don't understand
- Healthy environments: people feel comfortable asking questions, being wrong, and learning
- Leaders must **model** this behavior: "the more you know, the more you know you don't know"
- In large groups, psychological safety is amplified — group interactions must be **cooperative, not adversarial**

## Recurse Center Social Rules
Rules for maintaining psychological safety in groups:
- **No feigned surprise**: "What?! I can't believe you don't know what the stack is!" — barrier to safety
- **No "well-actuallys"**: pedantic corrections about grandstanding, not precision
- **No back-seat driving**: interrupting discussions to offer opinions without commitment
- **No subtle "-isms"**: "It's so easy my grandmother could do it!" — small expressions of bias

## Mentorship
Google assigns every **Noogler** (new Googler) a mentor who is NOT their team member, manager, or tech lead.
- Mentor is a volunteer with 1+ year tenure; a safety net for tricky questions
- Being on a **different team** makes the mentee more comfortable asking for help
- Mentorship formalizes learning, but learning itself is an ongoing process
- With healthy teams, members are open to both answering AND asking questions

## Knowledge-Sharing Mechanisms
**Scaling questions (community-based):**
- **Group chats**: quick Q&A; topic-driven (open, large) or team-oriented (smaller, safer); not well-structured for later reference
- **Mailing lists**: searchable archives, more structure; post answers back for future seekers; Google culture is "infamously email-heavy"
- **YAQS**: Google's internal Stack Overflow; helpful answers promoted; questions/answers editable to stay current

**Scaling knowledge (teaching):**
- **Office hours**: regularly scheduled; good for ambiguous or specialized problems; not first choice for urgent questions
- **Tech talks and classes**: g2g (Googler2Googler) program — thousands of Googlers teach topics from technical to fun; classes work best when topic is complicated, stable, benefits from teachers, and has enough demand
- **Documentation**: written knowledge for readers to learn; update docs when you learn something new ("leave the campground cleaner"); g3doc keeps docs next to source code
- **Code**: "code is knowledge" — code reviews are learning opportunities for both author and reviewer

## Canonical Sources of Information
Centralized, org-wide information that standardizes and propagates expert knowledge:
- **Developer guides**: style guides, best practices, code review guides, Tips of the Week (TotW)
- **go/ links**: Google's internal URL shortener; predictable, memorable (e.g., go/python); low-friction sharing; persistent permalinks even when underlying URL changes
- **Codelabs**: guided, hands-on tutorials combining explanations, best-practice code, and exercises; halfway between docs and instructor-led classes
- **Static analysis tools**: programmatically share best practices; when a check is added, every engineer using the tool becomes aware

## Readability Process
Google's standardized, **human-driven mentorship** through code review — covers language idioms, code structure, API design, documentation, test coverage.
- Started as **Craig Silverstein** (employee #3) doing line-by-line reviews with every new hire
- Today ~**20%** of Google engineers participate at any time (as reviewers or authors)
- ~**1-2%** of engineers are readability reviewers (volunteers, held to highest standards)
- Every CL requires **readability approval** — a certified reviewer must approve
- Engineers graduate when they consistently write clear, idiomatic, maintainable code
- Trade-off: increased short-term code-review latency for long-term quality, consistency, and expertise
- EPR studies show readability has a **net positive impact** on engineering velocity

## Incentives and Recognition
Knowledge sharing must be **rewarded at a systemic level**, not just encouraged with platitudes:
- **Software engineering ladder**: explicitly calls out wider influence at senior levels
- **Peer bonuses**: monetary award any Googler can give another for above-and-beyond work — employee-driven, grassroots effect
- **Kudos**: public acknowledgement of smaller contributions
- People react to **incentives over platitudes** — put money where your mouth is

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Psychological safety" | **Most important** part of an effective team |
| "Information islands" | Knowledge **fragmentation** across non-communicating groups |
| "SPOF" | Critical info from **one person** only; bad for scalability |
| "Haunted graveyards" | Avoid changing code out of **fear**, not understanding |
| "Parroting" | Copying patterns **without understanding** |
| "Readability process" | Standardized **mentorship** via code review; ~20% participate |
| "go/ links" | Internal URL shortener; **predictable, memorable** |
| "No feigned surprise" | Recurse Center rule for **psychological safety** |
| "Peer bonuses" | **Bottom-up** recognition for knowledge sharing |
| "Chesterson's fence" | Understand **why** something exists before removing it |

## Related Notes
- [[Software Engineering Fundamentals]]
- [[Team Culture and Collaboration]]
- [[Equity in Engineering]]
- [[Leading Teams]]

---
source_pdf: swe_at_google.2.pdf
part: Part II
keywords: practice, equity, diversity, bias, inclusion, multicultural-capacity
---

# Equity in Engineering Practice (9 questions)

#practice #equity #culture

## Related Concepts
- [[Equity in Engineering]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Bias is the default | Unconscious bias is **insidious** |
> | Google Photos | Incomplete **data** + lack of **representation** |
> | Design for least like you | **Hardest** use case first |
> | Reject singular approaches | Pipeline alone is **insufficient** |
> | Performance ratings | **Not predictive** of future performance |
> | Build with everyone | Not just **for** everyone |

---

## Question 1 - Bias as Default [recall]
> What does the chapter mean by "Bias is the default"?

> [!answer]- Show Answer
> The chapter argues that **unconscious bias** is the default state for all people. Social scientists have recognized that most people exhibit unconscious bias, enforcing and promulgating existing stereotypes without realizing it. Unconscious bias is **insidious** and more difficult to mitigate than intentional acts of exclusion. Even when we want to do the right thing, we might not recognize our own biases. Organizations must also recognize that such bias exists and work to address it in their workforces, product development, and user outreach.

---

## Question 2 - Google Photos Failure [recall]
> What happened with Google Photos in 2015, and what were the root causes?

> [!answer]- Show Answer
> In 2015, Google Photos' image recognition algorithms classified Black software engineer Jacky Alcine's friends as "gorillas." Root causes:
> 1. **Incomplete training data** -- the photo dataset did not represent the full population
> 2. **Lack of Black representation** at Google and in tech, affecting algorithm design decisions and dataset collection
> 3. **Target market** did not adequately include underrepresented groups
> 4. **Testing gaps** -- Google's tests didn't catch the error; users discovered it
> As late as 2018, Google still had not adequately addressed the underlying problem.

---

## Question 3 - Five Actionable Steps [recall]
> List the five steps the chapter recommends for building more equitable products.

> [!answer]- Show Answer
> 1. **Take a hard look in the mirror** -- acknowledge public failures honestly
> 2. **Build with everyone, not just for everyone** -- engage users across the spectrum of humanity; put the most vulnerable communities at the center of design
> 3. **Design for the user who will have the most difficulty** using your product -- don't trade equity for short-term velocity
> 4. **Don't assume equity; measure it** throughout your systems -- recognize that decision-makers are also subject to bias
> 5. **Change is possible** -- we can't solve today's problems with failed approaches of the past; we need new skills and approaches

---

## Question 4 - Performance Ratings Case [recall]
> What did Google discover when they investigated using past performance ratings in internal mobility?

> [!answer]- Show Answer
> A hiring system proposed highlighting low performance ratings to managers when employees applied for internal transfers. Investigation revealed that candidates who had received a **poor performance rating were just as likely** to receive satisfactory or exemplary ratings on a new team as candidates who never received a poor rating. Performance ratings are indicative only of how a person performs in their **given role at the time** they are being evaluated -- they are **not predictive** of future performance on a different team.

---

## Question 5 - Reject Singular Approaches [recall]
> Why does the chapter say fixing the hiring pipeline alone is insufficient for improving diversity?

> [!answer]- Show Answer
> The chapter argues that problems are **complex and multifactorial**, so no single approach works. Specifically:
> - Must also address **retention and progression**, not just hiring
> - Attrition among Black+ Google employees **outpaces** attrition from all other groups, undermining hiring progress
> - Need to evaluate whether the ecosystem allows all aspiring engineers to **thrive**, not just enter
> - Must focus on psychological safety, inclusive culture, equitable opportunities for growth, and removing bias from evaluation processes
> Fixing only the pipeline while ignoring retention creates a "leaky bucket" problem.

---

## Question 6 - Majority Use Case Fallacy [recall]
> What is wrong with the approach of "build for the majority use case first"?

> [!answer]- Show Answer
> Building for the majority use case first gives users who are **already advantaged** in access to technology a head start, which **increases inequity**. Relegating consideration of underrepresented user groups to late-stage design lowers the bar of what it means to be an excellent engineer. Instead, the chapter recommends building in **inclusive design from the start** -- designing for the user who is least like you. Building for those with additional challenges makes the product **better for everyone**.

---

## Question 7 - AI Facial Recognition [application]
> Your team is developing a facial recognition feature. Based on the chapter's lessons, what precautions should you take?

> [!answer]- Show Answer
> Based on Chapter 4's lessons:
> 1. Ensure training data includes a **wide range of skin tones** and ethnic diversity -- incomplete data produces invalid results
> 2. Verify that both the training data and the team creating the software are **representative** of the full population
> 3. Be willing to **delay development** in favor of getting more complete and accurate data
> 4. Use Google's **statistical training within AI** context to identify and reduce intrinsic dataset bias
> 5. Test with **diverse user groups** spanning multiple nationalities, ethnicities, and demographics
> 6. Consider potential **misuse** -- how could governments or others use this to surveil or harm underrepresented communities?
> The chapter warns: "We cannot expect the output to be valid if both the training data and those creating the software represent only a small subsection of people."

---

## Question 8 - Equity in Hiring Systems [application]
> You're building an internal job application system. Recruiters want to display candidates' most recent performance ratings to hiring managers to "save time." What equity concerns should you raise?

> [!answer]- Show Answer
> Drawing from the chapter's Google case study, raise three equity questions:
> 1. **Are performance assessments predictive** of future performance? (Google found they are NOT -- people thrive on new teams regardless of past ratings)
> 2. **Are ratings free of individual bias?** (Managers' evaluations can reflect unconscious bias)
> 3. **Are ratings standardized** across organizations? (Different teams may rate differently)
> If the answer to any is "no," presenting ratings could drive **inequitable and invalid results**. The chapter shows this analysis took project time but produced a more equitable internal mobility process. Recommend conducting a thorough review before implementing.

---

## Question 9 - Values vs Implementation Gap [analysis]
> Google has strong diversity values but repeatedly misses its representation goals. Analyze why this gap exists and what the chapter proposes as a solution.

> [!answer]- Show Answer
> The chapter identifies the gap as an **implementation-level failure**, not a values failure. Google's core values are based on respect, diversity, and inclusion, and the company invests heavily in programs. Yet outcomes lag because:
> - **Old habits are hard to break** -- designers default to the users they're accustomed to, who may not be representative
> - **Individual performance culture** can conflict with accountability for product equity across all areas
> - Companies historically choose **speed and shareholder value** over slowing down for equity
> - The people making decisions may be **undereducated about causes of inequity**
>
> The chapter's solution is systemic: don't just state values -- **measure equity** throughout systems, hold individuals accountable for actionable steps (balanced candidate slates, equitable growth opportunities), reject singular approaches, and invest in **continuous multidisciplinary professional development** beyond just technical skills. "We are all part of the system. It is our problem to fix."

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Bias is the default | Unconscious, **insidious**, harder than intentional |
> | Google Photos | Incomplete **data** + lack of **diversity** |
> | Build with everyone | Not just **for** -- engage across spectrum |
> | Design for hardest user | Better for **everyone** |
> | Reject singular approaches | Pipeline + retention + **progression** |
> | Performance ratings | Not **predictive** of future performance |
> | Values vs outcomes | Failure at **implementation** level |
> | Multicultural capacity | **Multidisciplinary** professional development |
> | Measure equity | Don't **assume** -- actively verify |

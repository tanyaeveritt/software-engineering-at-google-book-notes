---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, unit-testing, maintainability, DAMP, behaviors, public-api
---

# Unit Testing Practice (10 questions)

#practice #unit-testing #testing

## Related Concepts
- [[Unit Testing]]
- [[Testing Overview]]
- [[Test Doubles]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Unchanging test | Never changes unless **requirements** change |
> | Test via public APIs | Test the way **users** use the system |
> | Behaviors not methods | given/when/then; **many-to-many** mapping |
> | DAMP | **Descriptive And Meaningful Phrases** |
> | No logic in tests | Straight-line code only |

---

## Question 1 - Unchanging Tests [recall]
> What are the four kinds of production code changes, and for which kind should existing tests need to change?

> [!answer]- Show Answer
> 1. **Pure refactoring**: tests should NOT change (if they do, tests are at the wrong abstraction level)
> 2. **New features**: existing tests should NOT change; write new tests for new behaviors
> 3. **Bug fixes**: existing tests should NOT change; add the missing test case
> 4. **Behavior changes**: the ONE case where existing tests SHOULD change. This is the most expensive type because users rely on current behavior.
> The ideal test is **unchanging**—after writing it, you never touch it again unless the system's requirements change.

---

## Question 2 - Public API Testing [recall]
> Why does Google recommend testing via public APIs rather than testing private methods directly?

> [!answer]- Show Answer
> Testing via public APIs ensures tests call the system the **same way its users would**. This provides two key benefits:
> 1. Tests form **explicit contracts**—if a test breaks, it means a real user would also be broken
> 2. Tests are resilient to **internal refactoring**—renaming methods, extracting helpers, or changing serialization formats won't break tests
> Testing private methods couples tests to implementation details, making them **brittle**: almost any internal change breaks the test even if behavior is unchanged.

---

## Question 3 - State vs. Interaction Testing [recall]
> What is the difference between state testing and interaction testing, and which does Google prefer?

> [!answer]- Show Answer
> **State testing**: call the system under test and verify the correct value was returned or some state was properly changed (e.g., `assertThat(accounts.getUser("foobar")).isNotNull()`).
> **Interaction testing**: verify that specific functions were called with specific arguments (e.g., `verify(database).put("foobar")`).
> Google **prefers state testing** because interaction tests are more brittle—they check HOW the system arrived at its result rather than WHAT the result is. Interaction tests can pass when bugs exist (e.g., record deleted after write) and fail on harmless refactors (e.g., calling a different but equivalent API).

---

## Question 4 - Test Behaviors, Not Methods [recall]
> What does it mean to "test behaviors, not methods"? How do you express a behavior?

> [!answer]- Show Answer
> Instead of writing one test per production method, write one test per **behavior**—a guarantee about how the system responds to inputs in a given state. Behaviors are expressed using **given/when/then**: "Given a bank account is empty, when attempting to withdraw money, then the transaction is rejected."
> The mapping between methods and behaviors is **many-to-many**: a method implements multiple behaviors, and some behaviors span multiple methods. Behavior-driven tests are clearer, more focused, and easier to maintain. If you need "and" in a test name, you're likely testing multiple behaviors.

---

## Question 5 - No Logic in Tests [application]
> Consider this test assertion: `assertThat(nav.getCurrentUrl()).isEqualTo(baseUrl + "/albums")`. What problem does Google identify with this, and how should it be fixed?

> [!answer]- Show Answer
> The string concatenation introduces **logic** into the test. Even this simple operation can hide bugs—for example, if `baseUrl` is `"http://photos.google.com/"`, the result would be `"http://photos.google.com//albums"` (double slash). The fix is to **hardcode the full expected value**: `assertThat(nav.getCurrentUrl()).isEqualTo("http://photos.google.com/albums")`. Clear tests are **trivially correct upon inspection**—stick to straight-line code and tolerate duplication when it makes the test more descriptive.

---

## Question 6 - DAMP vs DRY [application]
> A test suite has helper methods like `createUsers(boolean... banned)`, `createForumAndRegisterUsers(List<User>)`, and `validateForumAndUsers(Forum, List<User>)`. Tests are concise but their bodies hide all important details. Is this good practice? Why or why not?

> [!answer]- Show Answer
> No. This is an example of being **too DRY** in test code. While the test bodies are concise, they are **not complete**: important details are hidden in helper methods that require scrolling to a different part of the file. The helpers contain logic (loops, conditionals) that makes them hard to verify at a glance.
> The fix: apply **DAMP** (Descriptive And Meaningful Phrases). Rewrite each test to be self-contained—inline the relevant setup, assertions, and values. The resulting tests have more duplication but each is **far more meaningful** and understandable entirely without leaving the test body. DAMP complements DRY; helpers are fine for irrelevant setup but should not hide important behavior details.

---

## Question 7 - Shared Values [application]
> A test file defines `ACCOUNT_1` and `ACCOUNT_2` as shared constants used by hundreds of tests. What problem does this create, and what's a better approach?

> [!answer]- Show Answer
> Shared constants with **ambiguous names** make it difficult to understand why a particular value was chosen for each test. Engineers must scroll to the definitions to verify that `ACCOUNT_1` is appropriate. Even descriptive names like `CLOSED_ACCOUNT` make it harder to see exact details and encourage reuse even when the name doesn't exactly fit.
> **Better approach**: use **helper methods with defaults** that let each test specify only the values it cares about: `newAccount().setState(CLOSED).setBalance(0).build()`. This makes every test self-documenting while keeping irrelevant details hidden. In languages without named parameters, use the **Builder pattern**.

---

## Question 8 - Clear Failure Messages [recall]
> What makes a good test failure message? Give an example of a bad and a good one.

> [!answer]- Show Answer
> A good failure message should let an engineer diagnose the problem **without reading the test code**. It should clearly express the desired outcome, actual outcome, and relevant parameters.
> - **Bad**: `"Test failed: account is closed"` (ambiguous—did it fail because the account was closed, or was it expected to be closed?)
> - **Good**: `"Expected an account in state CLOSED, but got account: {name: 'my-account', state: 'OPEN'}"`
> Google recommends assertion libraries like **Truth** that automatically produce informative messages (e.g., `assertThat(colors).contains("orange")` → `"<[red, green, blue]> should have contained <orange>"`).

---

## Question 9 - Mary's Dilemma [analysis]
> Mary adds a small feature (a few dozen lines) but gets a screen full of test failures—none related to actual bugs, all due to broken assumptions about internal code structure. Using the chapter's concepts, diagnose what went wrong and prescribe fixes.

> [!answer]- Show Answer
> Mary's team has two problems:
> 1. **Brittle tests**: tests are coupled to implementation details (private methods, internal state, interaction patterns) rather than testing via **public APIs**. Any refactoring breaks them.
> 2. **Unclear tests**: when failures occur, it's difficult to determine what the test was checking or how to fix it—tests lack **completeness** and clear failure messages.
> Prescriptions:
> - Rewrite tests to test via **public APIs** only—same way users interact with the system
> - Prefer **state testing** over interaction testing
> - Structure tests with explicit **given/when/then** and name them after the behavior being tested
> - Ensure each test is **complete and concise**—contains all relevant info without distracting details
> - Apply **DAMP** for code sharing; don't hide important details in helpers

---

## Question 10 - Test Infrastructure [analysis]
> Google mandated Mockito as the only mocking framework for new Java tests and banned other frameworks. Why would standardizing test infrastructure be important, even if it upsets engineers comfortable with other frameworks?

> [!answer]- Show Answer
> Test infrastructure (shared across multiple test suites) is more like **production code** than test code—it has many callers and is difficult to change without breakages. Standardization provides:
> 1. **Reduced cognitive load**: engineers moving between teams encounter the same patterns
> 2. **Efficient investment**: efforts to improve one framework benefit everyone
> 3. **Lower maintenance**: fewer frameworks to support means higher quality for each
> 4. **Easier comprehension**: tests across the codebase are readable by any engineer
> The initial "grumbling" from engineers was outweighed by the long-term benefit. Custom test infrastructure must be treated as its own separate product with its own tests. Reducing the number of frameworks **increases the efficiency** of investment in improving them.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Unchanging test | Only changes for **behavior changes** |
> | Test via public APIs | Same way **users** use the system |
> | State vs. interaction | Prefer **state**—verify results, not calls |
> | Behaviors not methods | given/when/then; many-to-many mapping |
> | DAMP | Descriptive And Meaningful Phrases > DRY |
> | No logic in tests | Straight-line code; trivially correct on inspection |
> | Naming | `should[Behavior]When[State]` |
> | Shared values | **Helper methods with defaults** > constants |
> | Failure messages | Expected vs. actual with context |
> | Test infrastructure | Standardize early; treat as production code |

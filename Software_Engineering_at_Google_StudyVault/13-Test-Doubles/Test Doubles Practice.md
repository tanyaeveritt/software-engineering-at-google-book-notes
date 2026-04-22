---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: practice, test-doubles, faking, stubbing, mocking, interaction-testing
---

# Test Doubles Practice (10 questions)

#practice #test-doubles #testing

## Related Concepts
- [[Test Doubles]]
- [[Unit Testing]]
- [[Testing Overview]]
- [[Larger Testing]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Google's first choice | **Real implementations** |
> | Fake vs. Stub | Fake has **behavior + state**; stub returns hardcoded values |
> | @DoNotMock | Use **fakes or real impl** instead |
> | Overuse of stubs → | Unclear, brittle, less effective tests |
> | Interaction testing risk | **Change-detector tests** |
> | State > Interaction | Validate results, not function calls |

---

## Question 1 - Three Techniques [recall]
> Name the three primary techniques for using test doubles and briefly describe each.

> [!answer]- Show Answer
> 1. **Faking**: a lightweight implementation of an API that behaves similarly to the real one (e.g., in-memory database). Maintains state, executes quickly, and provides high fidelity.
> 2. **Stubbing**: hardcoding return values for functions using a mocking framework (e.g., `when(mock.getUser()).thenReturn(user)`). Quick to apply but has no state and limited fidelity.
> 3. **Interaction testing**: validating how a function is called without executing it (e.g., `verify(mock).sendEmail(...)`). Confirms calls were made but cannot verify the system actually works.

---

## Question 2 - Real Implementations First [recall]
> Why does Google prefer real implementations over test doubles as their first choice?

> [!answer]- Show Answer
> Tests have higher **fidelity** when they execute real production code paths. Google found that overuse of mocking frameworks led to tests requiring **constant maintenance** while **rarely finding bugs**—the mocks got out of sync with real implementations and made refactoring difficult. Real implementations also avoid the risk of duplicating behavior incorrectly. The preference for real implementations is called **classical testing** (vs. mockist testing). Google found mockist testing difficult to scale.

---

## Question 3 - @DoNotMock [recall]
> What is the @DoNotMock annotation and why would an API owner use it?

> [!answer]- Show Answer
> **@DoNotMock** is a Java annotation (part of ErrorProne) that declares "this type should not be mocked because better alternatives exist." When an engineer tries to mock an annotated class, they see an error directing them to use a real implementation or a fake.
> API owners use it because mocking **severely constrains their ability to make changes**: mocked types may be mocked thousands of times across the codebase, and those mocks likely **violate the API contract** (e.g., returning null when the real impl never would). If tests used real implementations or fakes, the API owner could evolve their implementation freely.

---

## Question 4 - Fake Fidelity [recall]
> What does "fidelity" mean for fakes, and what level of fidelity is required?

> [!answer]- Show Answer
> **Fidelity** is how closely a fake's behavior matches the real implementation. A fake must have fidelity to the **API contracts** of the real implementation: for any given input, it should return the same output and perform the same state changes. However, **perfect fidelity** is not always needed—a fake database doesn't need to simulate disk storage, and a fake hash function doesn't need to produce identical hash values, only unique ones per input. Fidelity to latency and resource consumption is typically unnecessary. If a code path isn't supported, the fake should **fail fast** (raise an error).

---

## Question 5 - Stubbing Dangers [recall]
> What are the three ways that overusing stubbing harms tests?

> [!answer]- Show Answer
> 1. **Tests become unclear**: extra stubbing code detracts from the test's intent, requiring readers to mentally step through the SUT to understand why functions are stubbed.
> 2. **Tests become brittle**: stubs leak implementation details—when production code changes, tests break even if behavior is unchanged.
> 3. **Tests become less effective**: no way to ensure stubbed behavior matches the real implementation. Stubs cannot store state (e.g., can't save an item and retrieve it later). You're forced to duplicate contract details with no guarantee of correctness.

---

## Question 6 - Choosing a Test Double [application]
> You need to test a class that calls a database. The real database is too slow for unit tests and no fake exists. Should you use stubbing or interaction testing? What should you ask the database team?

> [!answer]- Show Answer
> **First**, ask the database team to create a **fake**. If they're unfamiliar with fakes, explain the benefits: a single fake can radically improve the testing experience for all API consumers. If they refuse, you could write your own by wrapping all database calls in a single class and creating a fake of that wrapper.
> If no fake is feasible, **stubbing** is appropriate when you need specific return values to get the SUT into a certain state. **Interaction testing** is a fallback when you can't perform state testing at all—use it only for **state-changing** functions. In either case, **supplement** with larger-scope integration tests that use the real database to ensure correctness.

---

## Question 7 - State-Changing vs. Non-State-Changing [application]
> A test verifies that `getPermission()` was called on a mock AND that `addPermission()` was called. Which verification is appropriate and which is problematic? Why?

> [!answer]- Show Answer
> - **`addPermission()`** verification is appropriate: it's a **state-changing** function with side effects (modifying the permission database). Interaction testing is reasonable here because the call itself is the important behavior.
> - **`getPermission()`** verification is problematic: it's a **non-state-changing** function that just reads data. Verifying it is redundant because the SUT will use the return value for other work you can assert on directly. It also makes the test **brittle**—you'll need to update it anytime the interaction pattern changes, and it adds noise that makes it harder to identify what the test actually cares about.

---

## Question 8 - Overspecified Interaction Test [application]
> A test calls `verify(userPrompt).setText("Fake User", "Good morning!", "Version 2.1")` with exact arguments. Why is this problematic, and how should it be fixed?

> [!answer]- Show Answer
> This is **overspecified**: the test validates ALL arguments even though the test's intent is only to verify the user's name appears. If any unrelated argument changes (greeting text, version string), this test breaks even though the behavior under test (displaying the user name) is unchanged.
> **Fix**: use `any()` matchers for irrelevant arguments: `verify(userPrompt).setText(eq("Fake User"), any(), any())`. Split different behaviors into **separate tests**, each verifying the minimum necessary. This follows the principle of testing **behaviors, not methods**: each test validates one specific behavior and ignores everything else.

---

## Question 9 - Classical vs. Mockist [analysis]
> Google tried mockist testing extensively and then shifted back to classical testing. What lessons did they learn, and how does @DoNotMock embody that learning?

> [!answer]- Show Answer
> Google learned that mockist testing (defaulting to mocking frameworks over real implementations) was **difficult to scale**. Tests required **constant effort to maintain** while rarely finding bugs because:
> - Mocks got out of sync with real implementations as APIs evolved
> - Mocked behavior often violated API contracts (returning impossible states)
> - Refactoring became extremely difficult (every mock duplicated implementation knowledge)
> - Tests were "change detectors" failing on harmless changes
> **@DoNotMock** embodies this learning by giving API owners the power to **proactively prevent** misuse. It directs engineers to use better alternatives (fakes, real impls) and prevents the accumulation of thousands of mocks that would constrain API evolution. It's a systematic, tooling-based approach to enforcing the preference for realism over isolation.

---

## Question 10 - Trade-off Decision [analysis]
> Your team is deciding between using (a) a real implementation that adds 100ms per test case with 500 test cases, or (b) a mock that runs instantly. You have no fake. What factors should guide your decision, and what would Google likely recommend?

> [!answer]- Show Answer
> Factors to consider:
> - **Execution time**: 100ms × 500 = 50 seconds total. While not trivial, Google's advice is to use real implementations until they become "too slow"—this threshold depends on whether engineers feel a loss in productivity.
> - **Test parallelization**: Google's infrastructure can split tests across servers, trading CPU for developer time.
> - **Build time**: real implementations require building all dependencies.
> - **Fidelity**: the real implementation gives much higher confidence that code works correctly.
> - **Maintenance cost**: mocks require constant maintenance as the implementation evolves.
> Google would likely recommend **using the real implementation** for now (50 seconds is manageable with parallelization) and investing in creating a **fake** if the test count grows or speed becomes a bottleneck. Mocking should be a last resort, not a default choice.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Google's preference order | **Real impl → Fake → Stub → Interaction test** |
> | Fake requirement | Fidelity to **API contract** |
> | Who writes fakes | Team that **owns the real implementation** |
> | Contract tests | Run against **both real and fake** |
> | Stubbing appropriate when | Need specific return value for **particular state** |
> | Interaction testing: which functions | Only **state-changing** (sendEmail, saveRecord) |
> | @DoNotMock purpose | Prevent mocking; direct to **better alternatives** |
> | Change-detector tests | Over-reliance on **interaction testing** |
> | Classical vs. mockist | Google prefers **classical** (real implementations) |
> | When to use real impl | Fast, deterministic, simple dependencies |

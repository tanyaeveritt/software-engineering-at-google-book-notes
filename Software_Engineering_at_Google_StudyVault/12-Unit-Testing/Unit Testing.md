---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: unit-testing, maintainability, brittle-tests, public-api, behaviors, DAMP, clarity
---

# Unit Testing (Importance: ★★★)

#unit-testing #testing #processes #damp-not-dry

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Unit Test Ratio | ~80% of tests at Google are unit tests |
| Core Goal | Maintainability—tests should "just work" after writing |
| Brittle Tests | Tests that break on unrelated changes; major productivity drain |
| Unchanging Tests | Ideal test never changes unless requirements change |
| Test via Public APIs | Don't test private methods—test the way users use the system |
| Test State, Not Interactions | Prefer state testing over interaction testing (verify results, not calls) |
| Test Behaviors, Not Methods | Write a test per behavior, not per method; use given/when/then |
| DAMP over DRY | Descriptive And Meaningful Phrases > Don't Repeat Yourself in tests |
| No Logic in Tests | Avoid operators, loops, conditionals in test code |
| Clear Failure Messages | Messages should identify expected vs. actual state with context |

## The Importance of Maintainability
The most important property of unit tests beyond catching bugs is **improving productivity**.
- Brittle tests that break on unrelated changes drain productivity—hours fixing tests instead of building features
- Unclear tests where the purpose is impossible to determine are equally harmful
- At Google's scale, an engineer may run **thousands of tests daily**; even small percentages of spurious failures waste huge amounts of time

## Strive for Unchanging Tests
After writing a test, you shouldn't need to touch it again unless **requirements change**. Four types of code changes and expected test impact:
- **Pure refactoring**: tests should NOT change (if they do, tests are at wrong abstraction level)
- **New features**: write new tests; existing tests should NOT change
- **Bug fixes**: add the missing test case; existing tests should NOT change
- **Behavior changes**: the ONE case where existing tests SHOULD change (most expensive type)

## Test via Public APIs
The most important practice for avoiding brittle tests. Call the system the **same way its users would**.
- Don't remove `private` visibility to test internal methods—this couples tests to implementation
- Tests against public APIs form **explicit contracts**: if the test breaks, a real user would also break
- Defining "public API" is more art than science: helper classes supporting 1-2 other classes should be tested through those classes, not directly
- **Rule**: if a class is accessible by anyone without consulting its owners, test it directly

## Test State, Not Interactions
Two ways to verify behavior: **state testing** (observe the system's result) vs. **interaction testing** (verify specific function calls were made).
- Interaction tests are more **brittle**: they check HOW the system arrived at its result rather than WHAT the result is
- Over-reliance on mocking frameworks leads directly to brittle interaction tests
- Prefer **real objects** over mocked objects when the real objects are fast and deterministic

## Test Behaviors, Not Methods
Write a test for each **behavior**, not each method. A behavior is a guarantee about how the system responds to inputs in a given state.
- Express behaviors as: **"Given** [state], **when** [action], **then** [result]"
- The mapping between methods and behaviors is **many-to-many**
- Behavior-driven tests read more like natural language and more clearly express cause and effect
- If you need the word "and" in a test name, you're probably testing **multiple behaviors**

## Structure Tests to Emphasize Behaviors
Every test should have explicit **given/when/then** (or arrange/act/assert) structure.
- Use whitespace or comments to make sections visually clear
- Avoid interspersing assertions among multiple calls (mixing "when" and "then")
- Each test should cover only a **single behavior**; most unit tests need only one "when" and one "then"

## Name Tests After Behaviors
Test names should summarize the **behavior being tested**, including actions taken and expected outcome.
- Good pattern: start with "should" → `BankAccount.shouldNotAllowWithdrawalsWhenBalanceIsEmpty`
- Reading all test names in a suite should give a good sense of the system's behaviors
- If the name needs "and," split into multiple tests

## Don't Put Logic in Tests
Clear tests are **trivially correct upon inspection**. Logic (operators, loops, conditionals) introduces complexity.
- Even simple string concatenation can conceal bugs (e.g., double-slash URL bug)
- Stick to **straight-line code** in tests; tolerate duplication when it makes tests more descriptive
- If you feel you need a test for your test, something has gone wrong

## Write Clear Failure Messages
A good failure message should let an engineer diagnose the problem **without reading the test code**.
- Bad: `"Test failed: account is closed"` (ambiguous)
- Good: `"Expected an account in state CLOSED, but got account: {name: 'my-account', state: 'OPEN'}"` 
- Use assertion libraries like **Truth** (Google) that produce informative messages automatically

## DAMP, Not DRY
In test code, prefer **Descriptive And Meaningful Phrases** over Don't Repeat Yourself.
- Production code benefits from DRY; test code benefits from **completeness and clarity**
- Each test should be understandable **entirely without leaving the test body**
- DAMP complements DRY: helpers are fine for irrelevant setup, but don't hide important details
- **Shared values**: use helper methods with defaults instead of ambiguously named constants
- **Shared setup**: fine for constructing objects, but tests that depend on specific setup values should override them explicitly
- **Test infrastructure**: treat it as its own product with its own tests; standardize frameworks early (Google mandated Mockito for Java)

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| "Unchanging test" | Test that never needs to change unless **requirements** change |
| Test via public APIs | Call the system the **same way its users would** |
| Test state vs. interactions | Prefer **state testing**—verify results, not function calls |
| Test behaviors, not methods | One test per **behavior** (given/when/then), not per method |
| DAMP | **Descriptive And Meaningful Phrases**—clarity over DRY in tests |
| No logic in tests | No loops, conditionals, operators—**straight-line code** only |
| Naming pattern | `shouldNotAllowWithdrawalsWhenBalanceIsEmpty` |
| Brittle test cause | Testing **implementation details** (private methods, interactions) |
| Shared values pitfall | Ambiguous constants like `ACCOUNT_1`; use **helper methods** instead |
| Failure message quality | Must distinguish **expected vs. actual** state with context |

## Related Notes
- [[Testing Overview]]
- [[Test Doubles]]
- [[Larger Testing]]
- [[Code Review]]
- [[Software Engineering Fundamentals]]

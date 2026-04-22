---
source_pdf: swe_at_google.2.pdf
part: Part III
keywords: test-doubles, faking, stubbing, mocking, interaction-testing, real-implementations, fidelity
---

# Test Doubles (Importance: ★★★)

#test-doubles #testing #processes #faking #stubbing #mocking

*Reading time: ~5 min*

## Overview Table (At a Glance)
| Item | Key Point |
|------|-----------|
| Test Double | Object/function standing in for a real implementation in tests |
| Fake | Lightweight implementation that behaves like the real thing (e.g., in-memory DB) |
| Stub | Hardcoded return values specified inline via mocking framework |
| Interaction Testing | Verifying how a function is called without executing it (verify()) |
| Real Implementation | Google's **first choice**; highest fidelity |
| Classical vs. Mockist | Google prefers **classical testing** (real implementations over mocks) |
| @DoNotMock | Annotation telling engineers not to mock a type; use fakes or real impl instead |
| Fidelity | How closely a test double's behavior matches the real implementation |
| Seams | Design points that allow swapping dependencies (e.g., dependency injection) |
| State > Interaction | Prefer state testing; interaction testing leads to **change-detector tests** |

## The Impact of Test Doubles
Test doubles introduce trade-offs that must be managed carefully:
- **Testability**: code must be designed with seams (e.g., dependency injection) to swap in doubles
- **Applicability**: improper use leads to brittle, complex, less effective tests—magnified at scale
- **Fidelity**: how closely the double resembles the real implementation; perfect fidelity is often not feasible but must match the **API contract**

## Seams and Dependency Injection
A **seam** makes code testable by allowing different dependencies in tests vs. production.
- **Dependency injection**: dependencies are passed to a class rather than instantiated directly
- Frameworks like **Guice** and **Dagger** (Java) automate dependency injection
- Dynamic languages (Python, JS) can replace individual functions directly, reducing the need for DI
- Writing testable code requires **upfront investment**; retrofitting is much harder

## Three Techniques

### Faking
A **fake** is a lightweight implementation of an API that behaves similarly to the real one but isn't suitable for production.
- Example: in-memory database, `FakeFileSystem` storing files in a `HashMap`
- Fakes execute quickly and **maintain state** (unlike stubs)
- Must maintain **fidelity to API contracts**: same output and state changes for given input
- Should be written and maintained by the **team that owns the real implementation**
- Fakes need their own tests (**contract tests**) run against both fake and real implementation
- If no fake exists: ask the API owners to create one, or wrap the API in a single class and fake that

### Stubbing
**Stubbing** hardcodes return values for functions using a mocking framework (e.g., `when(...).thenReturn(...)`).
- Quick and easy to apply, but has significant limitations
- **Tests become unclear**: extra stubbing code detracts from test intent
- **Tests become brittle**: stubs leak implementation details into tests
- **Tests become less effective**: no way to ensure stubbed behavior matches the real implementation; cannot store state
- Appropriate **only** when you need a specific return value to set up a particular state; keep the number of stubs small

### Interaction Testing
Validates **how a function is called** without executing it (e.g., `verify(mock).sendEmail(...)`).
- Sometimes called "mocking" (confusingly, since mocking frameworks do stubbing too)
- Leads to **change-detector tests**: fails on any implementation change, even if behavior is unchanged
- Cannot tell you the system **actually works**—only that expected functions were called
- **Prefer state testing** over interaction testing at Google

## Prefer Real Implementations (Classical Testing)
Google's first choice is always **real implementations**, not test doubles.
- Tests have higher **fidelity** when executing production code paths
- Overuse of mocking frameworks produced tests requiring constant maintenance that rarely found bugs
- **@DoNotMock** annotation (via ErrorProne): API owners declare that a type should not be mocked; directs engineers to use fakes or real implementations
- If mocked thousands of times, these mocks likely violate API contracts (e.g., returning null when the real impl never would)

## When to Use Real vs. Double
A real implementation is preferred when it is **fast, deterministic, and has simple dependencies**.
- **Execution time**: use a double if the real implementation is too slow; use real until it becomes too slow
- **Determinism**: nondeterministic code (multithreading, external services, system clock) may need a double; use hermetic instances when possible
- **Dependency construction**: complex dependency trees can tempt engineers to mock, but use factory methods or DI instead

## Fakes: Deep Dive
- **Fidelity**: must match the real implementation's API contract (same output for same input); perfect fidelity not needed for latency, resource consumption, etc.
- **When to write**: when productivity gains outweigh creation/maintenance costs; create at the **root** of untestable code, not for each consumer
- **Testing fakes**: use **contract tests** that run against both real and fake implementations
- **Fail fast**: if a fake doesn't support a code path, raise an error immediately rather than returning wrong results

## Stubbing: When Appropriate
Stubbing is appropriate when you need a function to return a **specific value** to get the SUT into a certain state.
- Each stub should have a **direct relationship** with the test's assertions
- Few stubs per test; many stubs = sign of overuse or overly complex SUT
- Real implementations and fakes are **still preferred** even when stubbing is appropriate

## Interaction Testing: Best Practices
When interaction testing is necessary (e.g., no real impl or fake available):
- Only test **state-changing** functions (`sendEmail()`, `saveRecord()`), NOT non-state-changing ones (`getUser()`, `findResults()`)
- **Avoid overspecification**: verify only the minimum arguments and calls needed for the behavior being tested
- Use `any()` matchers for arguments irrelevant to the test
- Supplement with **larger-scoped tests** that perform state testing against real implementations

---

## Exam/Test Patterns (Frequently Tested)
| Scenario/Keyword | Answer |
|-------------------|--------|
| Google's first choice for tests | **Real implementations** (classical testing) |
| Fake vs. Stub | Fake has **behavior and state**; stub returns **hardcoded values** |
| @DoNotMock | Annotation preventing mocking; directs to **fakes or real impl** |
| Overuse of stubs → | Tests become **unclear, brittle, and less effective** |
| Interaction testing risk | **Change-detector tests**—break on any implementation change |
| Prefer state testing because | Validates the system **actually works**, not just that calls were made |
| When to use real impl | When it is **fast, deterministic, simple dependencies** |
| Fake fidelity requirement | Must match **API contract** (same output for same input) |
| Who should write fakes | The **team that owns the real implementation** |
| Contract tests | Run against **both real and fake** to ensure fidelity |
| Interaction test: which functions | Only **state-changing** functions (sendEmail, saveRecord) |

## Related Notes
- [[Testing Overview]]
- [[Unit Testing]]
- [[Larger Testing]]
- [[Software Engineering Fundamentals]]
- [[Continuous Integration]]

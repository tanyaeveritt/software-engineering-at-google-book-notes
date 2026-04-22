---
source_pdf: swe_at_google.2.pdf
part: Part IV
keywords: practice, code search, Kythe, indexing, ranking
---

# Code Search Practice (10 questions)

#practice #code-search #tools

## Related Concepts
- [[Code Search]]

> [!hint]- Key Patterns (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Why not local IDE index | Codebase too large; centralized index scales linearly vs quadratic |
> | Kythe | Cross-references (definition, usages); updated daily |
> | Search index latency | <10 seconds from commit to visibility |
> | Most common search intent | "How" (33%) — seeing examples of API usage |
> | Sparse n-gram | 500x more efficient than brute force |
> | Canonical code link | All tools link to source code via Code Search |

---

## Question 1 - Five Search Intents [recall]
> List the five categories of developer intent when using Code Search and their approximate percentages.

> [!answer]- Show Answer
> 1. **How** (33%) — seeing examples of how an API is used
> 2. **What** (25%) — understanding what code does (exploratory browsing)
> 3. **Where** (16%) — finding a specific definition, config, or file
> 4. **Why** (16%) — understanding why code behaves a certain way (debugging)
> 5. **Who/When** (8%) — who introduced code and when, linked to version control history
>
> The most common intent is "How," reflecting developers' need to find working examples.

---

## Question 2 - Why Not an IDE? [recall]
> Give two reasons why Google built Code Search as a separate web tool instead of relying on IDE-based search.

> [!answer]- Show Answer
> 1. **Scale**: Google's codebase (~1.5 TB of content) is too large for a local IDE to index. A centralized cloud-based index is built once and shared across all developers, with linear scaling cost. Per-developer IDE indexing scales quadratically (more developers produce more code, and each IDE re-indexes the growing codebase).
> 2. **Specialization**: Code Search is not an IDE; its UX is optimized for browsing and understanding code rather than editing it. Every mouse click on a symbol is meaningful (show usages, jump to definition) rather than moving a cursor. Developers commonly have multiple Code Search tabs open alongside their editor.

---

## Question 3 - Kythe Cross-References [recall]
> What is Kythe and why can't its index be updated as quickly as the search index?

> [!answer]- Show Answer
> **Kythe** is a semantic indexing tool that provides cross-references: jump to symbol definitions, find all usages, and fetch decorations for a file. It instruments the build workflow to extract semantic nodes and edges from source code. Its index **cannot be incrementally updated** because any code change can potentially influence the entire codebase—nearly all of Google's full binaries need to be built or analyzed to determine semantic structure. Building the index takes hours and is done **daily**, compared to the search index which updates within **10 seconds** of a commit.

---

## Question 4 - Indexing Resource Estimate [recall]
> How does the book estimate the compute resources needed for brute-force search on Google's codebase?

> [!answer]- Show Answer
> RE2 regex matching processes ~100 MB/sec per core. For 1.5 TB of data in a 50ms window, ~300,000 cores would be needed for a single query. Using specialized substring search at ~1 GB/sec reduces this to ~30,000 cores. With ~200 queries/sec (10 concurrent in any 50ms window), it's back to ~300,000 cores. This is why the sparse n-gram index, which is **500x more efficient** than brute force, is essential.

---

## Question 5 - Ranking Signals [recall]
> Distinguish between query-independent and query-dependent signals in Code Search ranking.

> [!answer]- Show Answer
> **Query-independent signals** can be computed offline and don't depend on the search query: (1) **file view count** — frequently viewed files score higher; (2) **number of references** — analogous to PageRank, using import/include statements and build dependencies. **Query-dependent signals** must be computed per-query and are restricted to cheap-to-compute features: (1) **clean token matches** (word boundaries, case sensitivity); (2) **filename/symbol matches** boosted over content matches; (3) **path matching** — queries that qualify a path boost results where the path matches.

---

## Question 6 - Completeness Trade-off [recall]
> Why does Google choose to index "too much" rather than drop files to save resources?

> [!answer]- Show Answer
> For developers to **trust** Code Search, they need confidence that results are complete. It is generally impossible to give feedback about incomplete results for a search if dropped files weren't indexed in the first place. The resulting confusion and productivity loss is a high price for saved resources. Even if developers know the limitations, they'll perform the search in an ad hoc, error-prone way. Google errs on the side of indexing too much, with limits mainly to prevent abuse rather than save resources.

---

## Question 7 - Integration Platform [application]
> Your company is building a centralized code search tool. What integrations would maximize its value, based on Google's experience?

> [!answer]- Show Answer
> Based on Code Search's role as the **canonical code location**: (1) **Log viewers** should link log lines back to source code at specific revisions. (2) **Crash/stack traces** should link frames to source, restricted to the exact build revision for accuracy. (3) **Documentation** should embed live code snippets from head, remaining current without manual updates. (4) **Code review tools** should provide cross-reference navigation. (5) **Compilation errors and test failures** should linkify file/line references. (6) Expose **search and cross-reference APIs** so other tools can build on the platform without reimplementing. This approach frees tool creators from building UIs and ensures all developers benefit.

---

## Question 8 - Search Index Evolution [application]
> Explain why Google moved from a suffix-array-based index to a token-based n-gram solution. What trade-off was involved?

> [!answer]- Show Answer
> The move from suffix arrays to token-based n-grams was driven by the ability to leverage **Google's primary indexing and search stack** (standard reverse index technology). With suffix arrays, building and distributing custom indices was a challenge in itself. By using standard technology, Code Search benefits from all advances in reverse index construction, encoding, and serving made by the core search team, including **instant indexing**. The trade-off was implementation simplicity vs. performance: actual retrieval, matching, and scoring remain highly customized and optimized on top of the standard indices.

---

## Question 9 - Workspace Search [recall]
> How does Code Search handle searching in developers' local workspaces (unsubmitted changes)?

> [!answer]- Show Answer
> Multiple machines do **simple brute-force searches** on workspace data. The workspace data is loaded on the first request and kept in sync by listening for file changes. When memory runs out, the least recently used workspace is evicted. For unchanged documents, the **history index** is used instead. The search is implicitly restricted to the repository state to which the workspace is synced. This works because workspaces have a small delta from the global repository.

---

## Question 10 - Trust vs Completeness [analysis]
> Google's Code Search team once excluded generated JavaScript files from the index. Analyze the benefits and risks of this decision using the "developer trust" principle from the chapter.

> [!answer]- Show Answer
> **Benefits**: Generated/obfuscated JavaScript is unreadable, so excluding it reduces index size, resource consumption, and result noise. **Risks**: Developers who need to search generated JS (e.g., debugging production issues with obfuscated code) will find no results and may lose trust in the tool. The "trust" principle says developers must be able to rely on completeness—if they can't, they resort to ad hoc searches that are error-prone and slow. The book's recommendation is that such exclusions are "probably correct" but must be made cautiously, since the cost of lost trust (rare but potentially high) outweighs saved resources. The general policy is to err on the side of indexing too much, with limits set for system stability rather than resource savings.

---

> [!summary]- Pattern Summary (click to view)
> | Keyword | Answer |
> |---------|--------|
> | Most common intent | **How** (33%) — examples of API usage |
> | Scale problem | Local IDE indexing scales quadratically; centralized is linear |
> | Kythe | Semantic cross-references; updated daily (not instantly) |
> | Search index latency | <10 seconds from commit to visibility |
> | Sparse n-gram | 500x more efficient than brute force |
> | Ranking signals | Query-independent (views, refs) + Query-dependent (token, path) |
> | Completeness | Index nearly everything for developer trust |
> | Canonical link | All tools link to code via Code Search |

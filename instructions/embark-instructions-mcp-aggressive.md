# Tools

## Semantic Code Search: `code_search`

Use `code_search` to search the indexed codebase by meaning.

Before searching, ask yourself what Grep query you would actually write.

### Mandatory decision boundary

You MUST start with `code_search` when the task is to find existing code by behavior, responsibility, interaction, or pattern and the exact symbol, file, or literal text is not yet known.

You MUST NOT start with `code_search` when the task already gives a precise anchor, such as:
- an exact symbol name
- an exact string or import
- a file path
- a test name
- a config key
- a stack-trace fragment
- a tight regex
- a likely directory to inspect directly

In those cases, use **Grep** / **Glob** / **Read** instead.

Use **Glob** when you need files by name or extension.  
Use **Read** when the path is already known.  
Use **Explore / Task** for broad multi-file synthesis, optionally seeded by one `code_search` call.

`code_search` and Grep are stages, not substitutes.  
When `code_search` is the first move, Grep usually follows to narrow within the files it surfaced.

### Use `code_search` for these question types

Use `code_search` when the real question is:
- How does X work?
- Where does behavior Y live?
- Which components are involved in Z?
- What pattern is used for W?
- Which tests exercise behavior X?
- Where is the analogous implementation of this behavior?

Even if a symbol is named in the task, still use `code_search` when the question is behavioral or relational.

Examples:
- `Find EmailService` → **Grep**
- `How does EmailService coordinate with the retry controller during partial failures?` → **`code_search`**

### How to call it

For `text`:
- Use one focused natural-language query for ONE concept.
- Write a short question or sentence.
- Add a short purpose clause only when it sharpens ranking.

Good:
- `How is pagination implemented in the products list endpoint?`
- `Which tests exercise lambda-parameter indentation handling?`
- `Where is outbound HTTP request timeout configured? I want to raise it to 30s.`
- `How does the auth middleware validate the JWT before the controller?`

Bad:
- `email`
- `class User`
- `product composite REST controller integration service reviews productId openapi tests`
- `How is auth done and where is pagination and what about caching?`

For `pathFilter`:
- Use it only when you already have evidence for the right directory or file.
- It MUST be relative to the project root.
- Omit it, or leave it empty, to search the whole project.
- Never use absolute paths, leading slashes, `.`, `./`, `*`, or globs.

Good:
- `src/auth`
- `services/payments`
- `ktlint-ruleset-standard`
- `app/models/user.py`

Bad:
- `/Users/...`
- `/src/auth`
- `.`
- `./`
- `*`
- `src/auth/**`

### After a miss

Do not abandon semantic search after one miss.

Retry in this order:
1. Rephrase with different vocabulary.
2. Remove `pathFilter`.
3. Split a compound question into separate calls.

After 1–2 retries, fall back to Grep / Glob / Explore if needed.
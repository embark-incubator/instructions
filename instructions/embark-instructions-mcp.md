# Tools

## Semantic Code Search: `code_search`

`code_search` is an MCP tool that performs semantic search over the indexed codebase.
Given a natural-language question it returns ranked snippets (path + line numbers + code).
Ranking considers semantic meaning as well as symbol, file, and directory names.

### Which search tool fits which question?

Before searching, ask yourself: *what Grep query would I write to answer
this?*

| What your Grep query would look like | Tool |
|---|---|
| A specific symbol, literal, or tight regex that will uniquely identify the target (`class EmailService`, `@JsonTypeInfo`, `DEFAULT_TIMEOUT_MS`) | **Grep** |
| A fuzzy concept word you'd expect to match too much or miss synonyms (`payment`, `retry`, `auth`, `handler`), OR you do not yet know what symbol to Grep for | **`code_search`** |
| Enumerating files by name or extension | **Glob** |
| Path is already known | **Read** |
| Genuinely open-ended research — multi-round, needs planning | **Explore / Task subagent**, optionally seeded by one `code_search` call first |

The factor is **the precision of the Grep input you'd need**, not whether the question is locational or behavioural and not whether a symbol is named in the prompt.
"Find `EmailService`" has a precise Grep input (`class EmailService`) — use Grep.
"Find the class that handles payment processing" has a fuzzy Grep input (`payment` matches everything in a microservices repo) — use `code_search` even though it's also a location question.

Important note on the tooling decision boundary:
`code_search` and Grep are stages, not alternatives.
When `code_search` is your first move, Grep typically follows to narrow within the files it surfaced — not to replace the initial search.

### Typical workflow for a task on unfamiliar code

1. One focused `code_search` question to locate the area of interest.
2. `Read` the top-ranked files for context.
3. `Grep` for precise symbol or usage follow-up within those files.
4. `Edit` / `Write` once the target is confirmed.

### If a call returns no useful results

Do not abandon the tool after one miss. Reformulate:

1. Rephrase using different vocabulary (domain term ↔ implementation term).
2. Remove `pathFilter` if you had set one — a wrong filter silently hides matches.
3. Split a compound question into separate focused calls.

Fall back to Grep / Glob / Explore only after 2–3 reformulations still fail.

### Example calls 

Good queries:
```json
{ "text": "How does `EmailService` coordinate with the retry controller during partial failures? I want to add exponential backoff.", "pathFilter": "" }                                           
{ "text": "How do other REST controllers integrate with `FeatureFlagsService`? I want to match the existing wrapper pattern.", "pathFilter": "" }                                                  
{ "text": "Which services consume events emitted by `PaymentHandler`?", "pathFilter": "" }                                                           
{ "text": "Which tests exercise `IndentationRule` lambda-parameter handling?", "pathFilter": "ktlint" }  
{ "text": "How is pagination implemented in the products list endpoint?", "pathFilter": "services/products" }
{ "text": "Where is the request timeout configured for outbound HTTP calls?", "pathFilter": "" }
{ "text": "How does the auth middleware validate the JWT before the controller? I want to add an audience check.", "pathFilter": "src/auth" }
{ "text": "Which function parses URL query parameters?", "pathFilter": "" }
{ "text": "Where is the email validation rule on the user model defined? I want to loosen it.", "pathFilter": "app/models" }
```

Bad queries:
```json
{ "text": "email", "pathFilter": "" }
{ "text": "class User", "pathFilter": "." }
{ "text": "product composite REST controller integration service reviews productId openapi tests", "pathFilter": "" }
{ "text": "How is auth done and where is pagination and what about caching?", "pathFilter": "*" }
```
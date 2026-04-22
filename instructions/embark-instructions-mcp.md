# Tools

## Semantic Code Search: `code_search`

The `code_search` MCP tool performs search over the indexed codebase.
Given a natural-language query it returns ranked snippets (path + line numbers + code snippet).
Ranking considers semantic meaning as well as symbol, file, and directory names.

### Where it fits among your search tools

Before searching, ask yourself what *shape* of answer you need:

| Shape of the question                                                           | Tool |
|---------------------------------------------------------------------------------|---|
| "How does X work?"                                                              | **`code_search`** |
| "Where does behaviour Y live?"                                                  | **`code_search`** |
| "Where are the tests for X?"                                                    | **`code_search`** |
| "Which components are involved in Z?"                                           | **`code_search`** |
| "Where is this exact symbol / string / import / regex *defined or used*?"       | **Grep** |
| "Which files match this name or extension?"                                     | **Glob** |
| "Open this specific file."                                                      | **Read** |
| "Research this across many files and synthesize." — multi-round, needs planning | **Explore / Task subagent**, optionally seeded by one `code_search` call |

The deciding factor is the *shape of the question*, not whether a symbol is already named in the task. 
For example:
- "Find `EmailService`" is a location question, so use Grep.
- "How does `EmailService` coordinate with the retry controller during partial failures?" is a behaviour question even though `EmailService` is named - use `code_search`.

Important note on the tooling decision boundary:
`code_search` and Grep are stages, not alternatives.
When `code_search` is your first move, Grep typically follows to narrow within the files it surfaced — not to replace the initial search.


### How to call it
- A single specific natural-language query describing ONE thing to find. Specify:
  - WHAT you want to find — the class, behaviour, interaction, or concept.
  - WHY you want it (optional but helpful) — a short purpose clause of the search.
- (Optional) A path RELATIVE to the project root, used to scope the search to a subdirectory or single file. Omit or leave empty to search the whole project.
  - Must be relative. Do NOT pass absolute paths ("/testbed/...", "/Users/..."), leading slashes, or shell-style values ("*", "./", ".").
  - No wildcards — the filter is a path prefix or file path, not a glob.
- If you need several different things, make separate calls instead.
- Avoid in queries:
  - Single words or short keywords: "email", "auth", "timeout".
  - Noun phrases without a verb: "batch importer for bibliographic records". Rewrite as "Where is the batch importer that transforms bibliographic records into the normalized line format?"
  - Keyword bags joined with no connectives: "product composite REST controller integration service reviews productId openapi tests".

### If a call returns no useful results

Do not abandon the tool after one miss. Retry with a reformulation:
1. Rephrase using different vocabulary.
2. Remove `pathFilter` if you had set one - a wrong path silently hides matches.
3. Decompose a compound question into its single sub-concept calls.

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
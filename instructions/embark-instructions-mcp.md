# Tools

## Semantic Code Search: `code_search` (EmbArk)

The `code_search` MCP tool performs semantic search over the indexed codebase. Given a natural-language question it returns ranked snippets (path + line numbers + code). Ranking considers semantic meaning as well as symbol, file, and directory names.

### Where it fits among your search tools

Pick the search tool that matches the question:

| Question you are asking | Use |
|---|---|
| "Find this exact symbol / string / import / regex" | **Grep** |
| "Find files whose name or extension matches…" | **Glob** |
| "Read this specific file" | **Read** |
| "Where in the codebase is CONCEPT X implemented / handled?" — you know the idea, not the symbols | **`code_search`** |
| "Research this across many files and synthesize" — multi-round, needs planning | **Explore / Task subagent** (optionally seeded by one `code_search` call first) |

Prefer `code_search` over spawning an Explore subagent when a single well-phrased question is enough — it is faster and cheaper. Escalate to Explore only if one focused query is clearly insufficient.

### How to call it

- Pass ONE specific natural-language question in `text`. A short purpose clause at the end is fine (it sharpens ranking), but do not dump keywords.
- If you need several different things, make several focused calls.
- `pathFilter` is relative to the project root; omit it when you do not yet know the right subtree.

### Example calls

Good patterns (write queries like these):
```json
{ "text": "Where is the request timeout configured for outbound HTTP calls?", "pathFilter": "" }
{ "text": "How does the auth middleware validate the JWT before the controller? I want to add an audience check.", "pathFilter": "src/auth" }
{ "text": "Which function parses URL query parameters and how does it handle empty values?", "pathFilter": "" }
{ "text": "Where is the email validation rule on the user model defined? I want to loosen it.", "pathFilter": "app/models" }
{ "text": "How is pagination implemented in the products list endpoint?", "pathFilter": "services/products" }
```

Anti-patterns (do not write queries like these):

```json
{ "text": "email", "pathFilter": "" }
{ "text": "class User", "pathFilter": "." }
{ "text": "product composite REST controller integration service reviews productId openapi tests", "pathFilter": "" }
{ "text": "How is auth done and where is pagination and what about caching?", "pathFilter": "" }
```

### Typical interaction pattern

1. `code_search` with a focused question to locate the area of interest.
2. `Read` the top-ranked files for context.
3. `Grep` for precise symbol or usage follow-up within those files.
4. Edit / Write once the target is confirmed.

### If a call returns no useful results

Do not abandon the tool after one miss. Retry with a reformulation:
1. Rephrase using different vocabulary (domain term ↔ implementation term).
2. Remove `pathFilter` if you had set one — a wrong path silently hides matches.
3. Decompose a compound question into its single sub-concepts.

Fall back to Grep / Glob / Explore only after 2–3 reformulations still fail.
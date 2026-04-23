## `code_search` - Semantic Code Search

`code_search` (also available as `mcp__embark__code_search`) is the primary tool for code exploration and search.

### When to Use code_search

Prefer `code_search` over Grep/Glob/find for:
- Understanding what code does or where functionality lives
- Finding implementations by intent (e.g., "authentication logic", "error handling")
- Exploring unfamiliar parts of the codebase
- Any search where you describe what the code does rather than exact text

### When to Use Standard Tools

Use Grep/Glob when you need:
- Exact text matching (variable names, imports, specific strings)
- File path patterns (e.g., `**/*.go`)

### Fallback

If `code_search` is unavailable or errors out, fall back to standard Grep/Glob tools.

### Usage

- Specify a focused search query
- Use English queries for best results
- (Optional) Specify a path filter to scope the search to a subdirectory or single file to narrow down the results
  - Use it only when you already have evidence for the right directory or file
  - It must be relative to the project root
  - Omit it, or leave it empty, to search the whole project
  - Do not use absolute paths, leading slashes, `.`, `./`, `*`, or globs

### Examples

Good:
```json
{ "text": "user authentication flow", "pathFilter": "" }
{ "text": "how is pagination implemented in the products list endpoint?", "pathFilter": "services/products" }
{ "text": "request timeout configured for outbound HTTP calls", "pathFilter": "" }
{ "text": "which tests exercise `IndentationRule` lambda-parameter handling?", "pathFilter": "ktlint" }
```

Bad:
```json
{ "text": "email", "pathFilter": "." }
{ "text": "product composite REST controller integration service reviews productId openapi tests", "pathFilter": "" }
{ "text": "how is auth done and where is pagination and what about caching?", "pathFilter": "*" }
```

### Query Tips

- **Use English** for queries (better semantic matching)
- **Describe intent**, not implementation: "handles user login" not "func Login"
- **Be specific**: "JWT token validation" better than "token"
- Results include: file path, line numbers, code preview

### Workflow

1. Start with `code_search` to find relevant code
2. Use `Read` to examine files from the results
3. Use Grep for exact string searches when needed

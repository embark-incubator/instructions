## `embark search` - Semantic Code Search

**IMPORTANT: You MUST use `embark search` as your PRIMARY tool for code exploration and search.**

### When to Use `embark search` (REQUIRED)

Use `embark search` INSTEAD OF Grep/Glob/find for:
- Understanding what code does or where functionality lives
- Finding implementations by intent (e.g., "authentication logic", "error handling")
- Exploring unfamiliar parts of the codebase
- Any search where you describe WHAT the code does rather than exact text

### When to Use Standard Tools

Only use Grep/Glob when you need:
- Exact text matching (variable names, imports, specific strings)
- File path patterns (e.g., `**/*.go`)

### Fallback

If `embark search` fails (not connected or errors), fall back to standard Grep/Glob tools.

### Usage

- Specify a focused search query
- ALWAYS use English queries for best results
- (Optional) Specify a path filter to scope the search to a subdirectory or single file to narrow down the search results
  - Use it only when you already have evidence for the right directory or file
  - It MUST be relative to the project root
  - Omit it, or leave it empty, to search the whole project
  - Never use absolute paths, leading slashes, `.`, `./`, `*`, or globs

```bash
embark search "<QUERY>"
embark search -p "<PATH>" "<QUERY>"  # <PATH> must be relative to the project root
```

### Examples

GOOD:
```bash
embark search "user authentication flow"
embark search -p "services/products" "how is pagination implemented in the products list endpoint?"
embark search "request timeout configured for outbound HTTP calls"
embark search -p "ktlint" "which tests exercise `IndentationRule` lambda-parameter handling?"
```

BAD:
```bash
embark search -p "." "email"
embark search "product composite REST controller integration service reviews productId openapi tests"
embark search -p "*" "how is auth done and where is pagination and what about caching?"
```

### Query Tips

- **Use English** for queries (better semantic matching)
- **Describe intent**, not implementation: "handles user login" not "func Login"
- **Be specific**: "JWT token validation" better than "token"
- Results include: file path, line numbers, code preview

### Workflow

1. Start with `embark search` to find relevant code
2. Use `Read` tool to examine files from results
3. Only use Grep for exact string searches if needed
---
name: embark-search
description: Use embark-search for semantic code search. Use when you need to find code by meaning rather than exact text matching. Perfect for finding concepts like "authentication logic", "error handling patterns", or "database connection pooling".
allowed-tools: ["Bash"]
---

# embark-search - Semantic Code Search

Use `embark-search` to search code semantically using natural-language queries.
`embark-search` understands code meaning, not just text patterns.

## When to Use `embark search`

Prefer `embark search` over Grep/Glob/find for:
- Understanding what code does or where functionality lives
- Finding implementations by intent (e.g., "authentication logic", "error handling")
- Exploring unfamiliar parts of the codebase
- Any search where you describe what the code does rather than exact text

## When to Use Standard Tools

Use Grep/Glob when you need:
- Exact text matching (variable names, imports, specific strings)
- File path patterns (e.g., `**/*.go`)

## Fallback

If `embark search` is unavailable or errors out, fall back to standard Grep/Glob tools.

## Usage

- Specify a focused search query
- Use English queries for best results
- (Optional) Specify a path filter to scope the search to a subdirectory or single file to narrow down the results
  - Use it only when you already have evidence for the right directory or file
  - It must be relative to the project root
  - Omit it, or leave it empty, to search the whole project
  - Do not use absolute paths, leading slashes, `.`, `./`, `*`, or globs

```bash
embark search "<QUERY>"
embark search -p <PATH> "<QUERY>"  # <PATH> must be relative to the project root
```

## Examples

Good:
```bash
embark search "user authentication flow"
embark search -p services/products "how is pagination implemented in the products list endpoint?"
embark search "request timeout configured for outbound HTTP calls"
embark search -p ktlint "which tests exercise `IndentationRule` lambda-parameter handling?"
```

Bad:
```bash
embark search -p . "email"
embark search "product composite REST controller integration service reviews productId openapi tests"
embark search -p * "how is auth done and where is pagination and what about caching?"
```

## Query Tips

- **Use English** for queries (better semantic matching)
- **Describe intent**, not implementation: "handles user login" not "func Login"
- **Be specific**: "JWT token validation" better than "token"
- Results include: file path, line numbers, code preview
---
name: embark-search
description: Use embark-search for semantic code search. Use when you need to find code by meaning rather than exact text matching. Perfect for finding concepts like "authentication logic", "error handling patterns", or "database connection pooling".
allowed-tools: ["Bash"]
---

# embark-search - Semantic Code Search

Use `embark-search` to search code semantically using natural-language queries. `embark-search` understands code meaning, not just text patterns.

## When to Use

- Finding code by concept or functionality ("where do we handle authentication?")
- Discovering related code patterns ("show me retry logic")
- Exploring codebase structure ("how is the database connection managed?")
- Searching for implementation patterns ("where do we validate user input?")

## Basic Usage

### Search Command

```bash
embark search "your natural language query"
```

### Common Patterns

**Find functionality:**
```bash
embark search "where do we handle user authentication?"
```

**Search in a specific path:**
```bash
embark search -p "src/main/kotlin" "serialization configuration"
```

**Get more results:**
```bash
embark search --limit 50 "database queries"
```

**Search definitions and declarations:**
```bash
embark search --index-alias symbols "email validation function"
```

**Quick scan with filenames only:**
```bash
embark search --renderer filenames "API rate limiting"
```

**Show code snippets for all results:**
```bash
embark search --renderer code_snippet "retry logic"
```

**Agent-friendly JSON output:**
```bash
embark search --json-output "logging configuration"
```

## Command Options

- `--index-alias <value>`: Search `code-blocks`, `symbols`, or `files`
- `--limit <n>`: Maximum results to return
- `--path-filter <value>` / `-p <value>`: Restrict results to a path prefix. Must be relative. Do NOT pass absolute paths, leading slashes, or shell-style values ("*", "./", ".").
- `--renderer <mode>`: Output mode: `hybrid`, `code_snippet`, or `filenames`
- `--json-output`: Emit structured JSON output

## Examples

**Find authentication code:**
```bash
embark search -p "auth/" "how do we authenticate users?"
```

**Find error handling:**
```bash
embark search "error handling of `Users` class"
```

**Search specific directories (combine multiple filters):**
```bash
embark search --limit 30 -p "src/" "API endpoint definitions of LLM models"
```

## Understanding Results

Results vary by `--renderer`:

- `hybrid`: Shows code snippets for high-relevance results and filenames for lower-relevance ones
- `code_snippet`: Shows detailed result info with code previews for every result
- `filenames`: Shows only similarity score and file path

Use `symbols` when looking for declarations, `code-blocks` for implementation details, and `files` for broad discovery.

## Best Practices

1. Use natural-language queries, not just keywords. Ask questions like you would ask a colleague
2. Start with `code-blocks` for behavior and implementation searches.
3. Switch to `symbols` when looking for named declarations or definitions.
4. Add `-p` to narrow the search space to a specific directory.
5. Use `--renderer code_snippet` for inspection and scanning by default.
6. Use `--json-output` for scripts, agents, and automated post-processing.
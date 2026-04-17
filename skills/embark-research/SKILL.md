---
name: embark-research
context: fork
agent: Explore
argument-hint: query
description: "Research and understand unfamiliar codebases using semantic search with `embark search` and `embark history`"
---

You need to gather all context for task $ARGUMENTS thoroughly.

## When to Use

- Understanding unfamiliar codebases or locating specific functionality
- Finding implementations, definitions, or usage patterns
- Identifying code related to specific features or concepts
- Before making changes to understand the context and impact

## Tools Available

### `embark search`

Semantic code search that finds code by meaning, not just exact keywords.

```bash
embark search "<descriptive query>" [path]
embark search -p <path> "<query>" --limit 10  # <path> must be relative to the project root
embark search "<query>" --reranker # improve search results
embark search --json "<query>"  # For structured output
```

**Query Tips:**
- Be descriptive: "function that validates user email addresses" > "email"
- Include context: "error handling middleware for HTTP requests with logging"
- Specify what you're looking for: "React component that renders a modal dialog"

### `embark history`

Search through commit messages and changes to understand how code evolved.

```bash
embark history "<query>"
embark history --limit 20 "<query>"  # Get more results
embark history --json "<query>"  # For structured output
```

**Use Cases:**
- Find when a feature was added: "add user authentication"
- Find bug fixes: "fix memory leak in worker"
- Understand refactoring: "refactor database connection"

## Research Workflow

1. **Start broad**: Use `embark search` with general terms to understand the landscape
2. **Narrow down**: Add path filters (`-p`, relative to the project root) once you identify relevant directories
3. **Check history**: Use `embark history` to understand why code was written a certain way
4. **Read the code**: Once you find relevant files, read them to understand the details

## Example Session

```bash
# Find authentication-related code
embark search "user authentication login" --reranker --limit 5

# Narrow to specific directory
embark search -p src/auth "JWT token validation"

# Understand how auth evolved
embark history "add JWT authentication"
embark history "fix authentication bug"
```

# Tools

## Semantic Code Search (EmbArk)

You have access to the `mcp__embark-mcp__semantic_code_search` tool for searching the codebase semantically.
This tool can search for code snippets related in meaning to the search query and search objective.

### When to use semantic search:
- Understanding unfamiliar codebases or locating specific functionality.
- Finding implementations, definitions, or usage patterns.
- Identifying code related to specific features or concepts.
- Before making changes to understand the context and impact.

### Usage rules:
- The tool searches by MEANING, not just exact string matches.
- The tool takes the `text` parameter that should combine both what you're looking for and why:
    - Construct the `text` parameter by combining: what to find + search objective.
    - Make search objective in the query specific and self-contained, describe what you are trying to achieve by search and how the search results should help.
    - Objective should always provide an additional context of the query, describing the higher-level task.
- The tool optionally takes the `pathFilter` parameter to narrow the search to a specific directory or file:
   - Using `pathFilter` dramatically improves search relevance and precision.
   - Semantic search without path constraints might return irrelevant results from unrelated modules.
   - If not provided, the whole project is searched.

### Examples:
- `"text"="Refactor `MyFunction` to handle async operations", "pathFilter"="path/to/module"`: Searches `MyFunction` within the specified directory for refactoring.
- `"text"="Find `authorization` to add OAuth2 support to existing auth system", "pathFilter"="path/to/file/example.kt"`: Searches `authorization` inside the specified file to add OAuth2 support.
- `"text"="Find `class User` to add email validation to user model"`: Finds the definition of class `User` to add email validation.
- `"text"="Find `class User` to add email validation to user model", "pathFilter"="path/to/file/example.kt"`: Finds the definition of class `User` inside the specified file to add email validation.
- `"text"="Locate query with retries to improve error handling in database queries"`: Searches for anything containing `query*with*retries` in directory names, filenames, symbol names and semantic matches by meaning. It also returns a code line with the exact phrase `query with retries`.
- `"text"="Find `fun getConfig` to change default headers in authentication", "pathFilter"="path/to/module/submodule"`: Searches for `fun getConfig` within the specified directory `path/to/module/submodule` to change the default headers in authentication.

Use this tool proactively when you need to understand code structure or locate relevant implementations.
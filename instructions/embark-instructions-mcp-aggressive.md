# Tools

## Semantic Code Search: `code_search` (EmbArk)

The `code_search` MCP tool performs semantic search over the indexed codebase. On any code-search task where you know the concept, behaviour, or feature but NOT the exact symbol, `code_search` is MANDATORY and comes FIRST.

### Hard ordering rules

On a concept-level code-search task you MUST:
1. Call `code_search` with a focused natural-language question FIRST.
2. Retry with a reformulation if the first call misses (2–3 attempts).
3. Escalate to Explore / Task / broad Grep ONLY after reformulations still fail.

Skipping `code_search` and jumping straight to Explore, Task, Glob-sweep, or speculative keyword-Grep on a concept-level question is the most common failure mode on this stack. Do not do it.

### Tool selection (strict)

| Situation | Tool |
|---|---|
| Know a concept or behaviour, not the exact symbol | **`code_search` FIRST** |
| Starting on an unfamiliar codebase or localising a bug | **`code_search` FIRST** |
| About to spawn Explore / Task for code-finding research | **`code_search` FIRST** — escalate only if it is not enough |
| Exact symbol / string / import / regex is known | **Grep** — `code_search` is NOT a grep replacement |
| Enumerate files by name / extension | **Glob** |
| Path already known | **Read** |

Do NOT use `code_search` to replace Grep on exact-symbol lookups — Grep is deterministic and faster. Use `code_search` to replace the *Explore-first* habit on concept lookups, and to replace speculative keyword-Grep sweeps when you do not know the symbol.

### How to call it (strict)

- `text`: ONE specific natural-language question, full sentence. NOT a keyword bag. NOT a bare symbol. A short purpose clause ("I want to change X") sharpens ranking.
- `pathFilter`: a RELATIVE path only ("src/auth", "app/models/user.py"). NEVER absolute ("/testbed/..."), NEVER wildcards ("*", ".", "./"). Omit when you do not yet have evidence of the right subtree — a wrong filter silently hides all matches.
- Several different things → several focused calls. Never jam multiple questions into one `text`.

### After a miss — retry before fallback

A single miss is NOT a reason to abandon the tool. You MUST retry before falling back:
1. Rephrase with different vocabulary (domain term ↔ implementation term).
2. Drop the `pathFilter`.
3. Split a compound question into single sub-concepts.

Only after 2–3 reformulations still fail may you fall back to Explore / Grep / Glob.
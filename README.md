# EmbArk Skills

A collection of skills, agents, and instructions that integrate [EmbArk](https://embark.labs.jb.gg/) — a semantic code search CLI — into your AI coding workflow.

EmbArk lets your coding agent search your codebase by meaning, not just keywords. These extensions make that integration seamless.

## Install EmbArk

See the [install guide](https://embark.labs.jb.gg/install.md), or quick-start:

```bash
# macOS / Linux
curl -fsSL embark.labs.jb.gg/install.sh | bash

# Windows (PowerShell)
irm https://embark.labs.jb.gg/install.ps1 | iex
```

## What's Included

### Skills

Drop-in slash commands for Claude Code.

| Skill | Description |
|---|---|
| `embark-search` | `/embark-search` — run a semantic code search |
| `embark-research` | `/embark-research` — deep codebase exploration using `embark search` and `embark history` |
| `embark-install` | `/embark-install` — install the EmbArk CLI |

### Instructions

Ready-made `CLAUDE.md` snippets that teach Claude to use EmbArk automatically. Pick the variant that fits your setup:

| File | Mode | Integration |
|---|---|---|
| `embark-instructions.md` | Standard | CLI (`embark search`) |
| `embark-instructions-aggressive.md` | Aggressive | CLI (`embark search`) |
| `embark-instructions-mcp.md` | Standard | MCP (`code_search` tool) |
| `embark-instructions-mcp-aggressive.md` | Aggressive | MCP (`code_search` tool) |
| `claude-md.md` | Standard | MCP (detailed usage examples) |

**Standard** — suggests using EmbArk when appropriate.
**Aggressive** — mandates EmbArk as the primary search method over grep/ripgrep.

## Usage

Call `embark setup-agent --help` to learn how to install it into the agent or install it manually by copying.
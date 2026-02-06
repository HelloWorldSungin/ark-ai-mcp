# CLAUDE.md - ark-ai-mcp Plugin

## Overview

ark-ai-mcp is a Claude Code plugin providing MCP wrapper skills that call external services (Linear, GitHub, Context7, DeepWiki, Playwright) via the mcporter CLI. Each skill replaces ~10-30 MCP tool definitions with a single on-demand skill invocation, saving context window.

## Skill Structure

Each skill lives in `skills/<name>/SKILL.md` with:
- **YAML frontmatter**: `name`, `description`, `allowed-tools: [Bash]`
- **`<objective>`**: What the skill does and when to use it
- **`<process>`**: Call syntax, tool reference, common examples
- **`<tips>`**: Gotchas, auth requirements, performance notes

## Adding a New MCP Skill

1. Create `skills/mcp-<server>/SKILL.md`
2. Run `mcporter list <server> --all-parameters` to discover available tools
3. Follow the existing skill structure (objective, process, tips)
4. Test: invoke `/mcp-<server>` in a Claude Code session

## Installation

```bash
claude /plugin marketplace add HelloWorldSungin/ark-ai-mcp
claude /plugin install ark-ai-mcp@ark-ai-mcp
```

Or as a local submodule:
```bash
claude /plugin marketplace add ./external/ark-ai-mcp
claude /plugin install ark-ai-mcp@ark-ai-mcp
```

## Prerequisites

- **mcporter** installed globally (`brew install steipete/tap/mcporter` or from source)
- **~/.mcporter/mcporter.json** configured with MCP server definitions
- Per-server auth: `GITHUB_TOKEN` for GitHub, Linear OAuth via mcporter, etc.

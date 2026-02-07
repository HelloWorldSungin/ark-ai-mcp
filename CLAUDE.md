# CLAUDE.md - ark-ai-mcp Plugin

## Overview

ark-ai-mcp is a Claude Code plugin providing MCP wrapper skills that call external services (GitHub, Linear, Context7, DeepWiki, Playwright) via the mcporter CLI. Each skill replaces ~10-30 MCP tool definitions with a single on-demand skill invocation, saving context window.

## Skill Structure

Each skill lives in `skills/<name>/SKILL.md` with:
- **YAML frontmatter**: `name`, `description`, `allowed-tools: [Bash]`
- **`<objective>`**: What the skill does and when to use it
- **`<process>`**: Call syntax, tool reference, common examples
- **`<tips>`**: Gotchas, auth requirements, performance notes

## Adding a New MCP Skill

1. Copy the template: `cp skills/_template/SKILL.md.template skills/mcp-<server>/SKILL.md`
2. Run `mcporter list <server> --all-parameters` to discover available tools
3. Write the skill with all four parts: **decision framework** (when to use this vs alternatives), **tool catalog** (compact tables with all tools), **workflows** (2-4 multi-step bash examples), **tips** (critical params, pagination, auth)
4. Test: `mcporter call <server>.some_tool --output json`, then invoke `/mcp-<server>` in a Claude Code session
5. Bump version in `.claude-plugin/plugin.json` and `marketplace.json`, sync to `~/.claude/plugins/marketplaces/ark-ai-mcp/`

See README.md for the full guide and quality checklist.

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

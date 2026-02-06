# ark-ai-mcp

Claude Code plugin providing MCP wrapper skills via [mcporter](https://github.com/steipete/mcporter) CLI. Each skill replaces dozens of MCP tool definitions with a single on-demand invocation, keeping your context window lean.

## Skills

| Skill | Description |
|-------|-------------|
| `/mcp-linear` | Linear issues, projects, teams, comments, documents, cycles |
| `/mcp-github` | GitHub code search, file contents, repo queries (complements `gh` CLI) |
| `/mcp-context7` | Library/framework documentation lookup via Context7 |
| `/mcp-deepwiki` | Public GitHub repo documentation and architecture queries |

## Prerequisites

1. **mcporter** installed globally:
   ```bash
   brew install steipete/tap/mcporter
   ```

2. **~/.mcporter/mcporter.json** configured with MCP server definitions for `linear`, `github`, `context7`, `deepwiki`

3. **Auth setup** per server:
   - **Linear**: OAuth via `mcporter auth linear`
   - **GitHub**: `GITHUB_TOKEN` env var
   - **Context7**: No auth required
   - **DeepWiki**: No auth required

## Installation

```bash
# From GitHub
claude /plugin marketplace add HelloWorldSungin/ark-ai-mcp
claude /plugin install ark-ai-mcp@ark-ai-mcp

# From local submodule
claude /plugin marketplace add ./external/ark-ai-mcp
claude /plugin install ark-ai-mcp@ark-ai-mcp
```

Then enable in `~/.claude/settings.json`:
```json
{
  "enabledPlugins": {
    "ark-ai-mcp@ark-ai-mcp": true
  }
}
```

## Usage

In any Claude Code session, invoke skills by name:

```
/mcp-linear          # Then ask about Linear issues, create/update issues
/mcp-github          # Then search code, get file contents
/mcp-context7        # Then look up library docs
/mcp-deepwiki        # Then query public repo documentation
```

Each skill provides the mcporter call syntax and common examples. Use `--output json` for machine-readable results.

## License

MIT

# ark-ai-mcp

Claude Code plugin providing MCP wrapper skills via [mcporter](https://github.com/steipete/mcporter) CLI. Each skill replaces dozens of MCP tool definitions with a single on-demand invocation, keeping your context window lean.

## Skills

| Skill | Description |
|-------|-------------|
| `/mcp-linear` | Linear issues, projects, teams, comments, documents, cycles |
| `/mcp-github` | GitHub code search, file contents, repo queries (complements `gh` CLI) |
| `/mcp-context7` | Library/framework documentation lookup via Context7 |
| `/mcp-deepwiki` | Public GitHub repo documentation and architecture queries |
| `/mcp-playwright` | Browser automation — navigate, interact, screenshot, scrape web pages |

## Prerequisites

1. **mcporter** installed globally:
   ```bash
   brew install steipete/tap/mcporter
   ```

2. **~/.mcporter/mcporter.json** configured with MCP server definitions for `linear`, `github`, `context7`, `deepwiki`, `playwright`

3. **Auth setup** per server:
   - **Linear**: OAuth via `mcporter auth linear`
   - **GitHub**: `GITHUB_TOKEN` env var
   - **Context7**: No auth required
   - **DeepWiki**: No auth required
   - **Playwright**: No auth required (runs locally via npx)

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
/mcp-playwright      # Then navigate pages, fill forms, take screenshots
```

Each skill provides the mcporter call syntax and common examples. Use `--output json` for machine-readable results.

## Adding a New MCP Server

To make a new MCP server available to mcporter (and then wrap it as a skill):

### 1. Register the server in `~/.mcporter/mcporter.json`

Add an entry under `mcpServers`:

```json
{
  "mcpServers": {
    "my-server": {
      "description": "What this server does.",
      "baseUrl": "https://mcp.example.com/mcp"
    }
  }
}
```

**Common auth patterns:**

```json
// No auth (Context7, DeepWiki)
{ "baseUrl": "https://mcp.example.com/mcp" }

// Bearer token from env var (GitHub)
{ "baseUrl": "https://api.example.com/mcp/", "bearerTokenEnv": "MY_TOKEN" }

// OAuth flow (Linear)
{ "baseUrl": "https://mcp.example.com/mcp", "auth": "oauth" }
```

For OAuth servers, run `mcporter auth <server-name>` after adding the entry.

### 2. Verify the server works

```bash
# List available tools
mcporter list my-server --all-parameters

# Test a call
mcporter call my-server.some_tool --output json
```

### 3. Create the skill

Create `skills/mcp-<server>/SKILL.md` following the existing pattern (see CLAUDE.md for structure). Commit, bump the version in `.claude-plugin/plugin.json`, and update the plugin.

## License

MIT

# ark-ai-mcp

Claude Code plugin providing MCP wrapper skills via [mcporter](https://github.com/steipete/mcporter) CLI. Each skill replaces dozens of MCP tool definitions with a single on-demand invocation, keeping your context window lean.

## Skills

| Skill | Description |
|-------|-------------|
| `/mcp-github` | 40 tools for PRs, issues, repos, code search, releases (complements `gh` CLI) |
| `/mcp-linear` | Linear issues, projects, teams, comments, documents, cycles, milestones, attachments |
| `/mcp-context7` | Library/framework documentation lookup via Context7 |
| `/mcp-deepwiki` | Public GitHub repo documentation and architecture queries |
| `/mcp-playwright` | Headless browser automation — navigate, interact, screenshot, scrape |

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

## Adding a New MCP Skill

A template is available at `skills/_template/SKILL.md.template` — copy it as your starting point.

### Step 1: Register the MCP server in mcporter

Add an entry to `~/.mcporter/mcporter.json` under `mcpServers`:

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

### Step 2: Discover available tools

```bash
mcporter list my-server --all-parameters
```

Save the output — you'll use it to build the tool catalog and understand parameters.

### Step 3: Create the skill file

```bash
cp skills/_template/SKILL.md.template skills/mcp-myserver/SKILL.md
```

Edit the file following the structure below.

### Step 4: Write the skill content

A good skill has four parts, in order of importance:

#### 1. Objective with decision framework
Don't just say what the skill does — explain **when to use it vs alternatives**. Claude needs to decide between `gh` CLI, this skill, Claude-in-Chrome, etc. A comparison table or bullet list works well.

#### 2. Tool catalog as compact tables
Organize tools into logical categories (3-8 categories). Each tool gets one row:

```markdown
| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `tool_name` | `param1`, `param2` | One-line description |
```

For tools with a `method` parameter (like GitHub's `pull_request_read`), list all method values in the description — this is the most common source of missed capabilities.

#### 3. Multi-step workflows with bash examples
Include 2-4 workflows showing how to **chain tools for real tasks** — not just isolated calls. These are the most valuable part of the skill because they teach Claude what a complete task looks like.

Bad: `mcporter call github.get_file_contents ...` (isolated call, no context)
Good: "Review a PR" workflow showing get overview → get diff → check status → submit review

#### 4. Tips section
Focus on:
- Critical parameters that change behavior (method enums, ref formats, query syntax)
- Pagination patterns (`page` + `perPage` and their limits)
- Auth requirements and how to verify they're configured
- Performance gotchas (cold start, token cost, rate limits)

### Step 5: Test the skill

```bash
# Verify mcporter can reach the server
mcporter call my-server.some_tool --output json

# Invoke in Claude Code to verify skill loads
# (start a new session after syncing)
/mcp-myserver
```

### Step 6: Version bump and sync

1. Bump `version` in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`
2. Copy to installed location:
   ```bash
   cp -r skills/ ~/.claude/plugins/marketplaces/ark-ai-mcp/skills/
   cp .claude-plugin/*.json ~/.claude/plugins/marketplaces/ark-ai-mcp/.claude-plugin/
   ```
3. Commit and push

### Quality checklist

Before merging a new skill, verify:

- [ ] **Decision framework**: Objective explains when to use this vs alternatives
- [ ] **Complete tool catalog**: All tools from `mcporter list` are documented
- [ ] **Workflows**: At least 2 multi-step workflows with bash examples
- [ ] **Method enums**: Any `method` parameter has all values listed
- [ ] **Pagination**: List/search tools document `page`/`perPage` params and limits
- [ ] **Auth documented**: How to set up and verify authentication
- [ ] **Tested**: At least one mcporter call succeeds end-to-end
- [ ] **Synced**: Source and installed plugin locations match

## License

MIT

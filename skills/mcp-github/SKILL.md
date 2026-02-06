---
name: mcp-github
description: Call GitHub MCP tools via mcporter CLI. Use for GitHub operations not covered by the gh CLI, such as code search, file content retrieval, or advanced repo queries. Requires GITHUB_TOKEN env var.
allowed-tools: [Bash]
---

<objective>
Call GitHub MCP tools through the mcporter CLI. For most GitHub operations (PRs, issues, releases, checks), prefer the `gh` CLI which is already available and authenticated. Use this skill when you need GitHub MCP-specific capabilities like semantic code search or operations the `gh` CLI doesn't support.
</objective>

<process>

## When to Use This vs `gh` CLI

**Prefer `gh` CLI for:**
- Creating/viewing/merging PRs (`gh pr create`, `gh pr view`)
- Issues (`gh issue list`, `gh issue create`)
- Releases, checks, actions, API calls (`gh api`)
- Most day-to-day GitHub operations

**Use mcporter GitHub MCP for:**
- Semantic code search across repos
- File content retrieval via MCP
- Operations where MCP provides richer structured data

## Prerequisites

Requires `GITHUB_TOKEN` env var (set in ~/.zshrc).

## Call Syntax

```bash
# Colon-delimited
mcporter call github.TOOL_NAME key:value --output json

# Function-call style
mcporter call 'github.TOOL_NAME(key: "value")' --output json
```

## Discovering Available Tools

The GitHub MCP server tools depend on authentication state. To see what's available:

```bash
mcporter list github --all-parameters
```

## Common Patterns

```bash
# Search code
mcporter call 'github.search_code(query: "className in:file language:typescript")' --output json

# Get file contents
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "src/index.ts")' --output json

# List repos
mcporter call 'github.list_repos(owner: "org")' --output json
```

</process>

<tips>
- The `gh` CLI is almost always faster and simpler for standard operations. Use it first.
- If `GITHUB_TOKEN` is not set, mcporter will fail with an auth error. Check with `echo $GITHUB_TOKEN`.
- Use `--output json` for machine-readable results.
- Discover tools: `mcporter list github` (requires valid token).
</tips>

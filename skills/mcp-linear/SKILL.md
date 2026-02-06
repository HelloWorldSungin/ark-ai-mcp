---
name: mcp-linear
description: Call Linear MCP tools via mcporter CLI. Use when you need to interact with Linear issues, projects, teams, comments, or documents. Replaces the Linear MCP plugin to save context window.
allowed-tools: [Bash]
---

<objective>
Call Linear MCP tools through the mcporter CLI instead of loading 33 tool definitions into the context window. This skill covers all Linear operations: issues, projects, teams, comments, documents, cycles, milestones, labels, and users.
</objective>

<process>

## Call Syntax

Two equivalent forms — use whichever is clearer:

```bash
# Colon-delimited (shell-friendly, best for simple args)
mcporter call linear.TOOL_NAME key:value key2:value2 --output json

# Function-call style (better for complex args)
mcporter call 'linear.TOOL_NAME(key: "value", key2: "value2")' --output json
```

Always use `--output json` for machine-readable results you'll parse.

## Issues

```bash
# List issues (assignee "me" = current user)
mcporter call linear.list_issues assignee:me limit:10 --output json

# Search issues
mcporter call linear.list_issues query:"search terms" team:ENG --output json

# Get single issue by ID
mcporter call linear.get_issue id:ENG-123 --output json

# Get issue with relations (blocking/blocked-by/duplicates)
mcporter call linear.get_issue id:ENG-123 includeRelations:true --output json

# Create issue
mcporter call 'linear.create_issue(title: "Bug: login broken", team: "ENG", priority: 2, assignee: "me")' --output json

# Update issue (state, priority, assignee, etc.)
mcporter call 'linear.update_issue(id: "ENG-123", state: "In Progress", priority: 1)' --output json

# Filter by state, label, project, priority
mcporter call linear.list_issues state:started label:bug project:infra priority:2 --output json
```

## Comments

```bash
# List comments on an issue
mcporter call linear.list_comments issueId:ISSUE_UUID --output json

# Create comment (body is markdown)
mcporter call 'linear.create_comment(issueId: "ISSUE_UUID", body: "Comment text here")' --output json
```

## Teams

```bash
mcporter call linear.list_teams --output json
mcporter call linear.get_team query:ENG --output json
```

## Projects

```bash
mcporter call linear.list_projects --output json
mcporter call linear.get_project query:infra --output json
mcporter call linear.get_project query:infra includeMilestones:true --output json
```

## Users

```bash
mcporter call linear.list_users --output json
mcporter call linear.get_user query:me --output json
```

## Documents

```bash
mcporter call linear.list_documents --output json
mcporter call linear.get_document id:DOC_ID --output json
mcporter call 'linear.create_document(title: "My Doc", project: "infra", content: "# Heading\nBody")' --output json
```

## Cycles, Milestones, Labels

```bash
# Cycles
mcporter call linear.list_cycles teamId:TEAM_UUID type:current --output json

# Milestones
mcporter call linear.list_milestones project:infra --output json

# Issue labels
mcporter call linear.list_issue_labels team:ENG --output json

# Issue statuses
mcporter call linear.list_issue_statuses team:ENG --output json
```

## Other Tools

```bash
# Search Linear docs
mcporter call linear.search_documentation query:"keyboard shortcuts" --output json

# Extract images from markdown
mcporter call 'linear.extract_images(markdown: "![img](https://...)")' --output json
```

</process>

<tips>
- Use `--output json` for all calls when you need to parse the result.
- Issue identifiers (like ENG-123) work for `get_issue` but UUIDs are needed for `update_issue`, `list_comments`, etc. Get the UUID from `get_issue` first.
- To discover all available tools or check parameter details: `mcporter list linear --all-parameters`
- Priority values: 0=None, 1=Urgent, 2=High, 3=Normal, 4=Low
- State accepts type names (backlog, unstarted, started, completed, cancelled) or specific state names.
- The `assignee` field accepts "me", a user name, email, or UUID.
- For creating issues with labels, use: `labels:'["Bug","Frontend"]'`
- First call in a session may take ~2s for OAuth token refresh; subsequent calls are faster.
</tips>

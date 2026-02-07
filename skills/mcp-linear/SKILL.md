---
name: mcp-linear
description: Call Linear MCP tools via mcporter CLI. Use when you need to interact with Linear issues, projects, teams, comments, documents, cycles, milestones, attachments, or labels. Replaces the Linear MCP plugin to save context window.
allowed-tools: [Bash]
---

<objective>
Call Linear MCP tools through the mcporter CLI instead of loading 33 tool definitions into the context window. This skill covers all Linear operations: issues, projects, teams, comments, documents, cycles, milestones, labels, attachments, and users.
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

# Search issues by text
mcporter call linear.list_issues query:"search terms" team:ENG --output json

# Get single issue by ID
mcporter call linear.get_issue id:ENG-123 --output json

# Get issue with relations (blocking/blocked-by/duplicates)
mcporter call linear.get_issue id:ENG-123 includeRelations:true --output json

# Create issue
mcporter call 'linear.create_issue(title: "Bug: login broken", team: "ENG", priority: 2, assignee: "me")' --output json

# Create issue with labels
mcporter call 'linear.create_issue(title: "Fix deploy", team: "ENG", labels: "[\"Bug\",\"Infra\"]")' --output json

# Update issue (state, priority, assignee, etc.)
mcporter call 'linear.update_issue(id: "ENG-123", state: "In Progress", priority: 1)' --output json

# Filter by state, label, project, priority, delegate
mcporter call linear.list_issues state:started label:bug project:infra priority:2 --output json

# Filter by date (ISO-8601 durations)
mcporter call 'linear.list_issues(team: "ENG", createdAt: "-P7D")' --output json
mcporter call 'linear.list_issues(team: "ENG", updatedAt: "-P1D")' --output json

# Include archived issues
mcporter call linear.list_issues team:ENG includeArchived:true --output json
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
# List/get projects
mcporter call linear.list_projects --output json
mcporter call linear.get_project query:infra --output json
mcporter call linear.get_project query:infra includeMilestones:true --output json

# Create project
mcporter call 'linear.create_project(name: "Q2 Platform", description: "Platform improvements for Q2", teams: "[\"ENG\"]")' --output json

# Update project
mcporter call 'linear.update_project(id: "PROJECT_UUID", state: "started", description: "Updated scope")' --output json

# List project labels
mcporter call linear.list_project_labels --output json
```

## Milestones

```bash
mcporter call linear.list_milestones project:infra --output json
mcporter call linear.get_milestone id:MILESTONE_UUID --output json
mcporter call 'linear.create_milestone(name: "Beta Launch", project: "infra", targetDate: "2026-03-15")' --output json
mcporter call 'linear.update_milestone(id: "MILESTONE_UUID", name: "Beta Launch v2")' --output json
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
mcporter call 'linear.update_document(id: "DOC_ID", content: "# Updated\nNew content")' --output json
```

## Attachments

```bash
mcporter call linear.get_attachment id:ATTACHMENT_UUID --output json
mcporter call 'linear.create_attachment(issueId: "ISSUE_UUID", url: "https://example.com/file.pdf", title: "Design spec")' --output json
mcporter call linear.delete_attachment id:ATTACHMENT_UUID --output json
```

## Cycles, Labels, Statuses

```bash
# Cycles
mcporter call linear.list_cycles teamId:TEAM_UUID type:current --output json

# Issue labels
mcporter call linear.list_issue_labels team:ENG --output json
mcporter call 'linear.create_issue_label(name: "Technical Debt", team: "ENG", color: "#ff6600")' --output json

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
- **Priority values**: 0=None, 1=Urgent, 2=High, 3=Normal, 4=Low
- **State** accepts type names (backlog, unstarted, started, completed, cancelled) or specific state names.
- The `assignee` field accepts "me", a user name, email, or UUID.
- For creating issues with labels, use: `labels:'["Bug","Frontend"]'`
- **Date filters**: Use ISO-8601 durations — `-P1D` (last day), `-P7D` (last week), `-P30D` (last month), `-P90D` (last quarter).
- First call in a session may take ~2s for OAuth token refresh; subsequent calls are faster.
</tips>

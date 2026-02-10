# Linear MCP Tool Catalog — Extended Operations

Reference for less-common Linear MCP tools. For primary operations (Issues, Comments, Teams, Projects, Users), see the main SKILL.md. Use `mcporter list linear --all-parameters` for the latest.

## Milestones

```bash
mcporter call linear.list_milestones project:infra --output json
mcporter call linear.get_milestone id:MILESTONE_UUID --output json
mcporter call 'linear.create_milestone(name: "Beta Launch", project: "infra", targetDate: "2026-03-15")' --output json
mcporter call 'linear.update_milestone(id: "MILESTONE_UUID", name: "Beta Launch v2")' --output json
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

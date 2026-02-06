---
name: mcp-deepwiki
description: Query public GitHub repos and their documentation via DeepWiki MCP. Use when you need to understand a public repo's architecture, APIs, or docs without cloning it.
allowed-tools: [Bash]
---

<objective>
Query public GitHub repositories and their documentation through the DeepWiki MCP server. Get AI-powered answers about repo architecture, APIs, usage patterns, and more — without needing to clone the repo locally. No authentication required.
</objective>

<process>

## Available Tools

### Ask a Question About a Repo

Best for targeted questions. Returns AI-powered, context-grounded answers:

```bash
mcporter call 'deepwiki.ask_question(repoName: "facebook/react", question: "How does the fiber reconciler work?")' --output json
```

Can query multiple repos (max 10):

```bash
mcporter call 'deepwiki.ask_question(repoName: ["vercel/next.js", "remix-run/remix"], question: "How do these frameworks handle server-side rendering differently?")' --output json
```

### Browse Wiki Structure

See the documentation topics available for a repo:

```bash
mcporter call 'deepwiki.read_wiki_structure(repoName: "facebook/react")' --output json
```

### Read Wiki Contents

Get the full documentation for a repo:

```bash
mcporter call 'deepwiki.read_wiki_contents(repoName: "facebook/react")' --output json
```

## Common Examples

```bash
# Understand a library's architecture
mcporter call 'deepwiki.ask_question(repoName: "steipete/mcporter", question: "How does the config discovery and merging work?")' --output json

# Compare approaches across repos
mcporter call 'deepwiki.ask_question(repoName: ["expressjs/express", "fastify/fastify"], question: "How do these handle middleware differently?")' --output json

# Explore what docs are available
mcporter call 'deepwiki.read_wiki_structure(repoName: "anthropics/claude-code")' --output json
```

</process>

<tips>
- No authentication required — works out of the box.
- Use `ask_question` for specific questions; use `read_wiki_structure` to browse available topics first.
- The `repoName` format is always `owner/repo` (e.g., "facebook/react").
- For multiple repos, pass an array (max 10).
- Use `--output json` for machine-readable results.
- Best for public repos only — private repos won't be accessible.
</tips>

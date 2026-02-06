---
name: mcp-context7
description: Look up library and framework documentation via Context7 MCP. Use when you need up-to-date docs, API references, or code examples for any programming library.
allowed-tools: [Bash]
---

<objective>
Query up-to-date documentation and code examples for any programming library or framework via the Context7 MCP server. No authentication required. Two-step process: resolve the library ID, then query docs.
</objective>

<process>

## Step 1: Resolve Library ID

Find the Context7 library ID for the library you need:

```bash
mcporter call 'context7.resolve-library-id(query: "how to use React hooks", libraryName: "react")' --output json
```

This returns matching libraries ranked by relevance. Pick the `libraryId` from the best match (format: `/org/project`).

## Step 2: Query Documentation

Use the resolved library ID to fetch docs:

```bash
mcporter call 'context7.query-docs(libraryId: "/vercel/next.js", query: "how to set up middleware for authentication")' --output json
```

## Common Examples

```bash
# React hooks docs
mcporter call 'context7.resolve-library-id(query: "React hooks", libraryName: "react")' --output json
mcporter call 'context7.query-docs(libraryId: "/facebook/react", query: "useEffect cleanup patterns")' --output json

# Next.js routing
mcporter call 'context7.resolve-library-id(query: "Next.js app router", libraryName: "next.js")' --output json
mcporter call 'context7.query-docs(libraryId: "/vercel/next.js", query: "app router dynamic routes")' --output json

# Python library
mcporter call 'context7.resolve-library-id(query: "pandas dataframe", libraryName: "pandas")' --output json
mcporter call 'context7.query-docs(libraryId: "/pandas-dev/pandas", query: "merge dataframes on multiple columns")' --output json
```

</process>

<tips>
- Always call `resolve-library-id` first unless you already know the exact library ID (format: `/org/project`).
- Do not call either tool more than 3 times per question. Use the best result you have.
- No authentication required — this server works out of the box.
- The `query` parameter in both tools should describe what you're trying to accomplish, not just a keyword.
- Use `--output json` to parse results programmatically.
</tips>

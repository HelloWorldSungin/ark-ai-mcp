---
name: mcp-github
description: "Call GitHub MCP tools via mcporter CLI \u2014 40 tools for PRs, issues, repos, code search, releases, and more. Use for: 'search code across repos', 'read files from another branch', 'review PR diff', 'cross-repo queries', 'push files to remote'. Requires GITHUB_TOKEN env var."
allowed-tools: [Bash]
---

<objective>
Call GitHub MCP tools through the mcporter CLI. 40 structured tools for pull requests, issues, repositories, code search, releases, users, and teams. Use this for cross-repo operations, structured data retrieval, and capabilities the local `gh` CLI doesn't offer (semantic code search, file content by ref, batch operations). Requires `GITHUB_TOKEN` env var (set in ~/.zshrc).
</objective>

<process>

## When to Use This vs `gh` CLI

| Task | Use | Why |
|------|-----|-----|
| Local repo PRs (create, merge, view) | `gh` | Already authenticated for current repo |
| Read files from **any** repo/branch/tag | **mcporter** | `get_file_contents` with ref param |
| Search code across GitHub | **mcporter** | `search_code` with language/org/path filters |
| Get PR diff as structured data | **mcporter** | `pull_request_read(method: "get_diff")` |
| Cross-repo issue/PR queries | **mcporter** | No need to clone or switch repos |
| Create/push files to remote repos | **mcporter** | `push_files` for multi-file commits |
| Review PRs programmatically | **mcporter** | `pull_request_review_write` with line comments |
| Repo/org-level search | **mcporter** | `search_repositories`, `search_users` |
| Quick local checks (status, log, diff) | `gh` / `git` | Faster, no network call |

## Call Syntax

```bash
# Colon-delimited (shell-friendly, best for simple args)
mcporter call github.TOOL_NAME key:value key2:value2 --output json

# Function-call style (better for complex/quoted args)
mcporter call 'github.TOOL_NAME(key: "value", key2: "value2")' --output json
```

Always use `--output json` for machine-readable results.

## Tool Catalog Summary (40 tools)

| Category | Count | Key Tools |
|----------|-------|-----------|
| Pull Requests | 10 | `pull_request_read`, `create_pull_request`, `merge_pull_request`, `list_pull_requests`, `pull_request_review_write` |
| Repo Exploration | 8 | `get_file_contents`, `search_code`, `list_commits`, `get_commit`, `search_repositories` |
| Issues | 8 | `issue_read`, `issue_write`, `add_issue_comment`, `list_issues`, `search_issues` |
| Repo Management | 6 | `create_branch`, `push_files`, `create_or_update_file`, `create_repository` |
| Releases | 3 | `list_releases`, `get_latest_release`, `get_release_by_tag` |
| Users & Teams | 4 | `get_me`, `search_users`, `get_teams`, `get_team_members` |
| AI | 1 | `assign_copilot_to_issue` |

**Full parameter details**: Read `references/tool-catalog.md` or run `mcporter list github --all-parameters`

## Workflows

### 1. Review a Pull Request

```bash
# Get PR overview (title, body, state, author, merge status)
mcporter call 'github.pull_request_read(owner: "org", repo: "repo", pullNumber: 42, method: "get")' --output json

# Get the full diff
mcporter call 'github.pull_request_read(owner: "org", repo: "repo", pullNumber: 42, method: "get_diff")' --output json

# List changed files
mcporter call 'github.pull_request_read(owner: "org", repo: "repo", pullNumber: 42, method: "list_files")' --output json

# Get CI/check status
mcporter call 'github.pull_request_read(owner: "org", repo: "repo", pullNumber: 42, method: "get_status")' --output json

# Read existing review comments
mcporter call 'github.pull_request_read(owner: "org", repo: "repo", pullNumber: 42, method: "get_review_comments")' --output json

# Submit a review with inline comments
mcporter call 'github.pull_request_review_write(owner: "org", repo: "repo", pullNumber: 42, method: "create", event: "COMMENT", body: "Looks good overall", comments: "[{\"path\": \"src/main.py\", \"line\": 15, \"body\": \"Consider error handling here\"}]")' --output json
```

### 2. Explore a Remote Repository

```bash
# Browse repo root (returns directory listing)
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "")' --output json

# Read a specific file
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "src/config.py")' --output json

# Read file at a specific branch/tag/PR
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "README.md", ref: "refs/heads/develop")' --output json
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "README.md", ref: "refs/tags/v1.0.0")' --output json
mcporter call 'github.get_file_contents(owner: "org", repo: "repo", path: "README.md", ref: "refs/pull/42/head")' --output json

# View recent commits
mcporter call 'github.list_commits(owner: "org", repo: "repo", perPage: 10)' --output json

# Get a specific commit's diff
mcporter call 'github.get_commit(owner: "org", repo: "repo", sha: "abc1234")' --output json

# List branches and tags
mcporter call 'github.list_branches(owner: "org", repo: "repo")' --output json
mcporter call 'github.list_tags(owner: "org", repo: "repo")' --output json
```

### 3. Search Code Across GitHub

```bash
# Search by content
mcporter call 'github.search_code(query: "content:useEffect language:typescript org:vercel")' --output json

# Search in a specific repo and path
mcporter call 'github.search_code(query: "content:DATABASE_URL repo:HelloWorldSungin/ArkNode-AI path:src/config")' --output json

# Search by filename
mcporter call 'github.search_code(query: "filename:CLAUDE.md org:anthropics")' --output json

# Combine multiple filters
mcporter call 'github.search_code(query: "content:FastAPI language:python org:tiangolo path:docs")' --output json
```

**Query syntax for `search_code`:** `content:X language:Y org:Z repo:A/B path:C filename:D extension:E`

### 4. Manage Issues

```bash
# Create an issue
mcporter call 'github.issue_write(owner: "org", repo: "repo", method: "create", title: "Bug: login fails on mobile", body: "Steps to reproduce...", labels: "[\"bug\", \"mobile\"]", assignees: "[\"username\"]")' --output json

# Get issue with comments
mcporter call 'github.issue_read(owner: "org", repo: "repo", issue_number: 99, method: "get")' --output json
mcporter call 'github.issue_read(owner: "org", repo: "repo", issue_number: 99, method: "get_comments")' --output json

# Search issues across repos
mcporter call 'github.search_issues(query: "is:open label:bug org:HelloWorldSungin")' --output json

# Close an issue
mcporter call 'github.issue_write(owner: "org", repo: "repo", method: "update", issue_number: 99, state: "closed", state_reason: "completed")' --output json

# Add a comment
mcporter call 'github.add_issue_comment(owner: "org", repo: "repo", issue_number: 99, body: "Fixed in PR #100")' --output json
```

</process>

<tips>
- **`pull_request_read` method param is critical** — it's one tool with 7 methods: `get`, `get_diff`, `get_comments`, `get_reviews`, `get_status`, `list_files`, `get_review_comments`. Always specify the method.
- **`get_file_contents` ref param** supports `refs/heads/BRANCH`, `refs/tags/TAG`, `refs/pull/PR/head` for reading files at any ref. Omit ref for default branch.
- **`search_code` query syntax**: `content:X language:Y org:Z repo:A/B path:C filename:D extension:E`. These are GitHub qualifiers, not free text.
- **Pagination**: list/search tools support `page` (1-indexed) + `perPage` (max 100). Default perPage varies by tool.
- **`issue_read` also has methods**: `get`, `get_comments`, `get_sub_issues`, `get_labels`. Specify the one you need.
- **Multi-file commits**: Use `push_files` with a `files` array to commit multiple files atomically — avoids multiple `create_or_update_file` calls.
- **`list_pull_requests` sort options**: `created`, `updated`, `popularity`, `long-running`. The `long-running` sort is useful for finding stale PRs.
- If `GITHUB_TOKEN` is not set, mcporter will fail with an auth error. Check with `echo $GITHUB_TOKEN`.
- Discover all tools and parameters: `mcporter list github --all-parameters`
</tips>

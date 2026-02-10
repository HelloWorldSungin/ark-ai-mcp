# GitHub MCP Tool Catalog

Full parameter reference for all 40 GitHub MCP tools. Use `mcporter list github --all-parameters` for the latest.

## Pull Request Management (10 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `pull_request_read` | `owner`, `repo`, `pullNumber`, `method` | Read PR details — method: `get` / `get_diff` / `get_comments` / `get_reviews` / `get_status` / `list_files` / `get_review_comments` |
| `create_pull_request` | `owner`, `repo`, `title`, `head`, `base`, `body`, `draft` | Create a new PR |
| `update_pull_request` | `owner`, `repo`, `pullNumber`, `title`, `body`, `state`, `base` | Update PR title/body/state/base |
| `merge_pull_request` | `owner`, `repo`, `pullNumber`, `commit_title`, `merge_method` | Merge PR (merge/squash/rebase) |
| `list_pull_requests` | `owner`, `repo`, `state`, `head`, `base`, `sort`, `direction`, `page`, `perPage` | List PRs with filters |
| `search_pull_requests` | `query`, `page`, `perPage` | Search PRs across GitHub with query syntax |
| `pull_request_review_write` | `owner`, `repo`, `pullNumber`, `method` | Write reviews — method: `create` / `submit` / `dismiss` |
| `add_comment_to_pending_review` | `owner`, `repo`, `pullNumber`, `body`, `path`, `line` | Add inline comment to pending review |
| `request_copilot_review` | `owner`, `repo`, `pullNumber` | Request Copilot to review a PR |
| `update_pull_request_branch` | `owner`, `repo`, `pullNumber`, `expected_head_sha` | Update PR branch (merge base into head) |

## Repository Exploration (8 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `get_file_contents` | `owner`, `repo`, `path`, `ref` | Read file/directory contents at any ref |
| `list_commits` | `owner`, `repo`, `sha`, `path`, `author`, `since`, `until`, `page`, `perPage` | List commits with filters |
| `get_commit` | `owner`, `repo`, `sha` | Get single commit details + diff |
| `list_branches` | `owner`, `repo`, `page`, `perPage` | List all branches |
| `list_tags` | `owner`, `repo`, `page`, `perPage` | List all tags |
| `get_tag` | `owner`, `repo`, `tag` | Get tag details |
| `search_code` | `query`, `page`, `perPage` | Search code across GitHub |
| `search_repositories` | `query`, `page`, `perPage` | Search repositories |

## Issue Management (8 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `issue_read` | `owner`, `repo`, `issue_number`, `method` | Read issue — method: `get` / `get_comments` / `get_sub_issues` / `get_labels` |
| `issue_write` | `owner`, `repo`, `method`, `title`, `body`, `assignees`, `labels`, `state` | Create/update issues — method: `create` / `update` |
| `add_issue_comment` | `owner`, `repo`, `issue_number`, `body` | Add a comment to an issue |
| `list_issues` | `owner`, `repo`, `state`, `labels`, `sort`, `direction`, `since`, `page`, `perPage` | List issues with filters |
| `search_issues` | `query`, `page`, `perPage` | Search issues across GitHub |
| `list_issue_types` | `owner`, `repo` | List available issue types for a repo |
| `sub_issue_write` | `owner`, `repo`, `issue_number`, `method` | Manage sub-issues — method: `add` / `remove` / `reprioritize` |
| `get_label` | `owner`, `repo`, `name` | Get label details |

## Repository Management (6 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `create_branch` | `owner`, `repo`, `ref`, `sha` | Create a branch from a SHA |
| `create_or_update_file` | `owner`, `repo`, `path`, `content`, `message`, `branch`, `sha` | Create or update a single file |
| `delete_file` | `owner`, `repo`, `path`, `message`, `branch`, `sha` | Delete a file |
| `push_files` | `owner`, `repo`, `branch`, `files`, `message` | Push multiple files in one commit |
| `create_repository` | `name`, `description`, `private`, `autoInit` | Create a new repository |
| `fork_repository` | `owner`, `repo`, `organization` | Fork a repository |

## Releases (3 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `list_releases` | `owner`, `repo`, `page`, `perPage` | List all releases |
| `get_latest_release` | `owner`, `repo` | Get the latest release |
| `get_release_by_tag` | `owner`, `repo`, `tag` | Get release by tag name |

## Users & Teams (4 tools)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `get_me` | — | Get authenticated user info |
| `search_users` | `query`, `page`, `perPage` | Search GitHub users |
| `get_teams` | `org` | List teams in an organization |
| `get_team_members` | `org`, `team_slug` | List members of a team |

## AI (1 tool)

| Tool | Key Parameters | Description |
|------|---------------|-------------|
| `assign_copilot_to_issue` | `owner`, `repo`, `issue_number` | Assign Copilot to work on an issue |

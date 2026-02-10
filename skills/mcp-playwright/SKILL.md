---
name: mcp-playwright
description: "Browser automation via Playwright MCP \u2014 navigate, interact, screenshot, and inspect web pages headlessly. Use for: 'scrape this page', 'fill out a web form', 'take a screenshot of a URL', 'check if a site is up'. Use when no browser session exists."
allowed-tools: [Bash]
---

<objective>
Automate browser interactions through the Playwright MCP server via mcporter CLI. Navigate to URLs, click elements, fill forms, take screenshots, and read page content — all headless by default. Use this when you need to interact with web pages, verify UI, scrape content, or test web flows.

**Playwright MCP vs Claude-in-Chrome**: Use Playwright MCP for headless/CI/server automation where no browser window exists. Use Claude-in-Chrome MCP tools for interactive browser sessions where the user has Chrome open with the extension.
</objective>

<process>

## Call Syntax

```bash
# Colon-delimited
mcporter call playwright.TOOL_NAME key:value --output json

# Function-call style
mcporter call 'playwright.TOOL_NAME(key: "value")' --output json
```

## Tool Reference

### Navigation
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_navigate` | `url` | Navigate to a URL |
| `browser_navigate_back` | — | Go back to previous page |
| `browser_close` | — | Close the current page |

### Observation
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_snapshot` | — | Accessibility snapshot of the page (primary observation tool) |
| `browser_take_screenshot` | — | Capture a PNG screenshot |
| `browser_console_messages` | — | Get browser console output |
| `browser_network_requests` | — | List network requests made by the page |

### Interaction
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_click` | `element`, `ref` | Click an element (use ref from snapshot) |
| `browser_type` | `element`, `ref`, `text` | Type text into an input |
| `browser_select_option` | `element`, `ref`, `values` | Select dropdown options |
| `browser_hover` | `element`, `ref` | Hover over an element |
| `browser_drag` | `startElement`, `startRef`, `endElement`, `endRef` | Drag and drop |
| `browser_press_key` | `key` | Press a keyboard key |

### Forms
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_fill_form` | `fields` | Fill multiple form fields at once |

### JavaScript
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_evaluate` | `expression` | Run a JavaScript expression in the page |
| `browser_run_code` | `code` | Run a Playwright code snippet |

### Tabs, Waiting & Dialogs
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_tabs` | — | List open tabs |
| `browser_wait_for` | `text` or `timeout` | Wait for text to appear or a timeout |
| `browser_handle_dialog` | `accept` | Accept or dismiss a dialog |
| `browser_file_upload` | `paths` | Upload files to a file input |

### Setup
| Tool | Parameters | Description |
|------|-----------|-------------|
| `browser_install` | — | Install the browser binary if missing |

## Common Workflows

### Scrape Page Content

```bash
# Navigate and get accessibility tree
mcporter call 'playwright.browser_navigate(url: "https://example.com")' --output json
mcporter call playwright.browser_snapshot --output json

# Extract specific data via JavaScript
mcporter call 'playwright.browser_evaluate(expression: "document.querySelector(\"h1\").textContent")' --output json
```

### Fill and Submit a Form

```bash
# Navigate and inspect
mcporter call 'playwright.browser_navigate(url: "https://example.com/login")' --output json
mcporter call playwright.browser_snapshot --output json

# Use refs from snapshot to interact
mcporter call 'playwright.browser_type(element: "Username field", ref: "e3", text: "user@example.com")' --output json
mcporter call 'playwright.browser_type(element: "Password field", ref: "e5", text: "password")' --output json
mcporter call 'playwright.browser_click(element: "Submit button", ref: "e7")' --output json
```

### Take a Screenshot

```bash
mcporter call 'playwright.browser_navigate(url: "https://example.com")' --output json
mcporter call playwright.browser_take_screenshot --output json
```

### Debug a Page

```bash
mcporter call 'playwright.browser_navigate(url: "https://example.com")' --output json
mcporter call playwright.browser_console_messages --output json
mcporter call playwright.browser_network_requests --output json
```

</process>

<tips>
- Discover all tools and parameters: `mcporter list playwright --all-parameters`
- **Headless by default**: The server runs `--headless` since Claude operates in a terminal. No display server needed.
- **First call is slower**: npx downloads `@playwright/mcp` on the first invocation. Subsequent calls reuse the cached package.
- **Use `browser_snapshot` as your primary observation tool** — it returns an accessibility tree with element refs that you use for `browser_click`, `browser_type`, etc.
- **Element refs**: After a `browser_snapshot`, use the `ref` values (e.g., `"e3"`) to target elements in interaction tools.
- **Empty accessibility tree?** Use `browser_wait_for` with a timeout or text selector to wait for dynamic content to load, then retry `browser_snapshot`.
- No authentication required — this server runs locally via npx.
- **Install browser if needed**: If Playwright browsers aren't installed, call `browser_install` first.
- Extended capabilities: Pass `--caps` flags when configuring the server for vision mode, PDF generation, or testing features.
- Use `--output json` for machine-readable results.
</tips>

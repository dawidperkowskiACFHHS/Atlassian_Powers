---
name: "atlassian"
displayName: "Atlassian (Jira & Confluence)"
description: "Interact with Atlassian Jira and Confluence from Kiro - search issues, create tickets, manage pages, browse spaces, and streamline your development workflow. Supports both Cloud and Server/Data Center deployments."
keywords: ["atlassian", "jira", "confluence", "issues", "tickets", "wiki", "pages", "project management", "agile", "sprint", "backlog", "kanban", "scrum", "JQL", "CQL"]
author: "Dawid Perkowski"
---

# Onboarding

## Prerequisites

1. An Atlassian account with access to Jira and/or Confluence
2. Python and `uvx` installed (for running the MCP server)
   - Verify with: `uvx --version`
   - If not installed, see: https://docs.astral.sh/uv/getting-started/installation/

## Get Your API Credentials

### Atlassian Cloud

1. Go to https://id.atlassian.com/manage-profile/security/api-tokens
2. Click **Create API token**, give it a label, and copy the token
3. Note your Atlassian email and site URL (e.g. `https://your-company.atlassian.net`)

### Server / Data Center

1. Go to your profile → **Personal Access Tokens**
2. Create a new token and copy it
3. Note your server URL (e.g. `https://jira.your-company.com`)

## Configure Environment

Set these environment variables in the MCP configuration:

### For Cloud:
- `JIRA_URL` — Your Jira Cloud URL (e.g. `https://your-company.atlassian.net`)
- `JIRA_USERNAME` — Your Atlassian email
- `JIRA_API_TOKEN` — Your API token
- `CONFLUENCE_URL` — Your Confluence Cloud URL (e.g. `https://your-company.atlassian.net/wiki`)
- `CONFLUENCE_USERNAME` — Your Atlassian email
- `CONFLUENCE_API_TOKEN` — Your API token (same as Jira)

### For Server / Data Center:
- `JIRA_URL` — Your Jira server URL
- `JIRA_PERSONAL_TOKEN` — Your personal access token
- `CONFLUENCE_URL` — Your Confluence server URL
- `CONFLUENCE_PERSONAL_TOKEN` — Your personal access token

> You can configure just Jira, just Confluence, or both — the server adapts based on which environment variables are set.

# Overview

The Atlassian Power connects Kiro to your Jira and Confluence instances via the `mcp-atlassian` MCP server. You can search and manage Jira issues, browse and edit Confluence pages, and bridge workflows between both tools — all from within Kiro.

**Key capabilities:**

### Jira
- **Search**: Find issues using JQL (Jira Query Language)
- **Issues**: Get, create, update, and transition issues
- **Comments**: Add comments to issues
- **Projects**: List projects and issue types
- **Users**: Look up users by name or email
- **Links**: View remote issue links (e.g. linked Confluence pages)

### Confluence
- **Search**: Find content using CQL (Confluence Query Language)
- **Pages**: Get, create, and update pages
- **Spaces**: List and browse spaces
- **Comments**: View and add footer and inline comments
- **Navigation**: Browse page ancestors and descendants

## Available Steering Files

- **steering** — Detailed query patterns, JQL/CQL examples, workflow guides, and troubleshooting

## Available MCP Servers

### mcp-atlassian
**Package:** `mcp-atlassian` (PyPI)
**Connection:** Local stdio via uvx

**Jira Tools:**

1. **jira_search** — Search issues using JQL
   - Required: `jql` (string) — JQL query string
   - Optional: `limit` (number, default: 50) — Max results to return
   - Returns: List of matching issues with key fields

2. **jira_get_issue** — Get full details of a Jira issue
   - Required: `issue_key` (string) — Issue key (e.g. `PROJ-123`)
   - Returns: Complete issue details including description, status, assignee, comments

3. **jira_create_issue** — Create a new Jira issue
   - Required: `project_key` (string) — Project key (e.g. `PROJ`)
   - Required: `summary` (string) — Issue title
   - Required: `issue_type` (string) — Type (e.g. `Bug`, `Story`, `Task`)
   - Optional: `description` (string) — Issue description (markdown)
   - Optional: `assignee` (string) — Assignee account ID
   - Optional: `priority` (string) — Priority name
   - Optional: `labels` (array) — Labels to apply
   - Returns: Created issue key and URL

4. **jira_update_issue** — Update an existing issue
   - Required: `issue_key` (string) — Issue key
   - Optional: `summary`, `description`, `assignee`, `priority`, `labels`
   - Returns: Confirmation of update

5. **jira_transition_issue** — Change issue status
   - Required: `issue_key` (string) — Issue key
   - Required: `transition` (string) — Target status name (e.g. `Done`, `In Progress`)
   - Returns: Confirmation of transition

6. **jira_add_comment** — Add a comment to an issue
   - Required: `issue_key` (string) — Issue key
   - Required: `body` (string) — Comment text (markdown)
   - Returns: Confirmation with comment ID

7. **jira_list_projects** — List accessible Jira projects
   - Returns: List of projects with keys and names

8. **jira_get_project** — Get project details
   - Required: `project_key` (string) — Project key
   - Returns: Project details including issue types and leads

**Confluence Tools:**

9. **confluence_search** — Search content using CQL
   - Required: `cql` (string) — CQL query string
   - Optional: `limit` (number, default: 25) — Max results
   - Returns: Matching pages, blog posts, and other content

10. **confluence_get_page** — Get a Confluence page
    - Required: `page_id` (string) — Page ID
    - Returns: Page content in markdown format with metadata

11. **confluence_create_page** — Create a new page
    - Required: `space_key` (string) — Space key
    - Required: `title` (string) — Page title
    - Required: `body` (string) — Page content (markdown)
    - Optional: `parent_id` (string) — Parent page ID
    - Returns: Created page ID and URL

12. **confluence_update_page** — Update an existing page
    - Required: `page_id` (string) — Page ID
    - Required: `title` (string) — Page title
    - Required: `body` (string) — Updated content (markdown)
    - Returns: Confirmation of update

13. **confluence_list_spaces** — List Confluence spaces
    - Optional: `limit` (number) — Max spaces to return
    - Returns: List of spaces with keys and names

14. **confluence_get_comments** — Get comments on a page
    - Required: `page_id` (string) — Page ID
    - Returns: Footer and inline comments

15. **confluence_add_comment** — Add a comment to a page
    - Required: `page_id` (string) — Page ID
    - Required: `body` (string) — Comment text (markdown)
    - Returns: Confirmation with comment ID

## Configuration

**Setup in mcp.json (Cloud):**
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "your.email@company.com",
        "JIRA_API_TOKEN": "your_api_token",
        "CONFLUENCE_URL": "https://your-company.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "your.email@company.com",
        "CONFLUENCE_API_TOKEN": "your_api_token"
      }
    }
  }
}
```

**Setup in mcp.json (Server / Data Center):**
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://jira.your-company.com",
        "JIRA_PERSONAL_TOKEN": "your_personal_access_token",
        "CONFLUENCE_URL": "https://confluence.your-company.com",
        "CONFLUENCE_PERSONAL_TOKEN": "your_personal_access_token"
      }
    }
  }
}
```

## Best Practices

### ✅ Do:
- Use JQL for precise Jira searches (e.g. `project = PROJ AND status = "In Progress" AND assignee = currentUser()`)
- Use CQL for Confluence searches (e.g. `space = DEV AND type = page AND text ~ "deployment"`)
- Use issue keys (e.g. `PROJ-123`), not issue IDs when possible
- Use space keys (e.g. `DEV`) for Confluence operations
- Paginate results when working with large datasets
- Check available transitions before transitioning an issue

### ❌ Don't:
- Hardcode issue keys — use `jira_search` to discover them
- Create duplicate issues without searching first
- Update pages without reading the current content first
- Assume transition names — they vary by project workflow

## Tips

1. **Discover projects first** — Use `jira_list_projects` to find project keys
2. **Use JQL effectively** — `assignee = currentUser() AND status != Done ORDER BY updated DESC` finds your active work
3. **Link workflows** — Search Confluence for specs, then create Jira tickets from requirements
4. **Use CQL for Confluence** — `lastModified > now("-7d") AND space = TEAM` finds recent team updates
5. **Check transitions** — Issue workflows vary; query available transitions before changing status
6. **Markdown content** — Both Jira and Confluence accept markdown for descriptions and page content

---

**Package:** `mcp-atlassian` (by sooperset — https://github.com/sooperset/mcp-atlassian)
**Author:** Dawid Perkowski
**License:** MIT
**Connection:** Local stdio via uvx

> **Disclaimer:** This is a community-built Kiro Power created by Dawid Perkowski and is not officially affiliated with or endorsed by Atlassian Corporation. Jira and Confluence are registered trademarks of Atlassian Corporation.

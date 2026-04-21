# Kiro Power: Atlassian (Jira & Confluence)

> **Community-built Kiro Power by Dawid Perkowski**
> This is an unofficial community project and is not affiliated with or endorsed by Atlassian Corporation.

A Kiro Power that connects your Kiro agent to Atlassian Jira and Confluence via the [mcp-atlassian](https://github.com/sooperset/mcp-atlassian) MCP server. Supports both Cloud and Server/Data Center deployments.

## Capabilities

### Jira
- **Search issues** — Find issues using JQL (Jira Query Language)
- **Get issue details** — View full issue information including comments and links
- **Create issues** — Create bugs, stories, tasks, and epics
- **Update issues** — Modify summary, description, assignee, priority, labels
- **Transition issues** — Move issues through workflow states (To Do → In Progress → Done)
- **Add comments** — Comment on issues with markdown formatting
- **List projects** — Browse accessible projects and issue types

### Confluence
- **Search content** — Find pages and blog posts using CQL (Confluence Query Language)
- **Get pages** — Read page content in markdown format
- **Create pages** — Create new pages in any space
- **Update pages** — Edit existing page content
- **List spaces** — Browse available Confluence spaces
- **Comments** — View and add page comments

## Quick Start

### 1. Get Your API Credentials

**Cloud:**
- Go to https://id.atlassian.com/manage-profile/security/api-tokens → Create API token
- Note your email and site URL (e.g. `https://your-company.atlassian.net`)

**Server / Data Center:**
- Go to your profile → Personal Access Tokens → Create token
- Note your server URL

### 2. Install the Power

Install from the Kiro Powers panel or point to this repository as a custom power source.

### 3. Set Environment Variables

The power needs these environment variables configured in the MCP server:

#### Cloud

| Variable | Required | Description |
|---|---|---|
| `JIRA_URL` | Yes* | Jira Cloud URL (e.g. `https://your-company.atlassian.net`) |
| `JIRA_USERNAME` | Yes* | Your Atlassian email |
| `JIRA_API_TOKEN` | Yes* | API token from Atlassian |
| `CONFLUENCE_URL` | Yes* | Confluence URL (e.g. `https://your-company.atlassian.net/wiki`) |
| `CONFLUENCE_USERNAME` | Yes* | Your Atlassian email |
| `CONFLUENCE_API_TOKEN` | Yes* | API token (same as Jira) |

*Configure Jira variables, Confluence variables, or both depending on what you need.

#### Server / Data Center

| Variable | Required | Description |
|---|---|---|
| `JIRA_URL` | Yes* | Your Jira server URL |
| `JIRA_PERSONAL_TOKEN` | Yes* | Personal access token |
| `CONFLUENCE_URL` | Yes* | Your Confluence server URL |
| `CONFLUENCE_PERSONAL_TOKEN` | Yes* | Personal access token |

## Structure

```
├── POWER.md              # Power metadata, documentation, and tool reference
├── mcp.json              # MCP server configuration
├── steering/
│   └── steering.md       # JQL/CQL patterns, workflow guides, and troubleshooting
└── README.md             # This file
```

## Compatibility

| Product | Deployment | Support |
|---|---|---|
| Jira | Cloud | Fully supported |
| Jira | Server/Data Center | Supported (v8.14+) |
| Confluence | Cloud | Fully supported |
| Confluence | Server/Data Center | Supported (v6.0+) |

## Author

Created by Dawid Perkowski.

This is a community-built power and is not officially affiliated with or endorsed by Atlassian Corporation. Jira and Confluence are registered trademarks of Atlassian Corporation.

## License

MIT

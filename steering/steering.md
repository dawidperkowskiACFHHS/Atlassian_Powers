# Atlassian Power — Steering Guide

## JQL (Jira Query Language) Patterns

### Common Queries
- Find my open issues: `assignee = currentUser() AND status != Done ORDER BY updated DESC`
- Find bugs in a project: `project = PROJ AND issuetype = Bug AND status != Closed`
- Sprint backlog: `project = PROJ AND sprint in openSprints()`
- Recently updated: `project = PROJ AND updated >= -7d ORDER BY updated DESC`
- Unassigned issues: `project = PROJ AND assignee is EMPTY`
- High priority: `project = PROJ AND priority in (Highest, High) AND status != Done`
- Created this week: `project = PROJ AND created >= startOfWeek()`
- Issues with label: `project = PROJ AND labels = "frontend"`
- Epics: `project = PROJ AND issuetype = Epic`
- Subtasks of an issue: `parent = PROJ-100`

### JQL Tips
- Use `currentUser()` for the logged-in user
- Use relative dates: `-7d`, `-1w`, `-1M`, `startOfDay()`, `startOfWeek()`, `endOfMonth()`
- Combine with `AND`, `OR`, `NOT`
- Order with `ORDER BY field ASC/DESC`
- Text search: `summary ~ "search term"` or `text ~ "search term"`

## CQL (Confluence Query Language) Patterns

### Common Queries
- Search by text: `text ~ "deployment guide"`
- Pages in a space: `space = DEV AND type = page`
- Recently modified: `lastModified > now("-7d")`
- By author: `creator = "user@company.com"`
- By label: `label = "architecture"`
- Blog posts: `type = blogpost AND space = TEAM`
- Pages with title: `title = "Release Notes"`
- Combined: `space = DEV AND type = page AND label = "api" AND lastModified > now("-30d")`

### CQL Tips
- Use `~` for contains/fuzzy matching
- Use `=` for exact matching
- Combine with `AND`, `OR`, `NOT`
- Filter by type: `page`, `blogpost`, `comment`, `attachment`
- Use `now("-Xd")` for relative dates

## Workflow Examples

### Workflow 1: Bug Triage
```
1. Search for new bugs: jira_search with JQL "project = PROJ AND issuetype = Bug AND status = 'To Do' ORDER BY created DESC"
2. Get details on each: jira_get_issue for the bug key
3. Update priority if needed: jira_update_issue
4. Assign to developer: jira_update_issue with assignee
5. Transition to In Progress: jira_transition_issue
```

### Workflow 2: Sprint Planning
```
1. List backlog items: jira_search with JQL "project = PROJ AND sprint is EMPTY AND status = 'To Do' ORDER BY priority DESC"
2. Review each item: jira_get_issue for details
3. Check related Confluence docs: confluence_search with CQL for specs
4. Add comments with estimates: jira_add_comment
```

### Workflow 3: Documentation from Code
```
1. Create a Confluence page: confluence_create_page with space key, title, and markdown body
2. Create a Jira task to track: jira_create_issue linking to the page
3. Add a comment on the Jira issue with the Confluence page URL
```

### Workflow 4: Release Notes
```
1. Search for completed issues: jira_search with JQL "project = PROJ AND status = Done AND fixVersion = '2.1.0'"
2. Get details for each: jira_get_issue
3. Create a Confluence page: confluence_create_page with formatted release notes
4. Update the Jira issues with a link to the release notes page
```

### Workflow 5: Incident Investigation
```
1. Search for related issues: jira_search with JQL "text ~ 'login timeout' AND project = PROJ"
2. Check Confluence runbooks: confluence_search with CQL "label = 'runbook' AND text ~ 'login'"
3. Create an incident ticket: jira_create_issue with type Bug and high priority
4. Add investigation notes: jira_add_comment
```

## Troubleshooting

### Error: "401 Unauthorized"
**Cause:** Invalid or expired API token / credentials.
**Solution:**
- Cloud: Verify your email and API token at https://id.atlassian.com/manage-profile/security/api-tokens
- Server/DC: Regenerate your Personal Access Token

### Error: "403 Forbidden"
**Cause:** Your account lacks permission for the requested operation.
**Solution:** Check your project/space permissions with your Atlassian admin.

### Error: "404 Not Found"
**Cause:** Invalid issue key, page ID, or URL.
**Solution:** Verify the resource exists and your URL is correct. For Confluence Cloud, ensure the URL ends with `/wiki`.

### Error: "Field 'X' cannot be set"
**Cause:** Trying to set a field that doesn't exist or isn't editable in the issue's current state.
**Solution:** Check the project's field configuration and issue type scheme.

### MCP Server Won't Start
**Cause:** `uvx` can't find or run the package.
**Solution:**
1. Verify uvx is installed: `uvx --version`
2. Test manually: `uvx mcp-atlassian --help`
3. Check Python version: Python 3.10+ required

### Connection Issues with Server/Data Center
**Cause:** Self-signed certificates or network restrictions.
**Solution:**
- Set `REQUESTS_CA_BUNDLE` env var to your CA bundle path
- Or set `ATLASSIAN_SSL_VERIFY=false` (not recommended for production)

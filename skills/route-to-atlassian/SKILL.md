# route-to-atlassian

## Description
Routes operational and cross-functional findings to Atlassian tools. Creates Jira issues for actionable blockers and Confluence investigation pages for findings requiring deeper analysis.

## Trigger
This skill is invoked automatically when synthesized findings include LOBs mapped to Atlassian (typically Operations), or when orchestrated by the `ops-review` command.

## Instructions

### 1. Accept Input
Receive the synthesized LOB findings relevant to Atlassian (primarily Operations, but also cross-LOB blockers).

### 2. Load Templates
Read the Jira issue and Confluence page templates from `references/tool-templates.json`.

### 3. Create Jira Issues

For each finding with severity `critical` or `warning`:

Use the Atlassian MCP server to create a Jira issue:
- **Summary**: `[Strategy] {lob}: {finding_title}` (from template)
- **Description**: Structured with finding details, dashboard link, external context, and remediation steps (from template)
- **Issue Type**: Task
- **Priority**: Map severity → Jira priority (critical → Highest, warning → High, informational → Medium)
- **Labels**: `strategy-insights`, `automated`
- **Assignee**: From LOB mapping if configured, otherwise unassigned

### 4. Create Confluence Investigation Pages

For critical findings that require root cause analysis:

Use the Atlassian MCP server to create a Confluence page:
- **Title**: `Strategy Investigation: {finding_title} — {date}` (from template)
- **Space**: Operations or relevant team space
- **Body**: Include finding summary, dashboard link, external context, and placeholder sections for root cause analysis and action items
- Link the Confluence page to the corresponding Jira issue

### 5. Link Artifacts

- Add Confluence page link to the Jira issue description
- If multiple findings relate to the same operational area, link the Jira issues to each other

### 6. Return Results

```json
{
  "routed_at": "ISO-8601 timestamp",
  "jira_issues_created": 0,
  "confluence_pages_created": 0,
  "issue_keys": [],
  "page_urls": [],
  "errors": []
}
```

## Error Handling
- If Atlassian MCP is unavailable, log all intended actions for manual execution
- If Jira creation succeeds but Confluence fails, still return the Jira issue and note the Confluence error
- Continue processing remaining findings if individual items fail

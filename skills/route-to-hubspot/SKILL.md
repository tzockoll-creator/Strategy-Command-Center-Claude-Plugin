# route-to-hubspot

## Description
Routes sales and marketing intelligence from Strategy Cloud findings to HubSpot. Creates tasks, updates deal records with risk scores, and logs notes with dashboard-sourced insights.

## Trigger
This skill is invoked automatically when synthesized findings include LOBs mapped to HubSpot (typically Sales, Marketing, Customer Success), or when orchestrated by the `sales-review` command.

## Instructions

### 1. Accept Input
Receive the synthesized LOB findings relevant to HubSpot (Sales, Marketing, Customer Success).

### 2. Load Templates
Read the HubSpot templates from `references/tool-templates.json`.

### 3. Create HubSpot Tasks

For each finding with severity `critical` or `warning`:

Use the HubSpot MCP server to create a task:
- **Title**: `[{lob}] Strategy Alert: {summary}` (from template)
- **Body**: Include severity, dashboard name with link, metric details, external context, and recommended action (from template)
- **Priority**: Map severity → HubSpot priority (critical → HIGH, warning → MEDIUM, informational → LOW)
- **Due date**: Critical → today/tomorrow, Warning → end of week

### 4. Update Deal Records (if applicable)

For Sales findings that relate to specific pipeline or deal metrics:

- Query HubSpot for relevant deals matching the affected segment/region
- Add a note to matching deals with the Strategy intelligence finding
- Update deal risk score if the finding indicates pipeline risk
- Use the deal update template from `references/tool-templates.json`

### 5. Log Activity

For informational findings:
- Log as a note on the relevant HubSpot contact, company, or deal record
- Lower priority — no task creation required

### 6. Return Results

```json
{
  "routed_at": "ISO-8601 timestamp",
  "hubspot_tasks_created": 0,
  "deals_updated": 0,
  "notes_logged": 0,
  "task_ids": [],
  "errors": []
}
```

## Error Handling
- If HubSpot MCP is unavailable, log all intended actions and return them for manual execution
- If a specific task/deal update fails, continue with remaining items and report errors

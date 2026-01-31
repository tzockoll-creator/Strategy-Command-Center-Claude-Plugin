# route-to-slack

## Description
Posts findings summaries and critical alerts to Slack channels using the Slack MCP server. Handles both summary messages (end-of-review) and real-time critical alerts.

## Trigger
This skill is invoked automatically when findings need to be communicated via Slack, or when orchestrated by any review command. Also triggered by the post-skill hook for critical findings.

## Instructions

### 1. Accept Input
Receive synthesized findings and the review type context (full review, quick scan, ops review, etc.).

### 2. Load Templates
Read the Slack message templates from `references/tool-templates.json`.

### 3. Post Summary Message

After a review completes, post a summary message using the Slack MCP server:

- **Channel**: Primary alert channel (e.g., `#strategy-alerts` or configured channel)
- **Message format** (from template):
  ```
  📊 Strategy Command Center — {review_type} Complete

  Dashboards Analyzed: {dashboard_count}
  Findings: {critical_count} Critical | {warning_count} Warning | {info_count} Informational

  🔴 Critical Items:
  • {lob}: {finding_title} — {metric} ({variance})
    Dashboard: {dashboard_link}
  ...

  🟡 Warnings:
  • {lob}: {finding_title} — {metric} ({variance})
  ...

  Actions Taken:
  • Created {asana_count} Asana tasks
  • Updated {hubspot_count} HubSpot records
  • Created {jira_count} Jira issues
  • Generated {confluence_count} Confluence pages

  Run /strategy-command-center:strategy-review for full details
  ```

### 4. Post Individual Critical Alerts

For each `critical` severity finding, also post a dedicated alert:

- **Channel**: Same alert channel (or a dedicated `#critical-alerts` channel if configured)
- **Message format** (from template):
  ```
  🚨 Critical Strategy Alert

  {lob}: {finding_title}
  Dashboard: {dashboard_link}
  Metric: {metric} → {current_value} ({variance})

  {details}

  External Context: {web_context}

  Recommended Action: {recommendation}
  ```

### 5. Post to LOB-Specific Channels (optional)

If LOB-specific Slack channels are configured, also post relevant findings to those channels:
- `#sales-insights` for Sales findings
- `#ops-alerts` for Operations findings
- etc.

### 6. Return Results

```json
{
  "routed_at": "ISO-8601 timestamp",
  "summary_messages_sent": 0,
  "critical_alerts_sent": 0,
  "lob_messages_sent": 0,
  "channel_ids": [],
  "errors": []
}
```

## Error Handling
- If Slack MCP is unavailable, store the messages and note them for retry
- If posting to a specific channel fails (e.g., channel not found), try the fallback channel and report the error

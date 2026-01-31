# route-to-asana

## Description
Creates Asana tasks for LOB-scoped findings with severity-based prioritization, dashboard links, and recommended actions. Serves as the universal task-creation channel used by all LOBs.

## Trigger
This skill is invoked automatically when synthesized findings need task tracking, or when orchestrated by any review command.

## Instructions

### 1. Accept Input
Receive the synthesized LOB findings. All LOBs route to Asana — it is the common task management layer.

### 2. Load Templates and Mappings
- Read the Asana task template from `references/tool-templates.json`
- Read LOB-to-project mappings from `references/lob-mappings.json`

### 3. Create Asana Tasks

For each LOB that has findings, create one Asana task per LOB (consolidating findings):

Use the Asana MCP server to create a task:
- **Title**: `[{lob}] Strategy Insights: {summary} — {date}` (from template)
- **Project**: Use the LOB's configured `asana_project` from mappings, or default "Strategy Insights" project
- **Assignee**: Use the LOB's configured `assignee` from mappings, or leave unassigned
- **Description**: Include:
  - Executive summary (2-3 sentences)
  - Key findings organized by severity (Critical 🔴, Warning 🟡, Informational 🟢)
  - Dashboard links for each finding
  - External context sources
  - Generation timestamp
- **Due Date**:
  - If LOB has critical findings → today or next business day
  - If LOB has warning findings (no critical) → end of week
  - If LOB has only informational → no due date
- **Priority**: Based on highest severity finding in the LOB (critical → high, warning → medium, informational → low)
- **Tags**: `strategy-insights`, `automated`, `{lob-name}`

### 4. Handle Individual Critical Items

For each individual finding with `critical` severity, also create a separate focused task:
- **Title**: `🔴 [CRITICAL] {lob}: {metric} — {finding_title}`
- **Description**: Detailed single-finding view with full context and specific remediation
- **Due Date**: Today or next business day
- **Priority**: High

### 5. Return Results

```json
{
  "routed_at": "ISO-8601 timestamp",
  "lob_tasks_created": 0,
  "critical_tasks_created": 0,
  "total_tasks": 0,
  "task_ids": [],
  "errors": []
}
```

## Error Handling
- If Asana MCP is unavailable, format the tasks as a document and present to the user for manual creation
- If a specific task creation fails, continue with remaining tasks and report errors

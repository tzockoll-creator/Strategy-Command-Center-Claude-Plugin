# /strategy-command-center:ops-review

## Description
Operations-focused review: analyzes Strategy Cloud dashboards, enriches findings, and routes operational findings to Jira, Confluence, and Asana.

## User-Invoked
This command is invoked by the user with `/strategy-command-center:ops-review`.

## Instructions

When the user runs this command:

1. **Inform the user** that an operations-focused review is starting.

2. **Analyze**: Invoke `analyze-dashboards` to scan all Strategy Cloud dashboards.

3. **Enrich**: Invoke `enrich-findings` to add external context (focused on operational metrics: supply chain, fulfillment, inventory, quality).

4. **Synthesize**: Invoke `synthesize-by-lob` to group findings. Focus the output on Operations LOB, but include cross-LOB findings that impact operations.

5. **Route to Atlassian**: Invoke `route-to-atlassian` to:
   - Create Jira issues for operational blockers and action items
   - Create Confluence investigation pages for critical findings requiring root cause analysis

6. **Route to Asana**: Invoke `route-to-asana` to create tasks for the Operations team.

7. **Route to Slack**: Invoke `route-to-slack` to post operations findings summary and critical alerts.

8. **Display results** to the user:
   - Operations-focused findings summary
   - Jira issues created (with keys/links)
   - Confluence pages created (with links)
   - Asana tasks created
   - Slack messages posted

## Arguments
None.

## Example Usage
```
> /strategy-command-center:ops-review
```

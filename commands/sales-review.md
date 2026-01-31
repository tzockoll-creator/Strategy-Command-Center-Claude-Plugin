# /strategy-command-center:sales-review

## Description
Sales-focused review: analyzes Strategy Cloud dashboards, enriches findings, and routes sales intelligence to HubSpot and Asana.

## User-Invoked
This command is invoked by the user with `/strategy-command-center:sales-review`.

## Instructions

When the user runs this command:

1. **Inform the user** that a sales-focused review is starting.

2. **Analyze**: Invoke `analyze-dashboards` to scan all Strategy Cloud dashboards.

3. **Enrich**: Invoke `enrich-findings` to add external context (focused on sales metrics: revenue trends, competitor activity, pipeline health, market conditions).

4. **Synthesize**: Invoke `synthesize-by-lob` to group findings. Focus the output on Sales and Marketing LOBs, but include Customer Success and cross-LOB findings that impact revenue.

5. **Route to HubSpot**: Invoke `route-to-hubspot` to:
   - Create tasks for sales team action items
   - Update deal records with risk scores from Strategy intelligence
   - Log notes on relevant contacts/companies

6. **Route to Asana**: Invoke `route-to-asana` to create tasks for Sales and Marketing teams.

7. **Route to Slack**: Invoke `route-to-slack` to post sales findings summary and critical revenue alerts.

8. **Display results** to the user:
   - Sales and Marketing findings summary
   - HubSpot tasks created and deals updated
   - Asana tasks created
   - Slack messages posted

## Arguments
None.

## Example Usage
```
> /strategy-command-center:sales-review
```

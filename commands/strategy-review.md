# /strategy-command-center:strategy-review

## Description
Runs the full 5-step Strategy Analyst workflow: Discover → Analyze → Enrich → Synthesize → Deliver. Analyzes all Strategy Cloud dashboards, enriches findings with web research, groups by LOB, and routes to all configured enterprise tools.

## User-Invoked
This command is invoked by the user with `/strategy-command-center:strategy-review`.

## Instructions

When the user runs this command:

1. **Inform the user** that a full Strategy review is starting across all dashboards and enterprise tools.

2. **Step 1 — Discover + Analyze**: Invoke the `analyze-dashboards` skill to browse the Strategy Cloud library and analyze every dashboard for anomalies, threshold breaches, and trends.

3. **Step 2 — Enrich**: Invoke the `enrich-findings` skill to add external context via web search for each finding.

4. **Step 3 — Synthesize**: Invoke the `synthesize-by-lob` skill to group findings by Line of Business and prioritize.

5. **Step 4 — Deliver to all tools**: Based on the LOB mappings, invoke the appropriate routing skills:
   - `route-to-asana` — for all LOBs (universal task tracking)
   - `route-to-hubspot` — for Sales, Marketing, Customer Success findings
   - `route-to-atlassian` — for Operations findings (Jira issues + Confluence pages)
   - `route-to-slack` — post summary and critical alerts
   - `generate-visuals` — create Canva presentation and Figma annotations if executive findings exist

6. **Step 5 — Report**: Invoke `generate-executive-summary` to produce the formatted briefing.

7. **Display the executive summary** to the user, followed by a summary of all actions taken across enterprise tools.

## Arguments
None. This command performs a full review every time.

## Example Usage
```
> /strategy-command-center:strategy-review
```

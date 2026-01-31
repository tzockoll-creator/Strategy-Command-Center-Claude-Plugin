# /strategy-command-center:executive-briefing

## Description
Executive-focused review: analyzes Strategy Cloud dashboards, enriches findings, and produces executive deliverables via Slack, Canva presentations, and Figma annotations.

## User-Invoked
This command is invoked by the user with `/strategy-command-center:executive-briefing`.

## Instructions

When the user runs this command:

1. **Inform the user** that an executive briefing is being prepared.

2. **Analyze**: Invoke `analyze-dashboards` to scan all Strategy Cloud dashboards.

3. **Enrich**: Invoke `enrich-findings` to add external context (focused on cross-LOB themes, company-level KPIs, strategic initiatives, and competitive landscape).

4. **Synthesize**: Invoke `synthesize-by-lob` to group findings across all LOBs. Emphasize cross-LOB themes and executive-level patterns.

5. **Generate Executive Summary**: Invoke `generate-executive-summary` to produce the formatted briefing document.

6. **Generate Visuals**: Invoke `generate-visuals` to:
   - Create a Canva presentation with title, executive summary, LOB findings, and recommendations slides
   - Add Figma annotations to relevant design files if applicable

7. **Route to Slack**: Invoke `route-to-slack` to post the executive summary and critical alerts to leadership channels.

8. **Display results** to the user:
   - Full executive summary report
   - Canva presentation link
   - Figma annotations added (if any)
   - Slack messages posted to executive channels

## Arguments
None.

## Example Usage
```
> /strategy-command-center:executive-briefing
```

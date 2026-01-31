# generate-executive-summary

## Description
Produces a formatted executive summary report from synthesized findings. Combines dashboard analysis results, external context, and LOB-grouped insights into a comprehensive briefing document.

## Trigger
This skill is invoked automatically after synthesis completes, or when orchestrated by the `strategy-review` or `executive-briefing` commands.

## Instructions

### 1. Accept Input
Receive the full synthesized output from `synthesize-by-lob`, including enriched findings grouped by LOB.

### 2. Generate the Executive Summary Report

Produce the report in this format:

```
═══════════════════════════════════════════════════════════
        STRATEGY INTELLIGENCE BRIEFING
═══════════════════════════════════════════════════════════

Generated:        {timestamp}
Dashboards:       {analyzed_count} analyzed / {total_count} inventoried ({coverage}%)

───────────────────────────────────────────────────────────
                 EXECUTIVE OVERVIEW
───────────────────────────────────────────────────────────

Total Findings:   {total}
  🔴 Critical:    {critical_count}
  🟡 Warning:     {warning_count}
  🟢 Informational: {info_count}

Cross-LOB Themes:
{For each cross-LOB theme:}
  • {theme_description}

───────────────────────────────────────────────────────────
              CRITICAL ITEMS (Immediate Action)
───────────────────────────────────────────────────────────

{For each critical finding:}
  🔴 [{lob}] {finding_title}
     Metric:      {metric} → {current_value} ({variance}%)
     Dashboard:   {dashboard_name} ({dashboard_url})
     Context:     {web_context}
     Action:      {recommendation}

───────────────────────────────────────────────────────────
              FINDINGS BY LINE OF BUSINESS
───────────────────────────────────────────────────────────

{For each LOB with findings:}
▸ {lob_name} — {finding_count} findings ({critical_count} critical, {warning_count} warning)
  {executive_summary for this LOB}

  {For each finding in LOB:}
  [{severity_icon}] {finding_title}
     {metric}: {current_value} (vs {comparison_value}, {variance}%)
     Dashboard: {dashboard_name}
     Context: {web_context}

{For LOBs with no findings:}
▸ {lob_name} — No significant findings ✓

───────────────────────────────────────────────────────────
              ACTIONS TAKEN
───────────────────────────────────────────────────────────

  Tool        | Items Created | Details
  ------------|---------------|---------------------------
  Asana       | {count}       | {task_summary}
  HubSpot     | {count}       | {hubspot_summary}
  Jira        | {count}       | {jira_summary}
  Confluence  | {count}       | {confluence_summary}
  Slack       | {count}       | {slack_summary}
  Canva       | {count}       | {canva_summary}
  Figma       | {count}       | {figma_summary}

───────────────────────────────────────────────────────────
              RECOMMENDED PRIORITIES
───────────────────────────────────────────────────────────

  # | LOB        | Finding                    | Severity | Action
  --|------------|----------------------------|----------|---------------------------
  {Sorted by severity then variance:}
  1 | {lob}      | {finding_title}            | 🔴       | {recommendation}
  2 | {lob}      | {finding_title}            | 🟡       | {recommendation}
  ...

═══════════════════════════════════════════════════════════
                  END OF BRIEFING
═══════════════════════════════════════════════════════════
```

### 3. Formatting Rules
- Critical findings always appear first in every section
- Within same severity, sort by variance magnitude (highest first)
- Include dashboard URLs as clickable links where the output format supports it
- Keep executive summaries concise: 2-3 sentences per LOB
- The Actions Taken section should reflect what was actually routed (may be empty if routing hasn't occurred yet)

## Output Format
Return the formatted report as plain text/markdown, suitable for terminal display, Slack posting, or document export.

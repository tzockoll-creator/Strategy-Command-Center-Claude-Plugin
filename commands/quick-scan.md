# /strategy-command-center:quick-scan

## Description
Runs a fast anomaly scan across Strategy Cloud dashboards and returns a summary table of findings. No enrichment, no tool routing — just the raw analysis results.

## User-Invoked
This command is invoked by the user with `/strategy-command-center:quick-scan`.

## Instructions

When the user runs this command:

1. **Inform the user** that a quick scan is starting (analysis only, no routing).

2. **Analyze**: Invoke the `analyze-dashboards` skill to scan all Strategy Cloud dashboards.

3. **Synthesize**: Invoke the `synthesize-by-lob` skill to group findings by LOB (skip enrichment for speed).

4. **Display a concise summary table**:

```
Strategy Quick Scan — {timestamp}
Dashboards Analyzed: {count} ({coverage}%)

{critical_count} Critical | {warning_count} Warning | {info_count} Informational

Severity | LOB          | Dashboard              | Metric               | Value    | Variance
---------|--------------|------------------------|----------------------|----------|----------
🔴       | Sales        | Southwest Revenue      | Monthly Revenue      | $1.2M    | -18%
🔴       | Operations   | Fulfillment Tracker    | On-Time Delivery     | 82%      | -12%
🟡       | Marketing    | Lead Pipeline          | MQL Volume           | 340      | -15%
🟡       | Finance      | Budget vs Actual       | OpEx                 | $4.1M    | +22%
🟢       | Sales        | Pipeline Coverage      | Coverage Ratio       | 3.2x     | +8%
...
```

5. If critical findings exist, suggest:
   - `/strategy-command-center:strategy-review` for full analysis with enrichment and tool routing
   - `/strategy-command-center:ops-review` or `/strategy-command-center:sales-review` for targeted action

## Arguments
None.

## Example Usage
```
> /strategy-command-center:quick-scan
```

# synthesize-by-lob

## Description
Groups and prioritizes enriched findings by Line of Business (LOB) using the mappings defined in `references/lob-mappings.json`. Produces a structured output organized by LOB with severity-ranked findings.

## Trigger
This skill is invoked automatically after `enrich-findings` completes, or when orchestrated by commands.

## Instructions

### 1. Accept Input
Receive the enriched findings array from `enrich-findings`.

### 2. Load LOB Mappings
Read `references/lob-mappings.json` to get LOB definitions, metric mappings, and dashboard keywords.

### 3. Assign Findings to LOBs

For each finding, determine the LOB assignment using the priority order from the mappings:

1. **Dashboard folder location** — Match folder path against LOB keywords (e.g., `Sales/` → Sales)
2. **Dashboard name** — Match dashboard name against LOB `dashboard_keywords`
3. **Metric type** — Match the finding's metric against LOB `metrics` arrays
4. **Fallback** — If no match, assign to "Executive"

A finding may be relevant to multiple LOBs. In that case, assign to the primary LOB and note secondary LOBs.

### 4. Prioritize Within Each LOB

Within each LOB group, sort findings by:
1. Severity: Critical → Warning → Informational
2. Variance magnitude (highest first)
3. Recency of the data

### 5. Generate LOB Summaries

For each LOB with findings, produce:

```json
{
  "lob": "Sales",
  "finding_count": 0,
  "critical_count": 0,
  "warning_count": 0,
  "informational_count": 0,
  "executive_summary": "2-3 sentence summary of the LOB's key findings",
  "findings": [ ... ],
  "recommended_tools": ["hubspot", "asana"]
}
```

The `recommended_tools` field comes from the LOB mapping's `primary_tools` and determines which routing skills to invoke next.

### 6. Return Synthesized Output

```json
{
  "synthesized_at": "ISO-8601 timestamp",
  "total_findings": 0,
  "lobs_with_findings": 0,
  "lobs_clean": 0,
  "by_lob": [
    {
      "lob": "Sales",
      "finding_count": 2,
      "critical_count": 1,
      "warning_count": 1,
      "informational_count": 0,
      "executive_summary": "...",
      "findings": [ ... ],
      "recommended_tools": ["hubspot", "asana"]
    },
    ...
  ],
  "cross_lob_themes": [
    "Theme or pattern that spans multiple LOBs, if any"
  ]
}
```

### 7. Identify Cross-LOB Themes

After grouping, scan for patterns that appear across multiple LOBs:
- Same external factor affecting multiple areas (e.g., economic downturn hitting Sales and Finance)
- Correlated metrics across LOBs (e.g., Marketing lead drop correlating with Sales pipeline decline)
- Note these in the `cross_lob_themes` array for executive-level context

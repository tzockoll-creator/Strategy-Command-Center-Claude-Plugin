# enrich-findings

## Description
Takes raw dashboard findings and enriches them with external context via web search. Transforms data-only insights into contextualized intelligence by adding competitor dynamics, market trends, and macro factors.

## Trigger
This skill is invoked automatically after `analyze-dashboards` completes, or when orchestrated by commands.

## Instructions

### 1. Accept Input
Receive the findings array from `analyze-dashboards`.

### 2. Determine Search Strategy

For each finding, select the appropriate web search category based on the metric type:

**Revenue / Sales Anomalies:**
- Industry sales trends for the relevant sector
- Competitor earnings, product launches, or pricing changes
- Regional economic conditions affecting the market
- Product category market analysis

**Cost / Margin Changes:**
- Commodity and input price trends
- Supply chain disruptions or news
- Regional labor cost changes
- Inflation impact reports

**Customer Metrics (NPS, churn, CSAT):**
- Competitor customer reviews and sentiment
- Industry customer satisfaction benchmarks
- Consumer behavior trend reports

**Operational Metrics (fulfillment, inventory, quality):**
- Operational benchmarks for the industry
- Supply chain disruption reports
- Industry best practice publications

**Marketing Metrics (leads, conversion, campaigns):**
- Digital marketing trend reports
- Competitor campaign activity
- Channel performance benchmarks

### 3. Execute Web Searches

For each finding (prioritizing critical and warning severity):

- Construct 2-3 targeted search queries based on the metric and finding type
- Prioritize recent results (past 30 days)
- Cross-reference at least 2 sources for credibility
- Extract the most relevant context in 2-3 sentences

### 4. Enrich Each Finding

Add the following fields to each finding:

```json
{
  "web_context": "2-3 sentence summary of external factors explaining the finding",
  "external_sources": [
    {
      "title": "Source article/report title",
      "url": "https://...",
      "relevance": "How this source relates to the finding"
    }
  ],
  "market_factors": ["factor1", "factor2"],
  "competitive_dynamics": "Brief note on competitor activity if relevant"
}
```

### 5. Return Enriched Findings

Return the same findings array with web context appended to each finding.

```json
{
  "enriched_at": "ISO-8601 timestamp",
  "findings_enriched": 0,
  "findings_skipped": 0,
  "total_searches_performed": 0,
  "findings": [ ... ]
}
```

## Best Practices
- Use specific, targeted search queries (not broad terms)
- Prioritize enriching critical and warning findings; informational findings can have lighter enrichment
- Note source credibility (major publications > blog posts)
- If no relevant external context is found, set `web_context` to "No significant external factors identified" rather than forcing irrelevant context

## Error Handling
- If web search is unavailable, return findings with `web_context: "Web search unavailable — data-only insight"` and continue
- Never block the workflow due to search failures

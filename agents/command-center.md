# command-center

## Description
Autonomous multi-agent orchestrator that performs end-to-end business intelligence evaluation across Strategy Cloud, routing insights to 7 enterprise applications based on LOB context and finding severity.

## Agent Configuration
- **Name**: command-center
- **Type**: autonomous
- **Description**: Coordinates iterative Strategy queries, dashboard analysis, web enrichment, LOB synthesis, and enterprise tool routing in a single autonomous workflow.

## Instructions

This agent operates autonomously through three phases, each requiring different data granularity from Strategy.

---

### Phase 1: Intent Parsing

Parse the user's request to determine:
- **Scope**: Full review, specific LOBs, or specific metrics
- **Urgency**: Routine review vs time-sensitive investigation
- **Target tools**: Which enterprise tools to route to (default: all configured)
- **Focus areas**: Any specific dashboards, metrics, or time periods mentioned

If the request is ambiguous, default to a full review across all LOBs and all tools.

---

### Phase 2: Intelligence Collection

#### 2A — Sales Intelligence (Strategy → HubSpot)

Query Strategy MCP for **deal-level granularity**:
- Pipeline health metrics (coverage ratio, velocity, stage conversion)
- Revenue by segment/region with period-over-period variance
- Win/loss analysis with deal-level detail
- Quota attainment by rep/team

Invoke `analyze-dashboards` on sales-related dashboards.
Invoke `enrich-findings` for sales context (competitor pricing, market shifts).
Invoke `route-to-hubspot` with deal-level findings.

#### 2B — Operations Intelligence (Strategy → Atlassian/Asana)

Query Strategy MCP for **actionable blocker granularity**:
- Fulfillment and delivery bottlenecks with root cause indicators
- Inventory alerts with SKU-level detail
- Quality metrics with defect categorization
- Capacity utilization with constraint identification

Invoke `analyze-dashboards` on operations dashboards.
Invoke `enrich-findings` for operations context (supply chain news, benchmarks).
Invoke `route-to-atlassian` for Jira issues and Confluence investigation pages.
Invoke `route-to-asana` for operations task tracking.

#### 2C — Executive Intelligence (Strategy Analyst → Slack/Canva/Figma)

Query Strategy MCP for **cross-LOB patterns**:
- Company-level KPI rollups
- Strategic initiative progress
- Cross-functional dependencies and correlations
- Board-level metrics

Invoke `analyze-dashboards` on executive and cross-functional dashboards.
Invoke `enrich-findings` for macro context (market trends, competitive landscape).
Invoke `synthesize-by-lob` across all findings from phases 2A-2C.

---

### Phase 3: Delivery

1. **Generate executive summary**: Invoke `generate-executive-summary` with all synthesized findings.

2. **Route to Slack**: Invoke `route-to-slack` with:
   - Executive summary to leadership channel
   - Critical alerts to alert channel
   - LOB-specific summaries to team channels

3. **Generate visuals**: Invoke `generate-visuals` to create:
   - Canva executive briefing presentation
   - Figma annotations on relevant designs

4. **Produce execution summary**:
   ```
   ═══════════════════════════════════════
   COMMAND CENTER — Execution Summary
   ═══════════════════════════════════════

   Sources Consulted:
   • Strategy Cloud: {dashboard_count} dashboards
   • Web Search: {search_count} sources

   Actions Taken:
   • HubSpot: {hubspot_tasks} tasks, {deals_updated} deals updated
   • Jira: {jira_issues} issues created
   • Confluence: {confluence_pages} pages created
   • Asana: {asana_tasks} tasks created
   • Slack: {slack_messages} messages posted
   • Canva: {canva_presentations} presentations generated
   • Figma: {figma_comments} annotations added

   Critical Findings: {critical_count}
   Artifacts: {artifact_links}
   ═══════════════════════════════════════
   ```

---

### Iterative Querying

Each downstream tool needs different data from Strategy. The agent must query Strategy multiple times:
- **HubSpot** needs deal-level, rep-level granularity
- **Jira/Confluence** need specific blockers with enough detail for investigation
- **Slack/Canva/Figma** need summarized cross-LOB patterns for executive consumption

Do not attempt to satisfy all tools with a single Strategy query.

---

## Error Handling

- If Strategy MCP is unavailable: abort and inform the user (Strategy is the primary data source)
- If a specific enterprise tool MCP is unavailable: skip that tool, continue with others, note in execution summary
- If web search is unavailable: proceed with data-only insights, note enrichment was skipped
- Always complete maximum possible scope even when individual components fail
- Present a clear execution summary showing what succeeded and what failed

## Safety Measures
- Use environment-based credential handling (MCP servers manage credentials)
- Respect existing tool permissions — do not escalate access
- Remove PII before creating shareable artifacts (Canva, Figma, Slack)
- Log all actions taken for audit trail

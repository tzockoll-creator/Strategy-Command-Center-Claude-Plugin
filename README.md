# Strategy Command Center

A Claude Code plugin that orchestrates business intelligence analysis across Strategy Cloud (MicroStrategy) and routes actionable insights to 7 enterprise applications.

**Single request → Strategy analysis → Intelligent routing → Enterprise-wide action**

## What It Does

1. **Discover** — Browses the Strategy Cloud library and inventories all dashboards
2. **Analyze** — Opens each dashboard and detects anomalies, threshold breaches, and trends
3. **Enrich** — Conducts web searches for external context (competitor news, market trends, macro factors)
4. **Synthesize** — Groups findings by Line of Business (Sales, Marketing, Finance, Operations, HR, Customer Success, Executive)
5. **Deliver** — Routes findings to the right enterprise tools based on LOB context and severity

## Enterprise Tool Routing

| Tool | What Gets Routed |
|------|-----------------|
| **HubSpot** | Sales intelligence — deal risk scores, pipeline updates, task creation |
| **Jira** | Operational blockers — issues with severity and remediation steps |
| **Confluence** | Investigation pages for critical findings requiring root cause analysis |
| **Asana** | LOB-scoped tasks with severity, dashboard links, and due dates |
| **Slack** | Summary messages, critical alerts, LOB-specific channel posts |
| **Canva** | Executive briefing presentations with findings and recommendations |
| **Figma** | Design annotations linked to Strategy findings |

## Commands

| Command | Purpose |
|---------|---------|
| `/strategy-command-center:strategy-review` | Full end-to-end review across all dashboards and tools |
| `/strategy-command-center:quick-scan` | Fast anomaly scan — summary table only, no tool routing |
| `/strategy-command-center:sales-review` | Sales-focused — Strategy to HubSpot and Asana |
| `/strategy-command-center:ops-review` | Operations-focused — Strategy to Jira, Confluence, and Asana |
| `/strategy-command-center:executive-briefing` | Executive-focused — Strategy to Slack, Canva, and Figma |

## Autonomous Agent

The **command-center** agent runs the full orchestration autonomously:
- Parses user intent (scope, LOBs, urgency, target tools)
- Queries Strategy iteratively with different granularity per downstream tool
- Runs dashboard analysis and web enrichment
- Routes to all appropriate enterprise tools
- Produces an execution summary with artifact links

## Anomaly Detection

Detection patterns are configured in `references/finding-patterns.json`:

- **Spikes**: >20% value increase vs prior period
- **Drops**: >15% value decrease vs prior period
- **Outliers**: Data points beyond 2 standard deviations
- **Trends**: 3+ consecutive periods moving in the same direction
- **Threshold breaches**: Red/yellow status indicators, target misses >10%

Findings are classified as **Critical** (immediate action), **Warning** (near-term attention), or **Informational** (awareness only).

## Plugin Structure

```
strategy-command-center/
├── .claude-plugin/plugin.json          # Plugin manifest
├── .mcp.json                           # MCP server configurations
├── skills/                             # 9 model-invoked skills
│   ├── analyze-dashboards/             # Dashboard scanning and anomaly detection
│   ├── enrich-findings/                # Web search enrichment
│   ├── synthesize-by-lob/              # LOB grouping and prioritization
│   ├── route-to-hubspot/               # HubSpot task/deal routing
│   ├── route-to-atlassian/             # Jira issue and Confluence page creation
│   ├── route-to-asana/                 # Asana task creation by LOB
│   ├── route-to-slack/                 # Slack alerts and summaries
│   ├── generate-visuals/               # Canva presentations and Figma annotations
│   └── generate-executive-summary/     # Formatted executive briefing report
├── commands/                           # 5 user-invoked commands
├── agents/                             # Autonomous command-center agent
├── hooks/                              # Auto-alert on critical findings
└── references/                         # Detection patterns, LOB mappings, tool templates
```

## Prerequisites

- Claude Code with plugin support
- Strategy Cloud (MicroStrategy) access via MCP
- MCP servers configured for target enterprise tools:
  - Strategy, Slack, Asana, HubSpot, Atlassian, Canva, Figma

## Setup

1. Clone this repository:
   ```
   git clone https://github.com/tzockoll-creator/Strategy-Command-Center.git
   ```

2. Load the plugin:
   ```
   claude --plugin-dir ./Strategy-Command-Center
   ```

3. Configure LOB mappings in `references/lob-mappings.json`:
   - Set `assignee` and `asana_project` for each Line of Business
   - Adjust `dashboard_keywords` and `metrics` to match your Strategy Cloud structure

4. Run a quick scan to verify:
   ```
   /strategy-command-center:quick-scan
   ```

## Query Guidelines

- **Use natural language, not SQL.** Both Strategy AI agents and the MCP USL accept natural language queries. Write questions the way you would ask a colleague (e.g., "What were total sales last quarter by region?").
- **Only use tables in the playground schema.** All queries should target the playground schema exclusively. Do not reference tables outside of it.
- **Dashboard viewing defaults.** For dashboard analysis only, use `demo.microstrategy.com` as the default environment and the **Office Royale Sales** dashboard unless otherwise specified. For all other analysis (data queries, reporting, etc.), use the Strategy USL and Strategy Agent MCPs directly.

## Customization

- **Anomaly thresholds**: Edit `references/finding-patterns.json` to adjust spike/drop percentages and trend period lengths
- **LOB definitions**: Edit `references/lob-mappings.json` to add/modify Lines of Business and their tool routing
- **Output templates**: Edit `references/tool-templates.json` to customize how findings appear in each enterprise tool

## License

MIT

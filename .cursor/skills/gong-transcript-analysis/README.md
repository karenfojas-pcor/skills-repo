# Gong Transcript Analysis

Analyze Gong call transcripts stored in Snowflake for any Procore product or feature. The skill interviews you to scope the research, queries matching transcripts, and produces a structured UX analysis report with customer quotes and design rationales.

## How It Works

The skill runs a 4-phase conversational workflow:

### Phase 1: Interview

The agent walks you through 5 questions to define the scope:

1. **Product / feature focus** — What are you researching? (e.g. "Timesheets", "Drawing Management", "RFIs")
2. **Search terms** — The agent proposes primary terms (unique phrases like `"dataset editor"`) and secondary terms (generic words like `"pages"` paired with qualifiers like `"data grid"`). You confirm, add, or remove.
3. **Time range** — Last 30 days, 90 days, 6 months, 12 months, all data, or a custom range.
4. **Analysis focus** — Choose one or more: usability pain points, feature requests, competitive mentions, adoption blockers, sentiment, or a custom focus.
5. **Confirmation** — The agent summarizes the scope and asks for approval before querying.

### Phase 2: Query Snowflake

- Runs SQL against `PROCORE_IT.GONG_DATA_CLOUD_PREP` to find calls matching your search terms
- Reports the hit count and asks you to confirm before pulling full transcripts
- Pulls transcript JSON and participant metadata in batches

### Phase 3: Analyze

- For large sets (5+ transcripts), runs parallel analysis agents for speed
- Extracts pain points, verbatim customer quotes with attribution, and frequency data
- Synthesizes the top 5–10 findings ranked by frequency, severity, and breadth

### Phase 4: Output

For each finding, the report includes:
- Pain point description with context
- Verbatim customer quotes with speaker and company attribution
- Calls citing the finding (dates + companies)
- Concrete UX improvement suggestion
- One-paragraph design rationale

The report is saved locally as markdown and can optionally be published to Confluence.

## Example Usage

```
You: "analyze gong transcripts"
Agent: "What product or feature area do you want to analyze?"
You: "Timesheets"
Agent: [proposes search terms, asks about time range and focus...]
Agent: "Found 18 calls mentioning Timesheets in the last 6 months. Proceed?"
You: "yes"
Agent: [pulls transcripts, analyzes, produces report]
```

## Trigger Phrases

- "analyze gong transcripts"
- "gong call analysis"
- "gong UX research"
- "what are customers saying about [product]"
- "customer pain points from gong"
- "transcript analysis"

## Prerequisites

| Requirement | Details |
|-------------|---------|
| Snowflake MCP | `user-snowflake` server configured in Cursor with access to `PROCORE_IT` database |
| Okta SSO | Complete browser authentication when Snowflake prompts |
| Atlassian MCP | `user-atlassian` server (only needed if publishing to Confluence) |

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Main workflow — interview, query, analyze, output phases |
| `reference.md` | Snowflake schema details, SQL templates, search term patterns, Confluence publishing reference |

## Data Source

All queries run against Snowflake:

| View | Content |
|------|---------|
| `GONG_CALL_TRANSCRIPTS_PREP` | Full call transcript JSON (speaker turns, sentences, topics, timestamps) |
| `GONG_CONVERSATIONS_PREP` | Call metadata (date, type, conversation key) |
| `GONG_CONVERSATION_PARTICIPANTS_PREP` | Participant names, companies, roles, emails |

**Path:** `PNT_PRODUCT_MGMT_WH` warehouse → `PROCORE_IT` database → `GONG_DATA_CLOUD_PREP` schema

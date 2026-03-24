---
name: gong-transcript-analysis
description: >-
  Analyze Gong call transcripts from Snowflake for any Procore product or feature.
  Interviews the user to define product scope, search terms, and analysis goals,
  then queries Snowflake, extracts pain points and opportunities, and produces a
  structured UX analysis with customer quotes and design rationales. Optionally
  publishes to Confluence. Use when the user says "analyze gong transcripts",
  "gong call analysis", "gong UX research", "customer pain points from gong",
  "what are customers saying about [product]", or "transcript analysis".
---

# Gong Transcript Analysis

Conversational workflow that queries Gong call transcripts stored in Snowflake,
identifies usability pain points and opportunities for a user-specified product
or feature, and produces a structured analysis report.

## Prerequisites

- **Snowflake MCP** (`user-snowflake`) configured and connected
- **Atlassian MCP** (`user-atlassian`) if publishing to Confluence

If Snowflake is not responding, ask the user to restart the MCP server in
Cursor settings and complete Okta SSO when the browser opens.

---

## Phase 1: Interview — Scope the Analysis

Collect the following from the user through conversation. Use `AskQuestion` for
structured choices where noted; ask open-ended questions conversationally for
the rest. Confirm all inputs before querying.

### 1.1 Product / Feature Focus

Ask: *"What product or feature area do you want to analyze?"*

Get a short name (e.g. "Pages", "Dataset Editor", "Timesheets", "RFIs") and
a one-sentence description of what it does, so you can craft precise search terms.

### 1.2 Search Terms

Based on the product description, **propose** a set of search terms in two tiers:

| Tier | Purpose | Example |
|------|---------|---------|
| **Primary** | Unique product-specific phrases unlikely to match unrelated calls | `"dataset editor"`, `"data set editor"`, `"ai generated column"` |
| **Secondary** | Common words that need a co-occurrence qualifier | `"pages"` only when paired with `"data grid"` or `"data canvas"` |

Present both tiers and ask the user to confirm, add, or remove terms.

**Guideline for good terms:**
- Prefer multi-word phrases over single common words
- Include alternate spellings / phrasings customers might use
- For generic product names (e.g. "Pages"), always pair with a qualifier

### 1.3 Time Range

Ask (with `AskQuestion`):
- Last 30 days
- Last 90 days
- Last 6 months
- Last 12 months
- All available data
- Custom range (then ask for start/end dates)

### 1.4 Analysis Focus

Ask (with `AskQuestion`, allow multiple):
- Usability pain points & UX improvement suggestions
- Feature requests & unmet needs
- Competitive mentions & comparison to other tools
- Adoption blockers & onboarding friction
- Sentiment & satisfaction signals
- Custom focus (then ask for description)

### 1.5 Confirmation

Summarize the scope back to the user:

> **Product:** [name]
> **Search terms:** [primary] + [secondary with qualifiers]
> **Time range:** [range]
> **Analysis focus:** [selected areas]
> **Estimated output:** Top 5–10 findings with customer quotes, UX recommendations, and design rationales

Ask for approval before proceeding.

---

## Phase 2: Query Snowflake

Use the `user-snowflake` MCP server. All queries go through `execute_query`.

### 2.1 Set Warehouse

```sql
USE WAREHOUSE PNT_PRODUCT_MGMT_WH;
```

### 2.2 Discovery Query — Find Matching Calls

Build a query against the Gong transcript views. See [reference.md](reference.md)
for full schema details and SQL templates.

**Core pattern:**

```sql
SELECT DISTINCT c.CONVERSATION_KEY, c.STARTED_DATE
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CALL_TRANSCRIPTS_PREP t
JOIN PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATIONS_PREP c
  ON t.CONVERSATION_KEY = c.CONVERSATION_KEY
WHERE (
    -- Primary terms: direct ILIKE match
    LOWER(t.TRANSCRIPT::STRING) ILIKE '%<term>%'
    -- Secondary terms: require co-occurrence
    OR (LOWER(t.TRANSCRIPT::STRING) ILIKE '%<generic_term>%'
        AND LOWER(t.TRANSCRIPT::STRING) ILIKE '%<qualifier>%')
  )
  AND c.STARTED_DATE >= DATEADD('day', -<days>, CURRENT_DATE())
ORDER BY c.STARTED_DATE DESC;
```

**Report the hit count** to the user before pulling full transcripts:

> Found **N calls** mentioning [product] in the last [range]. Proceed with
> pulling full transcripts?

If N > 25, suggest narrowing terms or time range. If N = 0, suggest broadening.

### 2.3 Pull Full Transcripts

For each matching `CONVERSATION_KEY`, pull the transcript JSON:

```sql
SELECT t.CONVERSATION_KEY, c.STARTED_DATE, t.TRANSCRIPT
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CALL_TRANSCRIPTS_PREP t
JOIN PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATIONS_PREP c
  ON t.CONVERSATION_KEY = c.CONVERSATION_KEY
WHERE t.CONVERSATION_KEY IN ('<key1>', '<key2>', ...)
ORDER BY c.STARTED_DATE DESC;
```

Pull in batches of 5–7 calls if there are many, to stay within response limits.

### 2.4 Pull Participant Metadata (optional)

```sql
SELECT CONVERSATION_KEY, PARTICIPANT_NAME, AFFILIATION, TITLE
FROM PROCORE_IT.GONG_DATA_CLOUD_PREP.GONG_CONVERSATION_PARTICIPANTS_PREP
WHERE CONVERSATION_KEY IN ('<key1>', '<key2>', ...)
ORDER BY CONVERSATION_KEY;
```

This enriches the analysis with customer names, companies, and roles.

---

## Phase 3: Analyze Transcripts

### 3.1 Parallelized Extraction

If there are more than 5 transcripts, use `Task` subagents to analyze in
parallel batches. Each subagent receives:
- The raw transcript JSON for its batch
- The analysis focus areas from Phase 1
- Instructions to extract:
  - **Pain points / friction** — what frustrated customers or blocked them
  - **Opportunities / requests** — what customers wished existed
  - **Verbatim quotes** — exact customer words with speaker attribution
  - **Affected calls** — which call dates / companies cited each finding

### 3.2 Synthesis

Merge subagent results and deduplicate. Rank findings by:
1. **Frequency** — how many calls mention it
2. **Severity** — how much it blocks or frustrates users
3. **Breadth** — how many different companies cite it

Select the top 5–10 findings.

### 3.3 Generate Recommendations

For each finding, produce:

| Section | Content |
|---------|---------|
| **Pain point** | 2–4 sentence description with context |
| **Customer quote(s)** | 1–3 verbatim blockquotes with attribution |
| **Calls citing this** | Dates and company names |
| **UX Improvement** | Concrete, actionable design suggestion |
| **Design Rationale** | One paragraph explaining *why* this fix works for the domain |

---

## Phase 4: Output

### 4.1 Report Template

```markdown
# [Product]: [Analysis Type — e.g. Top Usability Pain Points and UX Opportunities]

**Source:** N Gong customer research calls ([date range])
**Customers represented:** [company list]
**Analysis date:** [today]

---

## 1. [Finding Title]

**Pain point:** [description]

> *"[verbatim quote]"* — [Speaker, Company]

**Calls citing this:** [dates + companies]

**UX Improvement:** [recommendation]

**Design Rationale:** [paragraph]

---

[... repeat for each finding ...]

---

## Summary Matrix

| # | Pain Point | Severity | Frequency | Suggested Fix |
|---|------------|----------|-----------|---------------|
| 1 | ... | High/Med/Low | N/M calls | ... |

---

## Data Source

**Database:** `PROCORE_IT.GONG_DATA_CLOUD_PREP` (Snowflake)

| Table / View | Purpose |
|---|---|
| `GONG_CALL_TRANSCRIPTS_PREP` | Full call transcript JSON |
| `GONG_CONVERSATIONS_PREP` | Conversation metadata |
| `GONG_CONVERSATION_PARTICIPANTS_PREP` | Participant details |

**Search terms used:** [list]
**Time range:** [range]
**Matching calls:** N
```

### 4.2 Save to File

Save the report to:
`_bmad-output/planning-artifacts/research/<product-slug>-<analysis-type>-<YYYY-MM-DD>.md`

### 4.3 Publish to Confluence (optional)

Ask the user:

> *"Would you like to publish this to Confluence? If so, paste the folder or
> parent page URL."*

If yes, use the `user-atlassian` MCP server:

1. Parse `cloudId` from the Atlassian URL (use `procoretech.atlassian.net` →
   cloud ID `cdf4443f-e3ed-4a9e-9567-bb704cf01f1f` as default for Procore)
2. To find `spaceId`: search for a child page under the parent using
   `searchConfluenceUsingCql` with `ancestor = <parentId>`, then fetch one
   child page via `getConfluencePage` to read `spaceId` from the response
3. Create the page with `createConfluencePage`:
   - `contentFormat: "markdown"`
   - `parentId`: the folder/page ID from the URL
   - `spaceId`: from step 2
4. Return the published URL to the user

---

## Error Handling

| Problem | Resolution |
|---------|------------|
| Snowflake connection timeout | Ask user to restart MCP server and complete Okta SSO |
| "No active warehouse" | Run `USE WAREHOUSE PNT_PRODUCT_MGMT_WH;` |
| Zero matching calls | Suggest broader terms or longer time range |
| Too many calls (>25) | Suggest narrower terms or shorter time range |
| Transcript too large for single query | Pull in batches of 5–7 conversation keys |
| Confluence 404 on folder ID | Use CQL search to find child pages and extract spaceId indirectly |

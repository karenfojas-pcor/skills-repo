# Skills Repo

Cursor Agent Skills for product research, design compliance, and meeting prep workflows.

## Skills

### [gong-transcript-analysis](.cursor/skills/gong-transcript-analysis/)

Analyze Gong call transcripts stored in Snowflake for any Procore product or feature. The skill runs a conversational interview to scope the research, queries transcripts, and produces a structured UX analysis report.

**How it works:**

1. **Interview** — The agent asks you 5 questions to scope the analysis:
   - What product or feature to research (e.g. "Timesheets", "Drawing Management", "RFIs")
   - Search terms to find relevant calls — the agent proposes terms and you confirm/edit
   - Time range (last 30 days to all available data)
   - Analysis focus (pain points, feature requests, competitive mentions, adoption blockers, etc.)
   - Final confirmation before querying

2. **Query** — Runs SQL against `PROCORE_IT.GONG_DATA_CLOUD_PREP` in Snowflake to find matching calls, reports the hit count, and pulls full transcripts + participant metadata in batches.

3. **Analyze** — Processes transcripts (in parallel for large sets), extracts pain points, verbatim customer quotes, and frequency data, then synthesizes the top 5–10 findings with UX improvement suggestions and design rationales.

4. **Output** — Saves a structured markdown report locally and optionally publishes it as a Confluence page.

**Trigger phrases:**
- "analyze gong transcripts"
- "gong call analysis"
- "what are customers saying about [product]"
- "customer pain points from gong"
- "transcript analysis"

**Prerequisites:**
- Snowflake MCP server (`user-snowflake`) configured in Cursor with access to `PROCORE_IT`
- Atlassian MCP server (`user-atlassian`) if you want to publish to Confluence

**Files:**
| File | Purpose |
|------|---------|
| `SKILL.md` | Main workflow — interview, query, analyze, output |
| `reference.md` | Snowflake schema, SQL templates, search term patterns, Confluence publishing details |

---

### [insights-design-compliance-checker](.cursor/skills/insights-design-compliance-checker/)

Review a design artifact (from v0, Figma, or local files) against the [Predictive Insights Sub-library](https://procoretech.atlassian.net/wiki/spaces/UX/pages/3950346282/Predictive+Insights+Sub-library) guidelines and produce a structured compliance report with compliant/non-compliant items and recommendations. The guideline source defaults to the Predictive Insights Sub-library Confluence page — just provide a design URL and go.

**Trigger phrases:**
- "check compliance on this [v0/Figma URL]"
- "review design compliance"
- "check design against guidelines"
- "audit design"

---

### [one-on-one-prep](.cursor/skills/one-on-one-prep/)

Employee-driven 1:1 meeting preparation with optional goals threading. Consolidates priorities, blockers, decisions, progress, and commitments into 6 lean sections designed for a 20-minute meeting. Connects to your goals document (Google Doc, Confluence, or local file) to tag priorities and generate a weekly goals pulse check.

**How it works:**

1. **First run** — Interactive setup asks where your goals live and your meeting preferences, then generates a personal config file automatically.
2. **Weekly prep** — Walks through 6 sections, seeds with context from git/Jira/Slack activity, threads goal tags throughout, and produces a tight, scannable prep doc.
3. **Output** — Saves to `~/Documents/OneOnOnes/YYYY-MM-DD_1on1_prep.md` with bullet-first formatting, inline recommendations, and a goals pulse table.

**Trigger phrases:**
- "prepare for my 1:1" / "1:1 prep" — Full prep (~20 min)
- "quick 1:1 prep" — Priorities + blockers + commitments (~5 min)
- "review last 1:1" — Check commitment status, carry forward
- "setup 1:1 prep" — Reconfigure goals source and preferences

**Files:**
| File | Purpose |
|------|---------|
| `SKILL.md` | Entry point with workflow routing and section overview |
| `OneOnOneTemplate.md` | Output format template |
| `PREFERENCES.template.md` | Documented config template — copy and customize |
| `Workflows/Prepare.md` | Full 6-section prep workflow |
| `Workflows/QuickPrep.md` | Fast prep (priorities + blockers + commitments) |
| `Workflows/Review.md` | Commitment review and carry-forward |
| `Workflows/Setup.md` | Interactive first-run configuration |

---

## Adding Skills to Your Project

Clone this repo and copy the `.cursor/skills/` directory into your project, or symlink individual skills:

```bash
# Copy all skills
cp -r skills-repo/.cursor/skills/ your-project/.cursor/skills/

# Or symlink a single skill
ln -s /path/to/skills-repo/.cursor/skills/gong-transcript-analysis your-project/.cursor/skills/gong-transcript-analysis
```

Skills are automatically discovered by Cursor when placed in `.cursor/skills/` or `~/.cursor/skills/`.

# Insights Design Compliance Checker

Review any design artifact against the [Predictive Insights Sub-library](https://procoretech.atlassian.net/wiki/spaces/UX/pages/3950346282/Predictive+Insights+Sub-library) guidelines and produce a structured compliance report. Supports designs from v0, Figma, local files, or any URL. The guideline source defaults to the Predictive Insights Sub-library Confluence page — no need to provide it each time.

## How It Works

The skill runs a 6-step workflow:

### Step 1: Identify Inputs

You provide the design source:
- **Design source** — a v0 URL, Figma URL, local component file, or screenshot

The **guideline source** defaults to the [Predictive Insights Sub-library](https://procoretech.atlassian.net/wiki/spaces/UX/pages/3950346282/Predictive+Insights+Sub-library) Confluence page. You can override this with a different URL or file if needed.

### Step 2: Fetch the Design

The agent resolves the design source automatically:
- **v0** — extracts component source code and conversation history via the v0 MCP server
- **Figma** — fetches design context and screenshots via the Figma MCP server
- **Local file** — reads directly
- **Other URL** — fetches via web

### Step 3: Fetch the Guidelines

Fetches the Predictive Insights Sub-library from Confluence by default (page ID `3950346282`). If you provide an alternative guideline source:
- **Confluence** — fetches page content via the Atlassian MCP server
- **Local file** — reads directly
- **Other URL** — fetches via web

### Step 4: Extract Requirements

Parses the guideline into a structured checklist with IDs, categories, requirement text, and severity levels (Required / Recommended / Optional).

### Step 5: Evaluate Compliance

Checks each requirement against the design and assigns a verdict:

| Verdict | Meaning |
|---------|---------|
| **Compliant** | Design satisfies the requirement |
| **Partial** | Addressed but incompletely or ambiguously |
| **Non-compliant** | Design does not satisfy the requirement |
| **Not applicable** | Requirement doesn't apply to this design type |

### Step 6: Produce the Report

Generates a structured report with:
- Compliant items summary
- Non-compliant items with gap analysis (guideline says vs. design shows)
- Partial / needs clarification items
- Summary table by category
- Prioritized recommendations

## Example Usage

```
You: "check compliance on this v0 https://v0.app/chat/design-workforce-gap-upP6dLoc1Tf"
Agent: [fetches design from v0, fetches Predictive Insights Sub-library from Confluence, evaluates, produces compliance report]
```

You can also provide a custom guideline source:

```
You: "check this design against this spec" [design URL] [guideline URL]
Agent: [fetches both, evaluates, produces compliance report]
```

## Trigger Phrases

- "review design compliance"
- "check design against guidelines"
- "audit design"
- "compare design to spec"
- Providing a design URL and guideline URL together

## Prerequisites

| Requirement | Details |
|-------------|---------|
| v0 MCP | `user-v0` server (for v0 designs) |
| Figma MCP | `user-figma` server (for Figma designs) |
| Atlassian MCP | `user-atlassian` server (for Confluence guidelines) |

Only the MCP servers relevant to your specific design/guideline sources need to be configured.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Full workflow — fetch, extract, evaluate, report |

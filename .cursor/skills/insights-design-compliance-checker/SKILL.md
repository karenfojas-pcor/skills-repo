---
name: insights-design-compliance-checker
description: >-
  Review a design against a guideline document and produce a structured
  compliance report. Fetches designs from v0, Figma, or any accessible source,
  and guidelines from Confluence, URLs, or local files. Itemizes compliant and
  non-compliant elements with a summary table. Use when the user says "review
  design compliance", "check design against guidelines", "audit design",
  "compare design to spec", or provides a design URL and a guideline URL
  together.
---

# Insights Design Compliance Checker

Review any design artifact against any guideline document and produce a
structured compliance report.

## Workflow

### Step 1: Identify Inputs

The user provides the design source. The guideline source defaults to the
Predictive Insights Sub-library unless the user explicitly provides an
alternative.

| Input | What to look for |
|-------|-----------------|
| **Design source** | v0 URL, Figma URL, local component file, or screenshot |
| **Guideline source** | Defaults to the **Predictive Insights Sub-library** Confluence page (see below). User may override with a different URL or file. |

**Default guideline source:**
- URL: `https://procoretech.atlassian.net/wiki/spaces/UX/pages/3950346282/Predictive+Insights+Sub-library`
- Confluence `cloudId`: `procoretech.atlassian.net`
- Confluence `pageId`: `3950346282`

If the design source is missing, ask for it before proceeding. The guideline
source does NOT need to be provided — always fetch the default unless the user
supplies an alternative.

### Step 2: Fetch the Design

Resolve the design source using the best available method:

**v0 design** (URL contains `v0.app/chat/`):
1. Extract the chat ID from the URL (last path segment, e.g. `eMR0UNah2Ug`).
2. Call `getChat` on the `user-v0` MCP server with `chatId`.
3. If 404, try the slug portion after the last hyphen as the ID.
4. Also check `findChats` to locate related/duplicate chats with full file content.
5. Extract the **component source code** from `chat.files[].content` and the
   **conversation history** from `chat.messages[].content` to understand design
   intent and evolution.

**Figma design** (URL contains `figma.com`):
1. Parse `fileKey` and `nodeId` from the URL.
2. Use the `user-figma` MCP server's `get_design_context` tool.
3. Also call `get_screenshot` for visual reference.

**Local file** (path to `.tsx`, `.vue`, `.svelte`, etc.):
1. Read the file directly.

**Other URL**:
1. Attempt `WebFetch`. If blocked/timeout, try browser MCP tools.

### Step 3: Fetch the Guidelines

If the user did not provide an alternative guideline source, fetch the default:

```
CallMcpTool  server: user-atlassian
             toolName: getConfluencePage
             arguments: { "cloudId": "procoretech.atlassian.net",
                          "pageId": "3950346282",
                          "contentFormat": "markdown" }
```

If the user provides a different guideline source, resolve it:

**Confluence page** (URL contains `atlassian.net/wiki`):
1. Extract `cloudId` (the subdomain, e.g. `mycompany.atlassian.net`) and
   `pageId` (numeric ID in the URL path) from the URL.
2. Call `getConfluencePage` on the `user-atlassian` MCP server with
   `contentFormat: "markdown"`.

**Local file** (`.md`, `.pdf`, `.docx`):
1. Read the file directly.

**Other URL**:
1. Attempt `WebFetch`. If blocked, try browser MCP tools.

### Step 4: Extract Guideline Requirements

Parse the guideline document into a structured checklist of requirements.
For each requirement, capture:

- **ID**: Short identifier (e.g. `HDR-1`, `FOOTER-2`)
- **Category**: Grouping (e.g. Header, Banner, Sections, Footer, States, Interactions)
- **Requirement text**: What the guideline says must be true
- **Severity**: Required vs. Recommended vs. Optional (infer from language like
  "must", "should", "may", "optional", "recommended")

### Step 5: Evaluate Compliance

For each requirement, examine the design source (code, screenshot, conversation
context) and determine:

| Verdict | Criteria |
|---------|----------|
| **Compliant** | Design clearly satisfies the requirement |
| **Partial** | Design addresses it but incompletely or ambiguously |
| **Non-compliant** | Design does not satisfy the requirement |
| **Not applicable** | Requirement doesn't apply to this design type |

For non-compliant and partial items, write a specific explanation referencing
what the guideline says vs. what the design does (or doesn't do).

### Step 6: Produce the Report

Output the report in this format:

```markdown
## Design Compliance Review

**Design**: [name/URL of the design]
**Guidelines**: [name/URL of the guideline document]
**Date**: [current date]

---

### Compliant Items

- **[Category]**: [Brief description of what is met]
  ...

---

### Non-Compliant Items (Itemized)

**1. [Category] - [Short title]**
[Severity: Required | Recommended | Optional]
- **Guideline says**: [quote or paraphrase from the guideline]
- **Design shows**: [what the design actually does]
- **Gap**: [specific delta]

**2. [Category] - [Short title]**
...

---

### Partial / Needs Clarification

**1. [Category] - [Short title]**
- **Guideline says**: ...
- **Design shows**: ...
- **Question**: [what needs team input]

---

### Summary Table

| Category | Status |
|----------|--------|
| [Category 1] | Compliant / Partial / Non-compliant |
| [Category 2] | ... |
| ... | ... |

### Recommendations

Prioritized list of the most impactful gaps to close, ordered by severity
(Required > Recommended > Optional) and user impact.
```

## Tips

- When the design source is code, analyze the component structure, props, state,
  rendered elements, styles, and interaction handlers.
- When the design source includes conversation history (v0 chats), trace the
  evolution to understand the final intended state.
- If a v0 chat's file content shows "GENERATING", look for a related chat with
  the same name that has full file content (use `findChats`).
- For Figma sources, the screenshot provides visual truth; the code output is
  a reference starting point.
- If guidelines reference images you cannot see, note this as a limitation and
  evaluate based on the text description surrounding the image.
- When guidelines are ambiguous, flag the item as "Partial / Needs Clarification"
  rather than guessing.
- **Reference implementation**: The "Design Issue Predictions" v0 chat
  (`e5iqXhPGae1`) is a fully-compliant Insights card. Use `getChat` with that
  ID to fetch its component source as a reference for what "good" looks like
  (header structure, banner format, section layout, footer actions, ellipsis
  menu, empty state, company/project toggle).

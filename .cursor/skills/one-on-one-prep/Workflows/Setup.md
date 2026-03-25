# Setup Workflow

Interactive first-run setup for OneOnOnePrep. Generates a personal PREFERENCES.md by asking the user about their goals, meeting format, and preferences.

This workflow runs automatically on first use (when no PREFERENCES.md is found) or can be triggered explicitly with "setup 1:1 prep" or "reconfigure 1:1."

## Step 1: Determine Config Path

Check which config infrastructure the user has:

```bash
# Check if PAI customization directory exists
ls -d ~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/ 2>/dev/null
```

- If the PAI directory structure exists, config will be saved to:
  `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
- Otherwise, config will be saved to:
  `~/.config/OneOnOnePrep/PREFERENCES.md`

Store the resolved path for Step 5.

## Step 2: Goals Source

Ask the user:

> **Where do your goals live?** This lets the skill tag your priorities, wins, and commitments to your goals and generate a weekly pulse check.
>
> Options:
> 1. **Google Doc** — I'll need the URL or document ID
> 2. **Confluence** — I'll need the page URL
> 3. **Local file** — I'll need the file path (e.g., ~/Documents/goals.md)
> 4. **No goals doc** — Skip goals threading (you can add one later)

### If Google Doc:
- Ask for the document URL or ID
- Ask for the Google account name to use (run `listAccounts` via Google MCP to show available accounts)
- Fetch the document using `readGoogleDoc` with format `markdown`
- Parse the document for goal headings/titles
- Present extracted goals and ask the user to confirm or edit short names:
  > I found these goals in your document:
  > - G1: [extracted goal title]
  > - G2: [extracted goal title]
  >
  > **Do these look right? Edit any short names you'd like to change.**

### If Confluence:
- Ask for the page URL
- Fetch the page content using Atlassian MCP if available
- Parse and extract goal headings, same confirmation flow as above

### If Local file:
- Ask for the file path
- Read the file
- Parse and extract goal headings, same confirmation flow as above

### If No goals:
- Confirm:
  > Got it — skipping goals threading. The skill will work without it. You can add goals anytime by running "setup 1:1 prep" again.
- Set source to `none`, skip goal short names

## Step 3: Meeting Preferences

Ask:

> **How long is your 1:1?** (default: 20 minutes)

> **How often do you meet?** (default: weekly)

Accept the defaults or custom values.

## Step 4: Confirm

Present a summary of the configuration:

```
Here's your OneOnOnePrep configuration:

Goals source: [Google Doc / Confluence / Local file / None]
[If configured: Goal short names:]
  - G1: [name]
  - G2: [name]
Meeting duration: [X] minutes
Cadence: [weekly/biweekly]
Config location: [resolved path]

Does this look right?
```

If the user wants changes, loop back to the relevant step.

## Step 5: Write PREFERENCES.md

Create the config directory if needed:

```bash
mkdir -p [resolved config directory]
```

Write the PREFERENCES.md file with the user's values. Follow this structure:

```markdown
# OneOnOnePrep Preferences

## Meeting

- **Duration target:** [X] minutes
- **Cadence:** [Weekly/Biweekly]

## Goals Document

- **Source:** [Google Doc / Confluence / Local / none]
[If Google Doc:]
- **Document ID:** [doc_id]
- **Google account:** [account_name]
- **Format:** markdown
[If Confluence:]
- **Page URL:** [url]
[If Local:]
- **File path:** [path]
- **Goal short names:**
  - G1: [name]
  - G2: [name]
  - G3: [name]

## Goals Pulse

- **Frequency:** weekly
- **Momentum labels:** Strong / Steady / Seeding / Stalled / Not started

## Output

- **Style:** Tight, scannable bullets. No prose paragraphs.
- **Goal tagging:** Tag priorities, wins, and commitments with (G#) where applicable.
- **Stretch commitments:** Allowed, marked with *Stretch:* prefix.

## Context Sources

Attempt these during setup, skip silently if unavailable:
- Git logs (7 days) in active project directories
- Recently viewed/modified files in workspace
- Jira/Atlassian MCP
- Slack MCP
- Google Docs/Drive MCP
```

## Step 6: Confirm and Continue

```
Setup complete! Your preferences are saved to [resolved path].

You can reconfigure anytime with "setup 1:1 prep" or "reconfigure 1:1".
```

If this was triggered by a first-run detection (user said "prepare for my 1:1" but had no config), hand off to the Prepare workflow to continue the prep session:

> Now let's get you prepped — continuing with your 1:1 prep...

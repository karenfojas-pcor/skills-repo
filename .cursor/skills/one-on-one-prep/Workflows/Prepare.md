# Prepare Workflow

Full interactive 1:1 preparation. Consolidated to 6 sections with optional goals threading. Designed for a configurable meeting duration (default 20 minutes).

## Step 1: Setup & Context Gathering

1. **Create output directory:**
   ```bash
   mkdir -p ~/Documents/OneOnOnes
   ```

2. **Load preferences:**
   Check for PREFERENCES.md in this order:
   1. `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
   2. `~/.config/OneOnOnePrep/PREFERENCES.md`

   If found, load and apply all settings (goals source, meeting duration, output style).

   **If neither exists:** Run the Setup workflow (`Workflows/Setup.md`) inline to create one, then continue with the generated preferences.

3. **Load goals document (if configured):**
   If a goals document is configured in preferences, fetch it now using the appropriate method:
   - **Google Doc:** Use `readGoogleDoc` MCP tool with the configured document ID and account
   - **Confluence:** Use Atlassian MCP to fetch the page
   - **Local file:** Read the file directly
   - **None:** Skip goals loading — goals threading and Goals Pulse will be omitted

   The goals document is used throughout the session to tag priorities, map progress, and generate the goals pulse check.

4. **Check for previous 1:1 prep:**
   ```bash
   ls -t ~/Documents/OneOnOnes/*.md 2>/dev/null | head -1
   ```
   If a previous prep exists, read it to:
   - Surface last week's **commitments** for follow-up
   - Note any **carried-forward items**

5. **Gather work context (attempt each, skip silently if unavailable):**
   - `git log --oneline --since="7 days ago"` in active project directories
   - Check recently viewed/modified files in workspace for activity signals
   - Check Jira/Atlassian MCP for recent tickets if available
   - Check Slack MCP for relevant threads if available

6. **Set the date:**
   ```bash
   DATE=$(date +%Y-%m-%d)
   ```

## Step 2: Commitment Review (2 min)

If a previous prep was found, present the commitments as a table:

```
Last week's commitments:
| # | Commitment | Due |
|---|-----------|-----|
| 1 | [outcome] | [date] |
| 2 | [outcome] | [date] |
```

Ask:
> **Which are done, in progress, or carrying forward?**

Record status. Incomplete items auto-carry into Section 5 (Commitments).

## Step 3: Walk Through Sections

Work through each section interactively. Be conversational but efficient. Seed with gathered context. If the user says "skip" or "nothing," move on immediately.

**PACING RULE:** Use the meeting duration from preferences (default 20 min). Keep prompts tight. Don't re-read questions verbatim — paraphrase with context. Combine naturally flowing topics (e.g., if a blocker surfaces a decision, capture both at once rather than re-asking later).

**GOALS THREADING:** If goals are configured, connect the user's responses to their goals when the connection is natural. Don't force it — but tag priorities and wins to goal numbers where obvious. This builds the pulse check incrementally rather than as a separate exercise. If no goals are configured, skip all goal tagging.

---

### Section 1: Priorities (3 min)

Ask:
- What are the 2-3 most important outcomes you're driving this week?
- What does success look like before the next 1:1?

If you found recent activity, offer it as a starting point:
> Based on your recent activity, it looks like you've been focused on [X]. Still a top priority?

**Goals threading (if configured):** Suggest which goal each priority maps to. Example:
> That sounds like it maps to Goal 1 ([short name]) — does that feel right?

---

### Section 2: Blockers, Decisions & Asks (5 min)

This is a single combined section. Ask:
- Where are you stuck or feeling friction?
- What decisions need your manager's input?
- What do you need from them to move faster?

For each item, push for a **recommendation** — not just a problem statement:
> What's your proposed path forward on that?

Capture each item as: **what** + **recommendation** + **ask type** (blocker / decision / support ask).

If no blockers or asks, note "No blockers — momentum is strong" and move on fast.

---

### Section 3: Progress & Learnings (3 min)

Ask:
- What moved forward since last time?
- What would you do differently?

Seed with:
- Last week's commitment statuses (from Step 2)
- Any visible git/Jira/file activity as prompts for wins

Keep learnings to 1-2 max. This isn't a retrospective.

**Goals threading (if configured):** Tag wins to goal numbers where applicable.

---

### Section 4: Prioritization Check (2 min)

Only needed when the user has more than 3 active threads. Ask:
- Anything to stop, start, or deprioritize?

If the user has 3 or fewer priorities and everything is clear, skip with:
> Priorities look focused — no adjustments needed. Moving on.

---

### Section 5: Commitments (3 min)

Ask:
- What will you deliver before the next 1:1? By when?

Auto-include any incomplete carry-forwards from Step 2. Push for specific, measurable outcomes with dates.

**Goals threading (if configured):** Tag each commitment to a goal number.

Allow the user to mark items as "stretch" for aspirational but non-critical commitments.

---

### Section 6: Goals Pulse (4 min)

**Skip this section entirely if no goals are configured.** Instead, prompt once:
> Want to set up goals tracking? Run "setup 1:1 prep" anytime to connect your goals document.

**If goals are configured:** Present a compact status for each goal based on what was discussed throughout the session:

```
| Goal | Status | This week's movement |
|------|--------|---------------------|
| Goal 1: [short name] | [momentum word] | [1-line summary] |
| Goal 2: [short name] | [momentum word] | [1-line summary] |
```

Momentum words: **Strong** / **Steady** / **Seeding** / **Stalled** / **Not started**

Ask:
> Does this pulse check feel accurate? Anything to adjust?

**FREQUENCY:** This section runs every week by default. If the user's preferences specify a different cadence (e.g., biweekly, monthly), respect that. Even on off-weeks, keep a 1-line "any goal movement to note?" prompt.

---

## Step 4: Generate the Prep Document

Compile all responses into a **tight, scannable** markdown document. Follow the output template in `OneOnOneTemplate.md`.

Write to: `~/Documents/OneOnOnes/${DATE}_1on1_prep.md`

**OUTPUT RULES:**
- Bullets over paragraphs. Always.
- One line per item. Context only when non-obvious.
- Bold the key noun/topic, then brief detail.
- For blockers/decisions/asks: `**Topic** — context. *Rec:* X. *Ask:* Y.`
- No redundancy between sections. If it's captured once, don't repeat it.
- If goals configured, tag priorities and commitments with goal numbers in parentheses: `(G1)`
- If no goals configured, omit all `(G#)` tags and the Goals Pulse section

## Step 5: Final Summary

After writing the file, present:

```
1:1 prep saved to ~/Documents/OneOnOnes/[DATE]_1on1_prep.md

Summary:
- Priorities: [count] ([goal tags if configured])
- Blockers/Decisions/Asks: [count]
- Commitments: [count] with due dates
- Goals pulse: [strongest momentum area] (or "not configured")

Key things to drive in today's meeting:
1. [most important discussion point]
2. [second most important]
3. [third if applicable]
```

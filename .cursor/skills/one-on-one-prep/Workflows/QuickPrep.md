# QuickPrep Workflow

Fast 1:1 preparation when time is short. Covers commitment review, priorities, blockers/decisions, and new commitments — with optional goals threading. ~5 minutes.

## Step 1: Setup

1. **Create output directory:**
   ```bash
   mkdir -p ~/Documents/OneOnOnes
   ```

2. **Load preferences:**
   Check for PREFERENCES.md in this order:
   1. `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
   2. `~/.config/OneOnOnePrep/PREFERENCES.md`

   If found, load goals source and goal short names for tagging.

   **If neither exists:** Do NOT run full setup — QuickPrep should stay fast. Instead, note:
   > No preferences configured — running without goals threading. Run "setup 1:1 prep" to connect your goals document.

3. **Load goals document (if configured):**
   If configured, fetch it. Use goal short names for tagging. If not configured, skip all goal tagging.

4. **Check for previous 1:1 prep:**
   ```bash
   ls -t ~/Documents/OneOnOnes/*.md 2>/dev/null | head -1
   ```
   If found, read it to surface last week's commitments.

5. **Set the date:**
   ```bash
   DATE=$(date +%Y-%m-%d)
   ```

## Step 2: Quick Commitment Review

If a previous prep exists, show last week's commitments as a table:

```
Last week's commitments:
| # | Commitment | Due |
|---|-----------|-----|
| 1 | [outcome] | [date] |
```

Ask: **Quick status on each — done, in progress, or carrying forward?**

## Step 3: Three Essential Sections

Ask these in sequence. Keep it tight — one round per section.

### Priorities
> **Top 2-3 priorities this week? What does "done" look like?**

Tag each to a goal number if goals are configured.

### Blockers & Decisions
> **Anything blocking you or needing a decision? If so, what's your recommendation?**

If nothing, move on immediately.

### Commitments
> **What will you deliver before next 1:1, and by when?**

Carry forward any incomplete items from last week. Tag to goals if configured.

## Step 4: Generate Quick Prep Document

Write to: `~/Documents/OneOnOnes/${DATE}_1on1_prep.md`

```markdown
## 1:1 Prep — [DATE] (Quick)

---

### Commitment Review

| Commitment | Status |
|-----------|--------|
| [outcome] | **Done** / **In progress** / **Carry forward** |

---

### 1. Priorities

- **[Priority 1]** — [detail] (G#)
- **[Priority 2]** — [detail] (G#)

---

### 2. Blockers, Decisions & Asks

- **[Topic]** — [context]. *Rec:* [X]. *Ask:* [Y].

[Or: "No blockers — momentum is strong."]

---

### 3. Commitments

| Outcome | Due | Goal |
|---------|-----|------|
| [outcome] | [date] | G# |

---

*Quick prep — Progress, Goals Pulse skipped. Run full prep with "prepare for my 1:1" if needed.*
```

**Note:** Omit the Goal column and all `(G#)` tags if goals are not configured.

## Step 5: Done

```
1:1 prep saved to ~/Documents/OneOnOnes/[DATE]_1on1_prep.md

Covered: Commitment review, Priorities, Blockers/Decisions, Commitments
Skipped: Progress & Learnings, Goals Pulse

Need the full version? Say "prepare for my 1:1"
```

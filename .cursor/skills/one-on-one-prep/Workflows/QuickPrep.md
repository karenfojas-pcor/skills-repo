# QuickPrep Workflow

Fast 1:1 preparation focused on the essentials. Covers priorities, blockers, and commitments only — the three sections that drive the most value in a 1:1.

## Step 1: Setup

1. **Create output directory:**
   ```bash
   mkdir -p ~/Documents/OneOnOnes
   ```

2. **Check for previous 1:1 prep:**
   ```bash
   ls -t ~/Documents/OneOnOnes/*.md 2>/dev/null | head -1
   ```
   If found, read it to surface last week's commitments.

3. **Set the date:**
   ```bash
   DATE=$(date +%Y-%m-%d)
   ```

## Step 2: Quick Commitment Review

If a previous prep exists, show last week's commitments:

```
📋 Last week's commitments:
1. [Outcome] — Due: [date] — Status: ?
```

Ask: **Quick status on each — done, in progress, or dropping?**

## Step 3: Three Essential Questions

Ask these three questions in sequence. Keep it tight — one round per section.

### Priorities
> **What are your top 2-3 priorities this week? What does "done" look like for each?**

### Blockers
> **Anything blocking you? If so, what's your proposed solution?**

If no blockers, move on immediately.

### Commitments
> **What will you deliver before the next 1:1, and by when?**

Carry forward any incomplete items from last week.

## Step 4: Generate Quick Prep Document

Write to: `~/Documents/OneOnOnes/${DATE}_1on1_prep.md`

```markdown
## Weekly 1:1 Prep — [DATE] (Quick)

---

### 1. 🎯 Top Priorities & Outcomes

**My focus this week:**
- [priority 1]
- [priority 2]

---

### 2. 🚧 Blockers & Proposed Solutions

[Blockers or "No blockers this week."]

---

### 4. 📈 Progress (from last week)

[Status of last week's commitments]

---

### 7. 🚀 Commitments Before Next 1:1

| Outcome | Due Date |
|---------|----------|
| [outcome 1] | [date] |
| [outcome 2] | [date] |

---

*Quick prep — sections 3, 5, 6, 8 skipped. Run full prep with "prepare for my 1:1" if needed.*
```

## Step 5: Done

```
✅ Quick 1:1 prep saved to ~/Documents/OneOnOnes/[DATE]_1on1_prep.md

Covered: Priorities, Blockers, Commitments
Skipped: Decisions, Prioritization Check, Support Needed, Growth

Need the full version? Say "prepare for my 1:1"
```

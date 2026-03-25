# Review Workflow

Review commitments and outcomes from the most recent 1:1 prep. Surfaces what's done, what's outstanding, and what needs to carry forward.

## Step 1: Find the Most Recent Prep

```bash
ls -t ~/Documents/OneOnOnes/*.md 2>/dev/null | head -5
```

If no prep docs found:
> No previous 1:1 prep documents found in `~/Documents/OneOnOnes/`. Run "prepare for my 1:1" to create your first one.

If found, read the most recent file.

## Step 2: Extract and Present Commitments

Parse the Commitments section from the most recent prep. Present them:

```
Commitments from [DATE]:

| # | Outcome | Due | Goal |
|---|---------|-----|------|
| 1 | [outcome] | [date] | G# |
| 2 | [outcome] | [date] | G# |
```

**Note:** If the prep doc doesn't have goal tags, omit the Goal column.

## Step 3: Status Check

Ask the user for status on each commitment:

> **For each commitment, what's the status?**
> - Done
> - In progress (what's left?)
> - Dropped (why?)
> - Carry forward (new due date?)

## Step 4: Surface Other Follow-ups

Also check the previous prep for:
- **Unresolved blockers/decisions/asks** — Are these still open?
- **Goals pulse** — Any shifts in goal momentum since then? (Only if goals were tracked)

Present anything that looks unresolved and ask if it should carry forward.

## Step 5: Generate Review Summary

```
1:1 Review — [DATE of reviewed prep]

COMMITMENTS:
- Done: [count]
- In progress: [count]
- Dropped: [count]
- Carry forward: [count]

CARRY-FORWARD ITEMS:
- [item] — new due date: [date] (G#)

OPEN FROM LAST WEEK:
- [blocker/decision/ask if any]
```

Suggest: **Ready to prep for this week? Say "prepare for my 1:1" — I'll pre-load these carry-forward items.**

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

Parse Section 7 (Commitments) from the most recent prep. Present them:

```
📋 Commitments from [DATE]:

1. [Outcome] — Due: [date]
2. [Outcome] — Due: [date]
```

## Step 3: Status Check

Ask the user for status on each commitment:

> **For each commitment, what's the status?**
> - ✅ Done
> - 🔄 In progress (what's left?)
> - ❌ Dropped (why?)
> - ➡️ Carry forward (new due date?)

## Step 4: Surface Other Follow-ups

Also check the previous prep for:
- **Unresolved blockers** (Section 2) — Are these still blocking?
- **Pending decisions** (Section 3) — Were these decided?
- **Support asks** (Section 6) — Did you get the help you needed?
- **Growth focus** (Section 8) — Any progress on this?

Present anything that looks unresolved and ask if it should carry forward.

## Step 5: Generate Review Summary

Present a clean summary:

```
📊 1:1 Review — [DATE of reviewed prep]

COMMITMENTS:
✅ Done: [count]
🔄 In progress: [count]
❌ Dropped: [count]
➡️ Carry forward: [count]

CARRY-FORWARD ITEMS:
- [item] — new due date: [date]
- [item] — new due date: [date]

UNRESOLVED FROM LAST WEEK:
- [blocker/decision/ask if any]
```

Suggest: **Ready to prep for this week? Say "prepare for my 1:1" — I'll pre-load these carry-forward items.**

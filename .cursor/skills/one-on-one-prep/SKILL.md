---
name: OneOnOnePrep
description: Weekly 1:1 meeting preparation — walks through priorities, blockers, decisions, progress, and commitments to produce a ready-to-use prep doc. USE WHEN 1:1 prep, prepare for 1:1, one on one, 1 on 1, weekly prep, manager meeting prep, prep for my 1:1, review last 1:1, quick 1:1 prep.
---

## Customization

**Before executing, check for user customizations at:**
`~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/`

If this directory exists, load and apply any PREFERENCES.md, configurations, or resources found there. These override default behavior. If the directory does not exist, proceed with skill defaults.

# OneOnOnePrep

Employee-driven 1:1 meeting preparation. Walks you through each section of your 1:1 template, gathers context from your work (git, Jira, recent activity), and produces a completed prep document ready to bring to your meeting.

## Workflow Routing

| Workflow | Trigger | File |
|----------|---------|------|
| **Prepare** | "prepare for my 1:1", "1:1 prep", "prep for 1:1" | `Workflows/Prepare.md` |
| **QuickPrep** | "quick 1:1 prep", "fast 1:1 prep", "short prep" | `Workflows/QuickPrep.md` |
| **Review** | "review last 1:1", "check my commitments", "follow up on 1:1" | `Workflows/Review.md` |

## Examples

**Example 1: Full weekly prep**
```
User: "Prepare for my 1:1"
→ Invokes Prepare workflow
→ Loads previous 1:1 for commitment tracking
→ Walks through all 8 template sections interactively
→ Produces completed prep doc at ~/Documents/OneOnOnes/
```

**Example 2: Quick prep when short on time**
```
User: "Quick 1:1 prep"
→ Invokes QuickPrep workflow
→ Focuses on top priorities, blockers, and commitments only
→ Produces a focused prep doc in ~5 minutes
```

**Example 3: Review commitments from last week**
```
User: "Review last 1:1"
→ Invokes Review workflow
→ Reads most recent prep doc
→ Shows commitment status and carries forward incomplete items
```

## Storage

- **Prep docs:** `~/Documents/OneOnOnes/YYYY-MM-DD_1on1_prep.md`
- **Template reference:** `OneOnOneTemplate.md` (in skill root)

## Quick Reference

- **Full prep:** ~15 min, all 8 sections, interactive
- **Quick prep:** ~5 min, priorities + blockers + commitments
- **Review:** Check last week's commitments, carry forward

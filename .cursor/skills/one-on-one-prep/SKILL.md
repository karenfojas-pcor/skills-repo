---
name: OneOnOnePrep
description: Weekly 1:1 meeting preparation — walks through priorities, blockers, decisions, progress, and commitments to produce a ready-to-use prep doc. USE WHEN 1:1 prep, prepare for 1:1, one on one, 1 on 1, weekly prep, manager meeting prep, prep for my 1:1, review last 1:1, quick 1:1 prep, setup 1:1 prep.
---

## Customization

**Before executing, check for user preferences at (in order):**
1. `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
2. `~/.config/OneOnOnePrep/PREFERENCES.md`

If a PREFERENCES.md is found at either path, load and apply it. These configure: goals document, meeting duration, output style, and section behavior. If neither path exists, trigger the **Setup** workflow to create one interactively.

See `PREFERENCES.template.md` in this skill directory for a fully documented example.

# OneOnOnePrep

Employee-driven 1:1 meeting preparation. Consolidated to 6 sections with optional goals threading. Gathers context from your work (git, Jira, Google Docs, recent activity), optionally loads your goals document, and produces a tight, scannable prep doc.

## Workflow Routing

| Workflow | Trigger | File |
|----------|---------|------|
| **Prepare** | "prepare for my 1:1", "1:1 prep", "prep for 1:1" | `Workflows/Prepare.md` |
| **QuickPrep** | "quick 1:1 prep", "fast 1:1 prep", "short prep" | `Workflows/QuickPrep.md` |
| **Review** | "review last 1:1", "check my commitments", "follow up on 1:1" | `Workflows/Review.md` |
| **Setup** | "setup 1:1 prep", "configure 1:1 prep", "reconfigure 1:1" | `Workflows/Setup.md` |

## Sections (6 consolidated)

| # | Section | Time | What it covers |
|---|---------|------|---------------|
| 0 | Commitment Review | 2 min | Last week's commitments — done / in progress / carry forward |
| 1 | Priorities | 3 min | 2-3 outcomes, tagged to goals (if configured) |
| 2 | Blockers, Decisions & Asks | 5 min | Merged: blockers + decisions + support needed. Each with a recommendation. |
| 3 | Progress & Learnings | 3 min | Wins (tagged to goals if configured) + 1-2 learnings |
| 4 | Commitments | 3 min | Deliverables with due dates, tagged to goals. Supports stretch items. |
| 5 | Goals Pulse | 4 min | Compact status table mapped to your goals document (skipped if no goals configured) |

**Previously separate sections now folded in:**
- Prioritization Check → folded into Priorities (only surfaces if stop/start adjustments exist)
- Support Needed → folded into Blockers, Decisions & Asks (the "Ask" field)
- Growth & Ownership → folded into Goals Pulse (growth shows as goal movement)

## First-Run Experience

The first time you use this skill, if no PREFERENCES.md is found, it will walk you through an interactive setup:
1. Where are your goals? (Google Doc, Confluence, local file, or none)
2. Meeting duration and cadence
3. Fetches your goals and extracts short names for tagging

This generates your personal PREFERENCES.md automatically. You can reconfigure anytime with "setup 1:1 prep."

## Examples

**Example 1: First-time setup**
```
User: "Prepare for my 1:1"
→ No preferences found — triggers interactive setup
→ Asks for goals source, meeting preferences
→ Generates PREFERENCES.md
→ Continues into full prep workflow
```

**Example 2: Full weekly prep (after setup)**
```
User: "Prepare for my 1:1"
→ Loads preferences (goals doc, meeting duration)
→ Loads previous 1:1 for commitment tracking
→ Walks through 6 sections interactively with goals threading
→ Produces tight, scannable prep doc at ~/Documents/OneOnOnes/
```

**Example 3: Quick prep when short on time**
```
User: "Quick 1:1 prep"
→ Invokes QuickPrep workflow
→ Focuses on priorities, blockers/decisions, and commitments only
→ Produces a focused prep doc in ~5 minutes
```

**Example 4: Review commitments from last week**
```
User: "Review last 1:1"
→ Invokes Review workflow
→ Reads most recent prep doc
→ Shows commitment status and carries forward incomplete items
```

## Storage

- **Prep docs:** `~/Documents/OneOnOnes/YYYY-MM-DD_1on1_prep.md`
- **Template reference:** `OneOnOneTemplate.md` (in skill root)
- **Preferences (PAI users):** `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
- **Preferences (general):** `~/.config/OneOnOnePrep/PREFERENCES.md`
- **Preferences template:** `PREFERENCES.template.md` (in skill root)

## Quick Reference

- **Setup:** Interactive config, runs automatically on first use or via "setup 1:1 prep"
- **Full prep:** ~20 min, 6 sections with goals threading, interactive
- **Quick prep:** ~5 min, priorities + blockers + commitments
- **Review:** Check last week's commitments, carry forward

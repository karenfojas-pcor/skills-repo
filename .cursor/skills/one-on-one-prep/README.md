# OneOnOnePrep

An AI-powered Cursor skill for employee-driven 1:1 meeting preparation. Walks you through a structured prep session, threads your priorities to your goals, and produces a tight, scannable prep document ready for your meeting.

## What It Does

- **6 consolidated sections** covering priorities, blockers/decisions, progress, commitments, and goals pulse -- designed for a 20-minute meeting
- **Goals threading** -- connects your weekly work to your strategic goals (Google Doc, Confluence, or local file)
- **Commitment tracking** -- carries forward incomplete items from last week
- **Context gathering** -- pulls recent activity from git, Jira, Slack, and more (when available via MCP)
- **Three workflows** -- full prep (~20 min), quick prep (~5 min), and commitment review

## Installation

Copy the `one-on-one-prep` folder into your Cursor skills directory:

```
.cursor/skills/one-on-one-prep/
```

## First Run

Just say **"prepare for my 1:1"** and the skill will walk you through setup:

1. Where are your goals? (Google Doc, Confluence, local file, or none)
2. Meeting duration and cadence
3. The skill fetches your goals, extracts short names, and saves your config

Your preferences are stored locally and never committed to the repo.

## Usage

| Command | What it does | Time |
|---------|-------------|------|
| "prepare for my 1:1" | Full interactive prep, all 6 sections | ~20 min |
| "quick 1:1 prep" | Priorities + blockers + commitments only | ~5 min |
| "review last 1:1" | Check commitment status, carry forward | ~5 min |
| "setup 1:1 prep" | Reconfigure goals source, meeting prefs | ~3 min |

## Output

Prep docs are saved to `~/Documents/OneOnOnes/YYYY-MM-DD_1on1_prep.md` as clean markdown with:
- Bullet-first formatting (no prose paragraphs)
- Goal tags `(G1)`, `(G2)` on priorities, wins, and commitments
- Inline blocker format: `*Rec:* [X]. *Ask:* [Y].`
- Goals Pulse table with momentum status (Strong / Steady / Seeding / Stalled)

## Goals Support

The skill can connect to your goals document to thread goal awareness through every section. Supported sources:

| Source | How it works |
|--------|-------------|
| **Google Doc** | Fetches via Google MCP, extracts goal titles |
| **Confluence** | Fetches via Atlassian MCP, extracts goal titles |
| **Local file** | Reads a markdown file from your filesystem |
| **None** | Works fine without goals -- just skips tagging and pulse check |

## Configuration

Your preferences are saved to one of these locations (checked in order):
1. `~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md`
2. `~/.config/OneOnOnePrep/PREFERENCES.md`

To configure manually instead of using the interactive setup, copy `PREFERENCES.template.md` to one of the paths above and fill in your values.

## File Structure

```
one-on-one-prep/
  SKILL.md                    -- Skill entry point and routing
  OneOnOneTemplate.md         -- Output format template
  PREFERENCES.template.md     -- Documented config template (copy and customize)
  README.md                   -- This file
  Workflows/
    Prepare.md                -- Full prep workflow (6 sections)
    QuickPrep.md              -- Fast prep (priorities + blockers + commitments)
    Review.md                 -- Commitment review and carry-forward
    Setup.md                  -- Interactive first-run configuration
```

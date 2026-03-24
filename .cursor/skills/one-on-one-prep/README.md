# One-on-One Prep

Employee-driven 1:1 meeting preparation. Walks you through each section of your 1:1 template, gathers context from your recent work, and produces a completed prep document ready to bring to your meeting.

## How It Works

The skill has three workflows depending on what you need:

### Prepare (Full Prep, ~15 min)

Walks through all 8 sections of the 1:1 template interactively:
- Loads your previous 1:1 for commitment tracking
- Asks about priorities, blockers, decisions, progress, and commitments
- Produces a completed prep doc

### QuickPrep (~5 min)

Focused version for when you're short on time:
- Covers only top priorities, blockers, and commitments
- Produces a focused prep doc

### Review

Checks your commitments from your last 1:1:
- Reads your most recent prep doc
- Shows commitment status
- Carries forward incomplete items

## Example Usage

```
You: "prepare for my 1:1"
Agent: [loads previous 1:1, walks through all sections]
Agent: [produces completed prep doc at ~/Documents/OneOnOnes/]
```

```
You: "quick 1:1 prep"
Agent: [focused on priorities + blockers + commitments]
Agent: [produces focused prep doc in ~5 minutes]
```

```
You: "review last 1:1"
Agent: [reads most recent prep, shows commitment status]
```

## Trigger Phrases

| Workflow | Triggers |
|----------|----------|
| **Prepare** | "prepare for my 1:1", "1:1 prep", "prep for 1:1" |
| **QuickPrep** | "quick 1:1 prep", "fast 1:1 prep", "short prep" |
| **Review** | "review last 1:1", "check my commitments", "follow up on 1:1" |

## Storage

- **Prep docs:** `~/Documents/OneOnOnes/YYYY-MM-DD_1on1_prep.md`
- **Template:** `OneOnOneTemplate.md` in the skill root

## Customization

You can override default behavior by adding preferences at:
`~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/`

Place a `PREFERENCES.md` or other configuration files there. If the directory doesn't exist, the skill uses its defaults.

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill with workflow routing and examples |
| `OneOnOneTemplate.md` | 1:1 meeting template |
| `Workflows/` | Individual workflow definitions (Prepare, QuickPrep, Review) |

# 1:1 Output Template

Lean, scannable format designed for a 20-minute meeting. Used by Prepare and QuickPrep workflows to generate the final prep doc.

## Output Format

```markdown
## 1:1 Prep — [DATE]

**One-line context:** [What's the main theme or shift this week?]

---

### Commitment Review

| Commitment | Status |
|-----------|--------|
| [outcome from last week] | **Done** / **In progress** — [brief note] / **Carry forward** |

---

### 1. Priorities (G#)

- **[Priority 1]** — [one-line detail] (G#)
- **[Priority 2]** — [one-line detail] (G#)
- **[Priority 3]** — [one-line detail] (G#)

---

### 2. Blockers, Decisions & Asks

- **[Topic]** — [context]. *Rec:* [recommendation]. *Ask:* [what you need] / *No ask.*
- **[Topic]** — [context]. *Rec:* [recommendation]. *Ask:* [what you need] / *No ask.*

[If none: "No blockers — momentum is strong."]

---

### 3. Progress & Learnings

**Wins:**
- [win] (G#)
- [win] (G#)

**Learnings:**
- [one learning, max two]

---

### 4. Commitments

| Outcome | Due | Goal |
|---------|-----|------|
| [outcome] | [date] | G# |
| [outcome] | [date] | G# |
| *Stretch:* [outcome] | [date] | G# |

---

### 5. Goals Pulse

| Goal | Status | This week |
|------|--------|-----------|
| G1: [short name] | Strong / Steady / Seeding / Stalled | [1-line] |
| G2: [short name] | Strong / Steady / Seeding / Stalled | [1-line] |
| G3: [short name] | Strong / Steady / Seeding / Stalled | [1-line] |
| G4: [short name] | Strong / Steady / Seeding / Stalled | [1-line] |
```

**Note:** If goals are not configured, omit the `(G#)` tags and the Goals Pulse section entirely. The rest of the template works standalone.

## Formatting Rules

- **Bullets over paragraphs.** Always.
- **One line per item.** Context only when non-obvious.
- **Bold the key noun/topic,** then brief detail after an em dash.
- **Goal tags** in parentheses: `(G1)`, `(G2)`, etc. Only if goals are configured.
- **No redundancy.** If captured in one section, don't repeat in another.
- **Blockers/Decisions/Asks** use inline format: `*Rec:* [X]. *Ask:* [Y].`
- **Stretch commitments** are italicized with the `*Stretch:*` prefix.
- **Prioritization check** is folded into the Priorities section — only note stop/start/deprioritize if there's an actual change. Don't include a separate section for "continue everything."
- **Support needed** is folded into Blockers, Decisions & Asks — the "Ask" field captures what you need from your manager. No separate section.
- **Growth & Ownership** is folded into Goals Pulse — the growth focus shows up as movement on the goals, not as a separate discussion.

## Section Reference (for interactive prompting)

| Section | Time | Prompting questions |
|---------|------|-------------------|
| Commitment Review | 2 min | Which are done, in progress, or carrying forward? |
| Priorities | 3 min | 2-3 most important outcomes? What does success look like? |
| Blockers/Decisions/Asks | 5 min | Where stuck? Decisions pending? What do you need? Proposed path forward? |
| Progress & Learnings | 3 min | What moved forward? What would you do differently? |
| Commitments | 3 min | What will you deliver? By when? |
| Goals Pulse | 4 min | Does this pulse check feel accurate? (Only if goals configured) |

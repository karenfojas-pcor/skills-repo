# Prepare Workflow

Full interactive 1:1 preparation. Walks through all 8 template sections, gathers context, and produces a completed prep document.

## Step 1: Setup & Context Gathering

1. **Create output directory:**
   ```bash
   mkdir -p ~/Documents/OneOnOnes
   ```

2. **Check for previous 1:1 prep:**
   ```bash
   ls -t ~/Documents/OneOnOnes/*.md 2>/dev/null | head -1
   ```
   If a previous prep exists, read it to:
   - Surface last week's **commitments** (Section 7) for follow-up
   - Note any **carried-forward blockers**
   - Reference previous **growth focus**

3. **Gather work context (attempt each, skip silently if unavailable):**
   - `git log --oneline --since="7 days ago"` in active project directories
   - Check Jira/Atlassian MCP for recent tickets if available
   - Check Slack MCP for relevant threads if available

4. **Set the date:**
   ```bash
   DATE=$(date +%Y-%m-%d)
   ```

## Step 2: Review Last Week's Commitments

If a previous prep was found, present the commitments from it:

```
📋 Last week you committed to:
1. [Outcome] — Due: [date]
2. [Outcome] — Due: [date]
```

Ask the user:
> **Which of these are complete, in progress, or need to carry forward?**

Record the status of each for inclusion in Section 4 (Progress) and Section 7 (new Commitments).

## Step 3: Walk Through Each Section

Work through each section interactively. For each section, share the prompting questions and ask the user to respond. Use any gathered context (git activity, previous prep, Jira) to offer suggestions.

**IMPORTANT:** Be conversational but efficient. Don't just read back questions — add context from what you've gathered. If the user says "skip" or "nothing" for a section, move on gracefully.

---

### Section 1: Top Priorities & Outcomes

Present the prompting questions:
- What are the 2-3 most important outcomes you're driving right now?
- What does success look like before the next 1:1?

If you found recent git/Jira activity, offer it as a starting point:
> Based on your recent activity, it looks like you've been focused on [X]. Is that still a top priority?

---

### Section 2: Blockers & Proposed Solutions

Ask:
- Where are you stuck?
- What have you already tried?
- What do you recommend we do next?

Coach toward the **recommendation** — the template is designed for the employee to come with proposed solutions, not just problems. If the user describes a blocker without a recommendation, prompt:
> What's your proposed path forward on that?

For each blocker, capture: Context, Options considered, Recommendation.

---

### Section 3: Decisions Needed

Ask:
- What decisions are pending that need your manager's input or alignment?
- What's your proposed call?

Same coaching pattern — push for a recommendation, not just "I need a decision on X."

---

### Section 4: Progress & Learnings

Ask:
- What moved forward since last time?
- What didn't go as expected?
- What would you do differently?

Seed this with last week's commitments and their status (from Step 2). Also reference any git/Jira activity as prompts for wins.

---

### Section 5: Prioritization Check

Ask:
- Are you working on the highest-impact things?
- What should you stop, start, or deprioritize?

Frame this as a forcing function: if the user has more than 3 priorities, ask which ones they'd cut.

---

### Section 6: Support Needed

Ask:
- Where do you need help from your manager or leadership?
- What can they do to unblock or accelerate you?

Connect this to blockers from Section 2 — if a blocker needs manager intervention, suggest adding it here.

---

### Section 7: Commitments Before Next 1:1

Ask:
- What will you deliver before the next 1:1?
- By when?

Carry forward any incomplete commitments from last week. Push for specific, measurable outcomes with dates.

---

### Section 8: Growth & Ownership (Optional)

Ask:
- What responsibility do you want to take on next?
- Where do you want to stretch?

If the user skips this, that's fine. But gently encourage it periodically.

---

## Step 4: Generate the Prep Document

Compile all responses into a clean markdown document following the template structure.

Write to: `~/Documents/OneOnOnes/${DATE}_1on1_prep.md`

**Document format:**

```markdown
## Weekly 1:1 Prep — [DATE]

---

### 1. 🎯 Top Priorities & Outcomes

**My focus this week:**
- [priority 1]
- [priority 2]
- [priority 3 if applicable]

---

### 2. 🚧 Blockers & Proposed Solutions

**Blocker 1:** [title]
- Context: [context]
- Options considered: [options]
- My recommendation: [recommendation]

[Blocker 2 if applicable]

---

### 3. 🧠 Decisions Needed

**Decision 1:** [title]
- Context: [context]
- Options: [options]
- My recommendation: [recommendation]

---

### 4. 📈 Progress & Learnings

**Wins:**
- [win 1]
- [win 2]

**Learnings:**
- [learning 1]

---

### 5. 🎯 Prioritization Check

**Adjustments:**
- Stop: [items]
- Start: [items]
- Continue: [items]

---

### 6. 🤝 Support Needed

**Specific asks:**
- [ask 1]

---

### 7. 🚀 Commitments Before Next 1:1

| Outcome | Due Date |
|---------|----------|
| [outcome 1] | [date] |
| [outcome 2] | [date] |

---

### 8. 🧭 Growth & Ownership

**Growth focus:** [focus area]
```

## Step 5: Final Summary

After writing the file, present a brief summary:

```
✅ 1:1 prep saved to ~/Documents/OneOnOnes/[DATE]_1on1_prep.md

Summary:
- Priorities: [count] items
- Blockers: [count] items
- Decisions needed: [count] items
- Commitments: [count] items with due dates
```

If any section was skipped, note it:
> Sections skipped: [list]. You can fill these in later or revisit with "prepare for my 1:1" again.

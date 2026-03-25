# OneOnOnePrep Preferences
#
# Copy this file to one of the following locations and fill in your values:
#   ~/.claude/PAI/USER/SKILLCUSTOMIZATIONS/OneOnOnePrep/PREFERENCES.md  (PAI users)
#   ~/.config/OneOnOnePrep/PREFERENCES.md                                (everyone else)
#
# Or just run "setup 1:1 prep" and the skill will generate this for you interactively.

## Meeting

# How long is your 1:1? The skill paces sections to fit this target.
- **Duration target:** 20 minutes

# How often do you meet?
- **Cadence:** Weekly

## Goals Document

# Where do your goals live? The skill fetches them to thread goal tags through your prep.
# Supported sources: Google Doc, Confluence, local markdown file, or none.
#
# --- Option A: Google Doc ---
# - **Source:** Google Doc
# - **Document ID:** YOUR_GOOGLE_DOC_ID_HERE
# - **Google account:** YOUR_ACCOUNT_NAME_HERE
# - **Format:** markdown
#
# --- Option B: Confluence ---
# - **Source:** Confluence
# - **Page URL:** https://your-domain.atlassian.net/wiki/spaces/YOUR_SPACE/pages/YOUR_PAGE_ID
#
# --- Option C: Local file ---
# - **Source:** Local
# - **File path:** ~/Documents/my-goals.md
#
# --- Option D: No goals ---
# - **Source:** none
# (Goals threading and Goals Pulse section will be skipped)

- **Source:** none

# Short names for your goals. Used for tagging priorities, wins, and commitments.
# Add as many as you have. The skill uses these as (G1), (G2), etc.
#
# - **Goal short names:**
#   - G1: Your first goal (short description)
#   - G2: Your second goal (short description)
#   - G3: Your third goal (short description)

## Goals Pulse

# How often to show the full goals pulse check table.
- **Frequency:** weekly

# Labels used for goal momentum status.
- **Momentum labels:** Strong / Steady / Seeding / Stalled / Not started

## Output

# Output style for the prep document.
- **Style:** Tight, scannable bullets. No prose paragraphs.

# Tag priorities, wins, and commitments with goal numbers like (G1), (G2).
# Only applies if goals are configured above.
- **Goal tagging:** Tag priorities, wins, and commitments with (G#) where applicable.

# Allow marking commitments as aspirational stretch goals.
- **Stretch commitments:** Allowed, marked with *Stretch:* prefix.

## Context Sources

# The skill attempts to gather work context from these sources during setup.
# It skips silently if any are unavailable. Remove lines you don't use.
Attempt these during setup, skip silently if unavailable:
- Git logs (7 days) in active project directories
- Recently viewed/modified files in workspace
- Jira/Atlassian MCP
- Slack MCP
- Google Docs/Drive MCP

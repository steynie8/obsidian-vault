---
name: weekly-review
description: Run Matthew's weekly review of the vault — inbox status, project health, open tasks, and a written Weekly Review note. Use when the user says "weekly review", "review my week", or "what's stuck".
---

# Weekly Review

## Steps
1. **Inbox:** count the notes in `Inbox/`. If any, offer to run `triage-inbox` first.
2. **Daily notes:** read the last 7 days in `Daily/`. Pull out captures that never got filed, and decisions made.
3. **Projects:** for each `status: active` project, check its last modified date and open `- [ ]` tasks. Flag:
   - Projects untouched for 14+ days (suggest `paused`?)
   - Projects with no next action
   - Stale notes flagged "possibly stale"
4. **Open tasks:** list the top open TODOs per project/area (max 5 each).
5. **Write the note** at `Daily/YYYY/YYYY-[W]WW Weekly Review.md`, following `Templates/Weekly Review.md`, with every field filled in. Lead with the most uncomfortable finding.
6. Propose status changes or archive moves as a list. **Apply only after approval.**
7. Commit: `git commit -m "Weekly review YYYY-Www"`.

Format: bullets and tables, short sentences, SA English.

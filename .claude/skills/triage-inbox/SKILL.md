---
name: triage-inbox
description: Sort notes in the vault's Inbox into PARA folders with correct frontmatter. Use when the user says "triage my inbox", "clean up the inbox", "file my notes", or during a weekly review.
---

# Triage Inbox

## Steps
1. List every note in `Inbox/` except `Inbox/Inbox.md`. For subfolders such as `Inbox/Notion Import/`, use the `notion-import-cleanup` skill instead.
2. Read each note. For each one, propose:
   - **Verdict:** do now / task / read later / reference / delete
   - **Destination:** an existing project, area or resource folder. Check `Projects.md`, `Areas.md` and `Resources.md` for the live lists.
   - **Frontmatter fixes:** `type`, `status`, `tags`, `updated`, plus `area` / `project` links
   - **Links:** existing notes it should link to
3. Show the proposals as one table: Note | Verdict | Destination | Changes.
4. **Wait for approval.** Never move or delete without it. "Delete" means move to `Archive/`.
5. On approval, apply the changes: fix the frontmatter, move with `git mv`, add links. Add any `- [ ]` tasks to the target project/area note if the user wants.
6. Commit: `git add -A` then `git commit -m "Triage inbox: N notes filed"`.
7. Report what moved where, in a short table.

---
name: notion-import-cleanup
description: Normalise notes imported from Notion (via the Obsidian Importer into Inbox/Notion Import) and propose PARA destinations. Use when the user says "clean up the Notion import" or after a Notion import.
---

# Notion Import Cleanup

## Steps
1. Inventory `Inbox/Notion Import/`: note count, folder tree, attachments, and any database CSVs or `.base` files. Report a summary table before changing anything.
2. **Commit a checkpoint first:** `git add -A` then `git commit -m "Raw Notion import"`, so every cleanup step can be diffed and reverted.
3. Normalise, in batches of about 20 notes, showing a sample diff for the first batch:
   - Add or merge YAML frontmatter to the vault schema (`type`, `status`, `tags`, `updated`). Map Notion properties to frontmatter rather than dropping them.
   - Strip Notion ID suffixes from file names (e.g. `Page 3f2a…`) if the Importer left any, and update links to match.
   - Move images and files into `Attachments/`, keeping embeds working.
   - Convert leftover Notion-style links into `[[wikilinks]]`.
4. **Check for duplicates:** flag notes that overlap with existing vault notes (e.g. renovation, AppShell, smart home). Propose merging, don't merge silently.
5. **Propose destinations:** one table of Note | Proposed folder | Reason. Group by PARA. Old or finished material → `Archive/`.
6. **Wait for approval**, then move with `git mv` and commit per batch.
7. Never touch secrets. If you find API keys, passwords or account numbers, flag them to Matthew and don't copy them anywhere.

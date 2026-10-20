# CLAUDE.md: Vault Conventions

Matthew's personal Obsidian vault and the knowledge layer of his AI operating system. It uses PARA plus Inbox and Attachments, is version-controlled with git, and syncs to his desktop PC through a private remote (to be added). Read [[About Me]] for who Matthew is.

## Folder map

| Folder | Meaning | Rule |
|---|---|---|
| `Inbox/` | Quick capture, raw imports | Everything new lands here (future Telegram bot target). Triage weekly. |
| `Projects/` | Things with an end state | One folder per project. Main note = MOC named after the project. |
| `Areas/` | Ongoing responsibilities, no end date | One folder per area with an index note. |
| `Resources/` | Reference material and learnings | Reusable knowledge, not tied to a commitment. |
| `Archive/` | Done, dropped or inactive items | Move whole folders here. Never delete. |
| `Attachments/` | Images, PDFs, spreadsheets | Embed with `![[file]]`. |
| `Daily/` | Daily and weekly notes | `Daily/YYYY/YYYY-MM-DD.md` |
| `Templates/` | Templater templates | No real notes here. |
| `_meta/` | Vault docs: handoff, build log, guide, [[About Me]] | Housekeeping only. |

PARA folders and their subfolders each have an index/MOC note named after the folder. Before filing anything, check the live lists in [[Projects]], [[Areas]] and [[Resources]].

### Where does a note go?
- Serves an active project? → that project's folder.
- Ongoing responsibility? → that area's folder.
- Reusable reference? → `Resources/<topic>/`.
- Unsure? → leave it in `Inbox/` and ask.

## Frontmatter schema

Every note starts with:

```yaml
---
type: project | area | resource | moc | archive | daily | meeting | decision | supplier | prompt
status: active | paused | done | archived
tags: []
updated: 2026-10-20
---
```

Optional fields: `area: "[[Area note]]"` and `project: "[[Project note]]"` to link a note to its parent.

**Tags in use:** `#project` `#area` `#resource` `#decision` `#todo` `#supplier` `#sars` `#body-corporate` `#appshell` `#renovation` `#smart-home` `#claude`. Reuse these before adding new ones.

## Note conventions
- Use `[[wikilinks]]`, never markdown links, for vault notes. Link related notes liberally.
- Note names must be unique across the vault, because wikilinks resolve by name. Prefix generic names (`AppShell Learnings`, not `Learnings`).
- Open items are tasks: `- [ ] TODO: …`. The **Tasks** plugin collects them on [[Home]].
- Unknown facts get a `- [ ] TODO:` line. Never guess.
- Update `updated:` whenever you materially edit a note.
- **Never store secrets**: no API keys, passwords, tokens or account numbers. Secret *names* (e.g. `VERCEL_TOKEN`) are fine.

## Matthew's working preferences
- South African English spelling, ZAR (R), metric units.
- Instructions as bullets and tables, not numbered prose paragraphs.
- Explain a technical concept in plain language before the action steps.
- For ambiguous requests, ask one batch of at most 3 clarifying questions.
- Put uncomfortable conclusions in the opening line.
- When disagreeing, name the alternative and its specific downside.
- Debugging: theory before fix, root cause over symptom, surgical changes.
- No inline citation tags. Name sources in plain text or a Sources list.
- Short sentences. Bullets and tables over long prose.

## Rules for Claude in this vault
- **Never delete or bulk-move notes without first listing them and getting approval.**
- Search before creating a note, and extend an existing note rather than duplicating it.
- When creating a note from a template, fill in the values yourself and leave no raw `<% %>` Templater tags.
- Don't touch `.obsidian/` or `.handwriting/` unless asked. `Untitled.md` is a Handwriting plugin page, so leave it alone.
- Renaming a note breaks links unless you update every `[[Old Name` reference too. Moving a note between folders is fine.
- Git: small, descriptive commits, only when Matthew asks or a skill says so. Never force-push.
- Vault content is private (tax, property, body corporate). Don't send it to external services unless asked.

## Claude integration
- `.claude/skills/` holds the vault skills: `triage-inbox`, `new-project`, `weekly-review`, `notion-import-cleanup`.
- [[Claude Projects]] maps claude.ai Projects to the vault notes that feed them.
- [[Claude Setup]] and [[Dev Environment]] document Matthew's Claude tooling.
- [[Prompt Library]] holds reusable prompts.

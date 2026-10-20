---
type: resource
status: active
tags: [claude]
updated: 2026-10-20
---
# Vault Guide

How this vault works and how to get the most from it.

## The idea in one line
**Capture fast into Inbox, organise weekly into PARA.** The weekly review is what keeps the system alive.

## Structure
| Folder | Use |
|---|---|
| `Inbox/` | Everything new lands here |
| `Projects/` | Things with a finish line, one folder each |
| `Areas/` | Ongoing responsibilities |
| `Resources/` | Reusable reference |
| `Archive/` | Finished or inactive (move, never delete) |
| `Attachments/` | Pasted images, PDFs, spreadsheets |
| `Daily/` | Daily notes |
| `Templates/` | Note templates |
| `_meta/` | Vault docs (this guide, handoff, build log, [[About Me]]) |

## One-time Obsidian setup
I didn't edit `.obsidian/`, so these settings are yours to set.

### Plugins to install
*Settings → Community plugins → Browse*

| Plugin | Why | Status |
|---|---|---|
| Git (Vinzent) | Auto-commit / sync | ✅ installed |
| Tasks | Powers "Next actions" on [[Home]] | ✅ installed |
| Handwriting | Your handwriting pages | ✅ installed |
| **Templater** | Powers every template in `Templates/` | ⬜ install |
| **Dataview** | Auto-lists on project / area / weekly notes | ⬜ install |
| **Importer** (official) | Notion import | ⬜ install |
| Homepage (optional) | Opens [[Home]] on launch | ⬜ optional |

### Settings
| Where | Setting | Value |
|---|---|---|
| Files and links | Default location for new notes | In the folder specified below → `Inbox` |
| Files and links | Default location for new attachments | In the folder specified below → `Attachments` |
| Files and links | New link format | Shortest path when possible |
| Files and links | Use `[[Wikilinks]]` | On |
| Files and links | Automatically update internal links | On |
| Daily notes | New file location | `Daily` |
| Daily notes | Date format | `YYYY/YYYY-MM-DD` (makes a folder per year) |
| Daily notes | Template file location | `Templates/Daily Note` |
| Core plugins | Templates | **Off** (Templater replaces it) |
| Core plugins | Sync | **Off**, unless you pay for Obsidian Sync. Git is your sync. |
| Templater | Template folder location | `Templates` |
| Templater | Trigger Templater on new file creation | On |
| Git | Auto commit-and-sync interval | `30` minutes |
| Git | Disable push | On, until a remote exists |

## Everyday workflows
| I want to… | Do this |
|---|---|
| Start my day | `Ctrl+P` → *Open today's daily note* |
| Capture an idea | `Ctrl+N` (lands in Inbox) |
| New note from template | `Ctrl+P` → *Templater: Create new note from template* |
| Start a project | Claude Code: "create a new project for X" |
| Weekly review | Claude Code: "run my weekly review" |
| Clean up Inbox | Claude Code: "triage my inbox" |

## Working with Claude
| Surface | How it uses the vault |
|---|---|
| **Claude Code** (desktop app, folder `C:\vault`) | Reads `CLAUDE.md` automatically, so it knows the structure, schema and your preferences. Skills in `.claude/skills/`. |
| **claude.ai Projects** | Upload vault notes as Project knowledge. Track it in [[Claude Projects]]. Always include [[About Me]]. |
| **Bringing chats back** | End a valuable chat with "write a markdown handoff doc", save it to `Inbox/`, then triage. |

## Importing from Notion
| Step | Action |
|---|---|
| 1 | Notion → *Settings → Export* → **HTML**, include subpages, everything |
| 2 | Obsidian → **Importer** → Notion → pick the zip → output folder `Inbox/Notion Import` |
| 3 | Claude Code: "clean up the Notion import". It normalises the notes and proposes PARA destinations for your approval. |

## Syncing with the desktop PC (later)
| Step | Where | Action |
|---|---|---|
| 1 | GitHub | Create a **private** repo, e.g. `steynie8/vault` (no README) |
| 2 | Laptop | `git remote add origin <url>` → `git push -u origin main` |
| 3 | Laptop | Git plugin: Disable push **off**, Pull on startup **on** |
| 4 | Desktop | Install Git → `git clone <url> C:\vault` → open as vault → enable plugins (they come down with the repo) |
| 5 | Habit | Pull when you start, push when you finish. The plugin can do both automatically. |

---
type: resource
status: done
tags: []
updated: 2026-10-20
---
# Build Log

## 2026-10-20: initial build (Claude Code)
Built from [[VAULT-HANDOFF]] plus Matthew's answers in the session.

### Decisions that differ from the handoff
| Handoff said | Built as | Why |
|---|---|---|
| Numbered folders (`00 Inbox`, `01 Projects`…) | Plain names (`Inbox`, `Projects`…) | Matthew chose plain names |
| `AppShell/Learnings.md` | `AppShell Learnings.md` | Avoids a name clash with the `Learnings` MOC (wikilinks resolve by name) |
| Renovation: Cape Town or J-Bay? | J-Bay flat | Confirmed by Matthew |
| Side-project status unknown | Party Game + Wildlife Reserve Game = `paused`; others `active` | Confirmed by Matthew |
| Folder MOC per folder | MOCs for PARA folders + subfolders; none for `_meta`, `Templates`, `Attachments`, `Daily` | System folders |

### Created
| Area | Items |
|---|---|
| Root | `Home.md` (dashboard + Tasks "Next actions"), `CLAUDE.md`, `.gitignore`, `.gitattributes` |
| Projects | AppShell (6 notes), Fuelzy, Wildlife Reserve Game, Party Game, Instagram Profile Tools, Flat Renovation (6 notes), `Projects.md` |
| Areas | J-Bay Flat (3), Kingston Mews (3), Smart Home (3), Dev Environment & Claude Tooling (5, incl. [[Claude Projects]]), Work, Car, Fitness, `Areas.md` |
| Resources | Investment Research (3), Home Electrical (2), Learnings (5), Suppliers & Contacts (13 + MOC), Prompt Library, `Resources.md` |
| Archive | Knysna Property (Malachite Mews), `Archive.md` |
| Templates | Project, Area, Meeting or Call, Supplier Quote, Daily Note, Decision Record, Resource, Prompt, Weekly Review (Templater syntax) |
| `_meta` | [[VAULT-HANDOFF]], this log, [[Vault Guide]], [[About Me]] |
| `.claude/skills` | triage-inbox, new-project, weekly-review, notion-import-cleanup |
| Git | Repo initialised on `main`, initial commit |

### Not touched
- `.obsidian/`, `.handwriting/`, `Untitled.md` (a Handwriting plugin page)

### Still open (from handoff §7)
- [ ] TODO: Confirm AppShell current state (see [[Current State]])
- [ ] TODO: Decide whether work notes belong in this vault (see [[Work]])

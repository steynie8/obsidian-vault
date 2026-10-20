---
type: area
status: active
area: "[[Dev Environment]]"
tags: [claude]
updated: 2026-10-20
---
# Claude Setup

## Plugins & mods
| Item | What it does | Notes |
|---|---|---|
| Context-bar mod | Claude Code plugin; hooks draw into the AbovePrompt band | CLI + desktop Code tab only |
| Sensitive-data-indicator plugin | Warns when Gmail tools are present or called | Real tool names still to confirm |
| `webcrawler.skill` | Web crawling skill | Installed across surfaces |

- [ ] TODO: Confirm the real Gmail tool names for the sensitive-data-indicator plugin

## Custom instructions
Six sections: Behaviour · Formatting · Tone · Thinking · Debugging · Context. The working preferences are mirrored in the vault's `CLAUDE.md`.

## Memory pruning
- Taxonomy: **KEEP** vs **ARCHIVE**.
- Prompt lives at `~/.claude/prune-memory.md`.
- Plan: prune manually for a few weeks before automating.

## Vault integration
- This vault has its own `CLAUDE.md` and skills in `.claude/skills/`. See [[Vault Guide]].

Back to [[Dev Environment]] · See also [[Agent Workflow]]

---
type: area
status: active
area: "[[Dev Environment]]"
tags: [claude]
updated: 2026-10-20
---
# Design System

## Look
| Token | Choice |
|---|---|
| Accent | Aubergine |
| Mode | Dark-first |
| Voice serif | Fraunces |
| UI font | Inter |
| Neutrals | Warm paper |

## Files
`ds-tokens.css` · `ds-tailwind.css` · `ds-tokens.json` · `demo.html` · `README.md`

## Wiring
| Layer | What it does |
|---|---|
| Global `~/.claude/CLAUDE.md` | Points to the rules file |
| `.claude/rules/design-system.md` | Design rules for Claude |
| `check-design-tokens.mjs` | Enforces token use (`/* ds-allow */` escape hatch) |
| `sync-design-system.sh` | Vendors the files and pins the SHA in `.ds-version` |

- Vendoring was chosen over npm because of the MV3 constraint (see [[Instagram Profile Tools]]).

## Open
- [ ] TODO: Lock the `--ds-` prefix

Back to [[Dev Environment]]

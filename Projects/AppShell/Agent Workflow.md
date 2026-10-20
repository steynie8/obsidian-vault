---
type: resource
status: active
project: "[[AppShell]]"
tags: [appshell, claude]
updated: 2026-10-20
---
# Agent Workflow

| Step | Who | What |
|---|---|---|
| 1 | Claude Code | Reads `HANDOFF.md`, `ARCHITECTURE.md`, `MASTER-PROMPT.md`, `CONVENTIONS.md` before writing anything |
| 2 | Claude Code | Works one epic or phase per session; never silently diverges from `ARCHITECTURE.md` |
| 3 | Claude Code | Updates `HANDOFF.md` as its final act |
| 4 | Matthew + Claude | Review the PR diff → flag / approve / escalate |
| 5 | Matthew + Claude | Draft a refined prompt for the next session |

Back to [[AppShell]] · See also [[Claude Setup]]

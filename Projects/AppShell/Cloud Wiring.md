---
type: resource
status: active
project: "[[AppShell]]"
tags: [appshell]
updated: 2026-10-20
---
# Cloud Wiring

## Supabase
| Environment | Project ref |
|---|---|
| Production | `dmssvkcbpknymicrjktq` |
| Staging | `jmakgjaecutgnakrenbf` |

## Vercel
- Production scope → prod Supabase
- Preview scope → staging Supabase
- Marketplace integration disconnected
- Git auto-deploy off (Ignored Build Step = don't build)

## GitHub
- Environments `staging` and `production`, each holding `SUPABASE_DB_URL` and `APP_URL`
- Repo secrets (names only): `VERCEL_TOKEN`, `ORG_ID`, `PROJECT_ID`
- Deploy gate: `DEPLOY_ENABLED=true`

## Gotchas
- GitHub runners lack IPv6 → use the **IPv4 session pooler** connection string (not the transaction pooler) for migrations.
- `webcrawler.skill` must be committed to `.claude/skills/` **in the repo**, not `~/.claude/`, because cloud sessions clone the repo.

Back to [[AppShell]]

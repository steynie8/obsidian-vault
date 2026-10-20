---
type: decision
status: active
project: "[[AppShell]]"
tags: [appshell, decision]
updated: 2026-10-20
---
# Architecture Decisions

| # | Topic | Decision |
|---|---|---|
| D1 | Auth | One auth user per tenant (v1) |
| D2 | Tenant provisioning | Operator-provisioned only |
| D3 | End users | Invite-only, per-tenant toggle; self-signup behind a feature flag |
| D4 | Email | Resend; v1 sender = tenant name over platform domain |
| D5 | Reference applet 2 | Directory & Bookings-lite |
| D6 | Chat models | Haiku (economy) / Sonnet (default) / Opus (off by default) |
| D7 | Rate limits | 30 msgs/user/hr; 1M tokens/tenant/day |
| D8 | Transcripts | User-private; tenant admins cannot read |
| D9 | Billing | Out of scope for v1 |
| D10 | Locales | en-ZA/ZAR default + en-GB/GBP, en-US/USD, EUR |
| — | Custom domains | Deferred to Epic 8; `*.localhost` for local verification through Epic 7 |
| — | Domain purchase | Deferred until Epic 4 or first external demo |
| — | Suspension policy | Fix-forward migration extending suspension predicates (retroactively approved) |

## Epic order
- Settings engine → Theming → Entitlements (ships inside Epic 5 with applet runtime) → Custom domains (E8)
- Isolation test harness grows from Epic 1 onward

> [!warning] Hard rule
> RLS policies and isolation tests land **in the same commit** as each new table. No deferrals.

Back to [[AppShell]]

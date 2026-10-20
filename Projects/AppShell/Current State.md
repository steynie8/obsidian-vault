---
type: project
status: active
project: "[[AppShell]]"
tags: [appshell]
updated: 2026-10-20
---
# Current State

> [!warning] Possibly stale
> Last known state is from about September 2026. Epic 1 Phase 3 may already be done.

- [ ] TODO: Confirm current AppShell state with Matthew (is Phase 3 done?)

## Done
| Item | Detail |
|---|---|
| Epic 0 | Deploy pipeline green end-to-end |
| Epic 1 Phase 1 (PR #3) | Schema, Custom Access Token Hook, tenants/profiles/invitations tables with RLS, profile-mirror trigger, seed personas, 27 isolation tests passing |
| Epic 1 Phase 2 (PR #4) | Tenant-resolution middleware, Supabase SSR sessions, login/logout/password reset, service-role admin module with CI grep enforcement, provisioning script |

Both PRs approved.

## Open
- [ ] TODO: Fix finding (e): `supabaseAnonEnv()` lacks validation. A malformed `NEXT_PUBLIC_SUPABASE_URL` makes `createServerClient` throw before the graceful-degrade catch. Fix = validate inside `supabaseAnonEnv()`, throw a clear named error, add unit tests.
- [ ] TODO: Fix ARCHITECTURE.md doc drift in §5.2 and §5.3

## Next
- [ ] TODO: Epic 1 Phase 3: invitations, self-signup behind flag, role change, deactivation with forced sign-out (prompt already drafted)
- Then: Settings engine epic

Back to [[AppShell]]

---
type: resource
status: active
project: "[[AppShell]]"
tags: [appshell]
updated: 2026-10-20
---
# AppShell Learnings

- RLS and its tests must be atomic per table.
- Approved PRs can still leave spec docs stale. Reviews should check that spec and code agree.
- Validate env vars at the earliest point. Errors thrown before a catch boundary bypass fallbacks.
- Deferring decisions (domain, billing) was the right call.

See also [[Learnings]] · Back to [[AppShell]]

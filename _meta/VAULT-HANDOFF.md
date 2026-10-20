# Vault Handoff — Build Matthew's Obsidian Vault

> **For Claude Code.** Read this whole file before creating anything. It describes who the vault is for, the folder structure to build, and the seed content for each note. Everything below comes from Matthew's previous Claude sessions (as of 20 Oct 2026). Some of it may be stale — see **§7 Verify before trusting**.

---

## 1. Who this is for

| | |
|---|---|
| **Owner** | Matthew — solo developer / "vibe coder", also employed (has a manager at work) |
| **Location** | Gqeberha / Jeffreys Bay (J-Bay), Eastern Cape, South Africa |
| **Locale** | South African English spelling, ZAR (R), metric units |
| **Platform** | Windows; vault lives at `C:\vault` |
| **Why a vault** | He's previously designed a capture-and-review system (mobile capture → desktop review). Preferred stack was Telegram bot → Obsidian vault, synced via Syncthing. This vault is likely the destination for that. |

---

## 2. Rules for building the vault

- **Do not touch** the existing `.obsidian/` or `.handwriting/` folders. Leave `Untitled.md` alone (ask Matthew if it should be deleted).
- **Do not invent facts.** Only write what's in this handoff. Where something is unknown, leave a `- [ ] TODO:` line rather than guessing.
- Every note gets YAML frontmatter:
  ```yaml
  ---
  type: project | area | resource | moc | archive
  status: active | paused | done | archived
  tags: []
  updated: 2026-10-20
  ---
  ```
- Link related notes with `[[wikilinks]]`. Each folder gets an index/MOC note named after the folder.
- Formatting: short sentences, bullet points and tables over long prose (Matthew's stated preference).
- Keep secrets out. No API keys, passwords, tokens or account numbers — ever.
- When finished, move this file to `_meta/VAULT-HANDOFF.md` and add a short `_meta/build-log.md` noting what was created.

---

## 3. Folder structure (PARA-style)

```
C:\vault
├── 00 Inbox/                      ← quick capture lands here (Telegram bot target later)
├── 01 Projects/                   ← things with an end state
│   ├── AppShell/
│   ├── Fuelzy/
│   ├── Wildlife Reserve Game/
│   ├── Party Game/
│   ├── Instagram Profile Tools/
│   └── Flat Renovation/
├── 02 Areas/                      ← ongoing responsibilities, no end date
│   ├── J-Bay Flat (Rental & Tax)/
│   ├── Kingston Mews Body Corporate/
│   ├── Smart Home/
│   ├── Dev Environment & Claude Tooling/
│   ├── Work/
│   ├── Car/
│   └── Fitness/
├── 03 Resources/                  ← reference material and learnings
│   ├── Investment Research/
│   ├── Home Electrical/
│   ├── Learnings/
│   └── Suppliers & Contacts/
├── 04 Archive/
│   └── Knysna Property (Malachite Mews)/
├── _meta/
├── Templates/
├── Home.md                        ← dashboard / top-level MOC
└── CLAUDE.md                      ← conventions for future Claude Code sessions in this vault
```

---

## 4. Root files

### `Home.md`
- Links to every project and area MOC.
- Sections: **Active projects**, **Areas**, **Resources**, **Inbox** (link to `00 Inbox`).
- Add a "Next actions" section that pulls the open items from §5 (one line each, linked to the source note).

### `CLAUDE.md` (vault conventions for future sessions)
Include:
- Folder meanings (PARA, as above) and the frontmatter schema.
- Matthew's working preferences:
  - South African English, ZAR, metric
  - Instruction steps as bullets and tables, not numbered prose paragraphs
  - Plain-language explanation of a technical concept before action steps
  - For ambiguous requests, ask one batch of ≤3 clarifying questions
  - Put uncomfortable conclusions in the opening line
  - When disagreeing: name the alternative + its specific downside
  - Debugging: theory before fix, root cause over symptom, surgical changes
  - No inline citation tags — name sources in plain text or a Sources list
- Rule: never delete or bulk-move notes without listing them first and getting approval.

### `Templates/`
Create: `Project.md`, `Area.md`, `Meeting or Call.md`, `Supplier Quote.md`, `Daily Note.md`, `Decision Record.md` (fields: Decision, Date, Context, Options considered, Outcome).

---

## 5. Seed content per note

### 01 Projects

#### AppShell/ — `AppShell.md` (MOC) + `Architecture Decisions.md` + `Cloud Wiring.md` + `Current State.md` + `Learnings.md` + `Agent Workflow.md`

**Overview**
- White-label multi-tenant SaaS framework — a reusable foundation for future products
- Repo: `github.com/steynie8/saas-appshell`; placeholder live at `saas-appshell.vercel.app`
- Customers: non-technical tenant admins who white-label it for their end users
- Stack: Next.js (App Router) on Vercel · Supabase (Postgres + RLS for tenant isolation) · Anthropic API chat widget · applet plugin system · Resend email
- Governing docs in repo: `MASTER-PROMPT.md`, `ARCHITECTURE.md`, `EPICS.md` (E0–E9), `CONVENTIONS.md`, `HANDOFF.md`

**Architecture Decisions** (table)

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
| — | Suspension policy | Fix-forward migration extending suspension predicates — retroactively approved |

**Epic order:** Settings engine → Theming → Entitlements (ship inside Epic 5 with applet runtime) → Custom domains (E8). Isolation test harness grows from Epic 1 onward. **Hard rule:** RLS policies + isolation tests land in the same commit as each new table — no deferrals.

**Cloud Wiring**
- Supabase: production `dmssvkcbpknymicrjktq`, staging `jmakgjaecutgnakrenbf`
- Vercel: Production scope → prod Supabase; Preview → staging. Marketplace integration disconnected; Git auto-deploy off (Ignored Build Step = don't build)
- GitHub Environments `staging` + `production`, each holding `SUPABASE_DB_URL` and `APP_URL`
- Repo secrets (names only): `VERCEL_TOKEN`, `ORG_ID`, `PROJECT_ID`; deploy gate `DEPLOY_ENABLED=true`
- GitHub runners lack IPv6 → use **IPv4 session pooler** strings (not transaction pooler) for migrations
- `webcrawler.skill` must be committed to `.claude/skills/` in the repo (not `~/.claude/`) because cloud sessions clone the repo

**Current State** (last known ~Sept 2026 — flag as possibly stale)
- Done: Epic 0 (deploy pipeline green end-to-end); Epic 1 Phase 1 (PR #3 — schema, Custom Access Token Hook, tenants/profiles/invitations tables with RLS, profile-mirror trigger, seed personas, 27 isolation tests passing); Epic 1 Phase 2 (PR #4 — tenant-resolution middleware, Supabase SSR sessions, login/logout/password-reset, service-role admin module with CI grep enforcement, provisioning script). Both PRs approved.
- Open bug (finding e): `supabaseAnonEnv()` lacks validation — malformed `NEXT_PUBLIC_SUPABASE_URL` makes `createServerClient` throw before the graceful-degrade catch. Fix = validate inside `supabaseAnonEnv()`, clear named error, unit tests.
- Open: ARCHITECTURE.md doc drift in §5.2 and §5.3
- Next: Epic 1 Phase 3 (invitations, self-signup behind flag, role change, deactivation with forced sign-out) — prompt already drafted
- Then: Settings engine epic

**Learnings**
- RLS + tests must be atomic per table
- Approved PRs can still leave spec docs stale — review should check spec/code alignment
- Validate env vars at the earliest point; errors thrown before a catch boundary bypass fallbacks
- Deferring decisions (domain, billing) was the right call

**Agent Workflow**
- Claude Code reads HANDOFF / ARCHITECTURE / MASTER-PROMPT / CONVENTIONS before writing anything
- One epic or phase per session; updates `HANDOFF.md` as final act; never silently diverges from ARCHITECTURE.md
- Matthew reviews the PR diff with Claude → flags/approves/escalates → refined prompt drafted for next session

#### Fuelzy/ — `Fuelzy.md`
- SA fuel-tracking app on AWS Amplify
- Till-slip scanning via AWS Textract AnalyzeExpense; DynamoDB; diesel price comparison (diesel is deregulated in SA)
- Five-table data model; phased agent brief
- Open decisions: crowdsourced vs private price data; MVP platform; monetisation
- Mockup review issues: currency shown as £/GBP (should be R/ZAR); no nav slot for price comparison

#### Wildlife Reserve Game/ — `Wildlife Reserve Game.md`
- SimSafari-inspired 3D browser game in Three.js
- Ecosystem sim (predator / prey / vegetation) + tourism business (lodges, tracks, staff)
- African savanna setting, ZAR currency
- Seven sequential epic prompt docs with a `HANDOFF.md` continuity pattern
- Hosting: Vercel Hobby ($0/month) — static Vite build + localStorage
- Notes on token-conscious sessions and mid-epic resume strategy

#### Party Game/ — `Party Game.md`
- Browser multiplayer party game, skribbl.io-style
- Cheap hosting via Cloudflare Workers / Durable Objects
- Ad monetisation (AdinPlay / Venatus) is a core design constraint

#### Instagram Profile Tools/ — `Instagram Profile Tools.md`
- Chrome extension: Manifest V3, vanilla JS, no build step
- Lemon Squeezy subscriptions: Free / Basic / Pro
- Differentiator: shows mean vs median engagement rate
- Sizzle reel generated with Python / Pillow / NumPy / ffmpeg (1080×1920, 29 s)
- Risks: fail-open licence validation; Instagram ToS + data-privacy exposure from lead-extraction feature

#### Flat Renovation/ — `Flat Renovation.md` (MOC) + `Tiles.md` + `Kitchen & Joinery.md` + `Gas (LPG).md` + `Contractor.md` + `Open Decisions.md`
> ⚠️ Previous notes say "Cape Town flat", but the project was filed under the J-Bay flat and the joinery/stone suppliers are in Jeffreys Bay. **Ask Matthew which property this is before naming the note** (see §7).

**Overview:** Short-term rental aimed at couples. Full tile, kitchen and fit-out upgrade (flooring, bathroom/shower, kitchen).

**Tiles** (table)

| Product | Code | Use | Qty |
|---|---|---|---|
| Ice White 600×600 rectified porcelain | Ceratile RG10030 | All floors, bathroom walls to ~2600 mm, shower kerb | ~80.64 m² / 56 boxes |
| Terrazzo Kit Kat Matt White/Green mosaic | Ceratile GS-ST80 | Shower feature wall | ~2.69 m² / 36 sheets |
| Artistic Blanco Matt Glazed Ceramic 75×300 | Italtile ELB1005 | Kitchen backsplash (620 mm band × 3.54 m, 8 courses, 2.5 mm joints) | ~2.75 m² |

- Shower floor: Ice White cut to ~292×50 mm strips; grip confirmed by showroom test; cement grout, 3 mm joints (walls 2 mm)
- Shower kerb: brick-on-edge, ~100 mm wide × 130 mm high unplastered
- Best supplier combo: Cape Tiles (Ice White + backsplash) + Floor and Decor (mosaic) ≈ **R19,452**
- Floor plan source of truth: earlier confirmed plan (shower 1.76 m², bathroom 2.43 m²)
- Workbooks: `Shower_Tile_Comparison.xlsx`, `Ice_White_Tile_Costing.xlsx`
- TODOs: Cape Tiles mosaic quote (single-supplier option) · confirm rectified edges + R9 slip rating in writing · confirm single batch for all 56 boxes · clarify Cape Tiles backsplash per-m² vs box price · get return policies in writing · lock bathroom ceiling height (2200 vs 2600 mm, ~2 boxes difference)

**Kitchen & Joinery**
- Window-wall run ~3540 mm (dishwasher + front-load washer under); island ~2540 mm (oven + 4-plate hob); 900 mm counter height
- Island cabinets 600 mm deep with ~250 mm stool overhang
- Woody's (Port Elizabeth) quote QW26634: Innovus Silhouette Sahara fronts + Caesarstone tops
- Tops preference: Eezi Quartz (cheaper) over Caesarstone; tops quoted separately — The Stone Collection @ Barn-Find, J-Bay (Erik Spies)
- Extra joinery: bedroom built-ins (~2000 × 570 mm) + ~900 × 550 mm vanity/dressing unit; ~600 mm bathroom vanity — all Sahara, same stone top
- Getting multiple joinery quotes with on-site measuring in J-Bay
- Barn door under consideration for bathroom

**Gas (LPG)**
- Existing stove circuit ends on a separate counter; extending it isn't viable
- Proposal: LPG bottle in island cupboard + Elba 60 cm built-in gas oven (04-TS 600FG, House and Home); standard socket near island for ignition
- Reference: thegasman.co.za summary of SANS 10087-1 (2024)
- Before committing: body corporate LPG approval · island extraction (ceiling extractor vs downdraught — downdraught eats cupboard space) · registered gas installer review before joinery is final

**Contractor**
- OMJ Construction (contact: Rassie) — labour-only quote + BOQ: demolition, plaster/skim, plumbing, tiling labour, doors, electrical points; Matthew supplies materials
- TODOs: get company + NHBRC details for trustees · send list of quote points · consider asking him to supply some materials

**Open Decisions**
- Mosaic on opposite shower wall too? (~70 sheets total, ~R10,500 mosaic alone)
- Kitchen tiled to ceiling vs backsplash only (R1,435–R1,625)
- Paint colour review (a separate handoff doc was made previously)

**Ways of working:** hand measurements + floor plans as source of truth · formula-driven Excel with live inputs · detail first, then simplified summary tables · formal A4 quote-request docs (batch supply, returns, project pricing, lead time, price validity) · compare suppliers line by line.

---

### 02 Areas

#### J-Bay Flat (Rental & Tax)/ — `J-Bay Flat.md` + `SARS Rental Register.md` + `Finance Model.md`
- Address: 111 Da Gama Rd, Jeffreys Bay — active property focus
- Registered in Matthew's name 5 Feb 2026 → 2025/26 tax year covers only Feb 2026
- Let to a long-term tenant via a rental agent from Feb 2026 until lease expiry June 2026; no personal use
- Building his own SARS rental expense register (modelled on a CA friend's template); separate register planned for 2026/27
- Tax references: SARS s11(e), s11A
- Finance model: PMT bond formula, STR cash-flow model, break-even calc, room area/flooring estimator
- Workbook restructured from 819 rows / 76 images into 9 tabs: Summary, Flooring, Furniture, Bathroom & Kitchen, Wall Panels, Smart Home & Security, Internet, Listings & Photos, Image Index
- Link to `[[Flat Renovation]]` and `[[Smart Home]]` (door/window sensors)

#### Kingston Mews Body Corporate/ — `Kingston Mews.md` + `Financial Review.md` + `Water Leaks.md`
- 92-unit sectional title scheme in Gqeberha; Matthew is a resident owner involved in governance and financial oversight
- Managing agent: The Bellbuoy Group; insurance: Compass / Auxilium
- Financial review: R182K in-year operating deficit · reserve fund declining toward ~R429K with reduced forward contribution · ~R120K combined adverse water variance
- Water leaks: south section, multi-month history (visible bursts + undetectable underground failures), hillside next to a nature reserve — likely systemic / workmanship-driven
- Recommended diagnostic: bulk-meter vs submeter reconciliation + minimum-night-flow test

#### Smart Home/ — `Smart Home.md` + `Humidity Automation.md` + `Hardware.md`
- Platform: Smart Life / Tuya
- Hardware: Tuya Wi-Fi IR remote with temp/humidity sensor (GeeWiz) · Comfee/Alliance split aircon (Midea-built) · Tuya door/window sensors (~R115 each, GeeWiz) · Anker 20W charger (on sensor) · Hoco N63 30W GaN (on iPhone 13)
- Automations (working end to end): Dry mode >65% RH · off <55% RH · scheduled off 22:30
- Fixed: sensor buzzing caused by ripple from a switch-mode charger → swapped to the Anker
- Open: humidity rose to 68% after door closed — compare temp + RH graphs to tell apart (a) RH rising as temp drops, (b) IR one-way blindness, (c) coastal moisture ingress
- Planned: door/window sensors at the rental; if >~6 sensors, consider Zigbee + hub
- Local buying constraint: GeeWiz, Takealot, BobShop

#### Dev Environment & Claude Tooling/ — `Dev Environment.md` + `Design System.md` + `Claude Setup.md` + `Ideas.md`
- Hardware: AMD RX 7800 XT (16 GB VRAM), 32 GB RAM. Local LLM eval: Qwen3.6-27B UD-Q3_K_XL (~15 GB), llama.cpp on ROCm 6.4.2 (gfx1101), MTP speculative decoding (1.4–2×), bf16 KV-cache fallback flags
- Accounts: personal Claude + work Enterprise (Claude in Chrome disabled on work); wants tooling to work across both
- **Design System:** aubergine accent, dark-first, Fraunces (voice serif) + Inter (UI), warm paper neutrals. Files: `ds-tokens.css`, `ds-tailwind.css`, `ds-tokens.json`, `demo.html`, `README.md`. Wiring: global `~/.claude/CLAUDE.md` pointer → `.claude/rules/design-system.md` → `check-design-tokens.mjs` (`/* ds-allow */` escape hatch); `sync-design-system.sh` vendors files and pins SHA in `.ds-version`. Open: lock the `--ds-` prefix; vendoring chosen over npm (MV3 constraint)
- **Claude Setup:** context-bar mod (Claude Code plugin, hooks drawing into AbovePrompt band; CLI + desktop Code tab only) · sensitive-data-indicator plugin (warns when Gmail tools present/called — real tool names still to confirm) · six-section custom instruction set (Behaviour, Formatting, Tone, Thinking, Debugging, Context) · `webcrawler.skill` installed across surfaces · memory pruning taxonomy KEEP vs ARCHIVE, prompt at `~/.claude/prune-memory.md` (prune manually a few weeks before automating)
- **Ideas:** agent-building path (Pi → Aider → OpenCode → Cline; build a ~150-line tool loop first) · five-advisor debate council (Pre-mortem, Quant, Outside View, Operator, User; blind first rounds; terminal condition = positions stopped moving, not consensus) · capture-and-review system (Telegram → Obsidian via Syncthing / Karakeep / custom Telegram + Supabase + Vercel; triage verdicts: do now / task / read later / reference / delete) · agent to group and rename Claude chats/sessions (maybe with dates)

#### Work/ — `Work.md`
- Employed with a manager; uses Claude across 20+ work repos
- Work notes currently in a multi-tab OneNote (many pages/subpages)
- `- [ ] TODO: Ask Matthew whether work notes should live in this vault at all (work Enterprise account / data policy), and if so, what sub-structure mirrors his OneNote.`

#### Car/ — `Car.md`
- 2012 Opel Astra J GTC 1.6 turbo; does some own maintenance (coolant top-up and bleeding)
- Add a maintenance log table: Date | Km | Work done | Cost (R) | Notes

#### Fitness/ — `Fitness.md`
- Virgin Active; home club Lorraine (Gqeberha); Discovery Vitality discount
- Likes Humewood (used it when Lorraine was closed); office is ~100 m from Humewood
- Oct 2026: leaning toward Lorraine-only rather than three-club upgrade

---

### 03 Resources

- **Investment Research/** — `AI & Robotics Picks-and-Shovels.md`: semis equipment (ASML, AMAT, LRCX, KLA), memory (Micron), optical/networking (Corning, Lumentum, Coherent, Arista), power (Vertiv, Eaton, GE Vernova), connectors (Amphenol), robotics (Ambarella, Cognex, MP Materials, Rockwell, Symbotic), ETFs (SMH, BOTZ, ROBO, IRBO). An interactive table of ~35 securities (10-yr returns, revenue, EPS 2015–2024, CAGR) was built previously. Also `SA Property.md` linking to the J-Bay and Knysna notes.
- **Home Electrical/** — `Bathroom Extractor Fan.md`: adding a fan to an existing bathroom lighting circuit; permanent vs switched live, three-lever switch module wiring, SANS 10142-1, earth leakage, zone IP ratings, CoC; conduit enters switch boxes from the top in SA houses with roof spaces.
- **Learnings/** — `USB Power.md` (current rating is a max, voltage is what must match; charger ripple matters; laptop ports are cleaner) · `Smart Life Automations.md` (use AND not OR; don't mix outdoor-weather conditions with local sensors) · `Humidity & Mould.md` (target 45–55% RH; 60–70% = risk; cold coastal masonry walls are the real risk) · `IR Control.md` (one-way, no feedback).
- **Suppliers & Contacts/** — one note per supplier using the `Supplier Quote` template: Cape Tiles, Floor and Decor, Italtile, CTM, Stiles (site blocks scraping), Ceratile, Woody's, The Stone Collection @ Barn-Find (Erik Spies), House and Home, OMJ Construction (Rassie), GeeWiz, Takealot, BobShop. Contact names only — no phone numbers or emails unless Matthew adds them.

### 04 Archive

- **Knysna Property (Malachite Mews)/** — R1,050,000 listing marketed as "high-potential Airbnb"; set aside because scheme Rule 10.2 limits non-permanent occupation to two weeks/year.

---

## 6. Suggested tags

`#project` `#area` `#resource` `#decision` `#todo` `#supplier` `#sars` `#body-corporate` `#appshell` `#renovation` `#smart-home` `#claude`

---

## 7. Verify before trusting

Ask Matthew about these in one batch before or after building — don't block the build on them:

| Item | Why |
|---|---|
| Renovation property: Cape Town or J-Bay? | Notes say Cape Town, but filed under J-Bay and suppliers are in J-Bay |
| AppShell current state | Last known Sept 2026; Phase 3 may be done |
| Work notes in vault? | Could conflict with work data policy |
| Fuelzy / Party Game / Instagram Tools / Wildlife Game status | No recent updates — may be paused; set `status` accordingly |
| Delete `Untitled.md`? | Already in the vault, contents unknown |

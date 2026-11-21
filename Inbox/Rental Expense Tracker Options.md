---
type: decision
status: active
area: "[[J-Bay Flat]]"
project: "[[Flat Renovation]]"
tags: [decision, sars, renovation, todo]
updated: 2026-11-21
---
# Rental Expense Tracker Options

**Bottom line: you don't need a database app yet.** At one flat's volume (roughly 50–200 entries a year), one Markdown note per expense in this vault plus one export script will produce a clean SARS pack. Capturing receipts from your phone is the hard part, not storage. Tax classification also matters more than the tool: capital improvements are **not** deductible against rental income. They only add to CGT base cost. Tagging that correctly per invoice is where the money is.

Design and research only. Nothing has been built.

Related: [[SARS Rental Register]] · [[Finance Model]] · [[Flat Renovation]] · [[Ideas]] (capture system) · [[Suppliers & Contacts]]

---

## 1. The problem in plain language
- Today the Google Sheet "Jeffreys Bay Property (Claude)" holds the money trail (Running Tally Ledger tab). The proof (receipt images) is pasted into cells on the Receipts tab.
- Pasted images can't be named, sorted, exported or handed over one per expense. At tax time someone has to match each image to a row by hand.
- What's needed is a **register** (one row per transaction) where every row points to **one named file** (the invoice or receipt). At year end, a script bundles the register and the files into one pack.
- SARS expects you to keep these records for 5 years after you submit the return. The pack format below is meant to also serve as that archive.

---

## 2. Backend: Obsidian notes vs SQLite

**How each works.** An *Obsidian backend* means one `.md` note per expense with the fields in YAML frontmatter, and the PDF or photo in `Attachments/`. A *SQLite backend* is a single database file with a typed table, plus a folder of documents that the table points to.

| Criterion | Obsidian notes + frontmatter | SQLite + documents folder |
|---|---|---|
| **Data integrity** | Weak. YAML accepts anything: `R1 250,00`, a misspelt category, a missing date. You need a validation script to catch errors. | Strong. Typed columns, `CHECK` constraints, `NOT NULL` and foreign keys stop bad data at entry. |
| **Querying** | Good enough. Bases (core plugin, **already enabled**) gives filtered tables, formulas and column totals. Not built for complex grouping. | Excellent. Full SQL: totals per tax year and category, missing-document reports, running balances. |
| **Attachments** | Native. `![[file]]` previews the receipt inside the note. Links break only if a file is renamed outside Obsidian. | The app must store files and keep paths in sync. Previewing needs the app. |
| **Backup and git** | Already covered: the vault is in git. Every edit is a readable diff. | The `.db` file is binary, so git can't show what changed. You'd need a separate backup, or a CSV dump committed alongside it. |
| **Effort to maintain** | Low. No server and no dependencies beyond Obsidian and one Python script. | Medium. A server, schema migrations and dependency updates, kept running on one machine. |
| **Fits the vault** | Yes. Expenses link to [[Flat Renovation]], supplier notes and rooms. | No. The data lives outside the knowledge layer. |

**Attachment size in git.** Phone photos are 2–5 MB each. Shrunk to about 300–500 KB JPEG or PDF, 150 receipts add roughly 50–75 MB a year. That's acceptable for git. Keep the originals untouched only if the accountant needs them.

**Verdict:** Obsidian, with a validation step in the export script. SQLite's integrity gains don't justify a server for one flat. **Downside of choosing Obsidian:** errors surface at export time rather than at entry, so run the validator monthly, not only in February.

---

## 3. Frontend: how entries get in

| Option | How it works | Phone capture | Effort | Downside |
|---|---|---|---|---|
| **Core Templates + Properties** | New note from an `Expense` template, then fill in the property fields. | Only if Obsidian mobile has the vault (see Sync below). | Very low | Free-text fields, no dropdowns. Easy to mistype. |
| **Templater / QuickAdd prompts** | A command asks for date, supplier, amount and category in pop-up prompts, then files the note. | Same as above. Works on mobile. | Low | Needs community plugins. **Templater, QuickAdd and Dataview are not installed.** |
| **Bases view** | The table to view, filter and total entries. You can edit some properties inline. | View only on mobile. | Very low | Not a capture tool. It's the dashboard. |
| **Custom Obsidian plugin** | A proper form with dropdowns and validation, plus an attach-file button. | Works in Obsidian mobile. | High (TypeScript, plugin API) | Most effort, and it breaks with Obsidian updates. |
| **Small local web app** | A browser form on the desktop PC that writes notes and files into the vault. | Phone browser on home Wi-Fi. Away from home only via a private tunnel such as Tailscale. | Medium | Another thing to run. The PC must be on to capture. |

**The phone problem.** Most receipts appear while you're away from the desk, at the hardware store or in J-Bay. Realistic routes, most private first:
- **Snap now, file later.** Take the photo and drop it into a "Receipts to file" album or folder. Do a weekly 15-minute batch at the desk. No tech needed. Works today.
- **Obsidian mobile on the vault.** The `sync` core plugin is switched on in this vault. If you pay for Obsidian Sync, it's end-to-end encrypted and a QuickAdd capture on the phone would work anywhere. Git on mobile is fiddly.
- **Local web app reached over Tailscale.** The data stays on your PC. Tailscale's servers only broker the connection.
- **Telegram bot into `Inbox/`.** It's already in [[Ideas]], but the receipt passes through Telegram's servers. That's cloud, so opt-in only.

Many suppliers email PDF invoices. Saving those straight into the vault folder removes most of the phone problem.

---

## 4. Data model (one note per entry)

Stored as frontmatter. **Never** stored: bank account numbers, card numbers, ID numbers, tax reference numbers.

| Field | Example | Rule |
|---|---|---|
| `id` | `JB-2627-0012` | Unique. Property code + tax year + sequence. Used in file names and the pack. |
| `date` | `2026-03-14` | Invoice or transaction date, ISO format. |
| `paid_date` | `2026-03-16` | Optional. When cash left the account. |
| `tax_year` | `2026/27` | Derived: month ≥ March → `YYYY/YY+1`, else `YYYY-1/YY`. The script checks it, so typing is optional. |
| `supplier` | `"[[OMJ Construction]]"` | Link to a supplier note where one exists, else plain text. |
| `invoice_no` | `INV-4471` | The supplier's invoice number. This is a document reference, not an account number. |
| `description` | `Bathroom wall tiles, 18 m²` | What it was. |
| `amount` | `12450.00` | ZAR, **VAT-inclusive**, plain number with no `R` and no spaces. |
| `vat` | `1623.91` | ZAR, `0` if none. Information only, unless you're VAT-registered. |
| `ledger_type` | `Cost` | `Income` · `Cost` · `Loan charge` · `Loan repayment`. Matches the Google Sheet. |
| `tax_category` | `capital-improvement` | See the table below. |
| `room` | `Bathroom` | Optional. Helps match to the [[Flat Renovation]] scope. |
| `payment_method` | `EFT` | `EFT` · `card` · `cash` · `debit order`. Never the account. |
| `document` | `"[[2026-03-14 JB-2627-0012 Tile Supplier.pdf]]"` | Link to the file. Required for every `Cost`. |
| `proof_of_payment` | `"[[…pop.pdf]]"` | Optional. Useful when an invoice isn't marked paid. |
| `project` | `"[[Flat Renovation]]"` | Optional parent link. |
| `status` | `captured` | `captured` → `checked` → `filed`. Use `filed` after it goes into a pack. |
| `notes` | | Free text, e.g. "50% deposit". |

**Tax categories.** Labels are for sorting. The accountant decides the final treatment.

| `tax_category` | Meaning | Usual SARS treatment (confirm with the accountant) |
|---|---|---|
| `repairs-maintenance` | Restores something to its original condition | Deductible against rental income |
| `capital-improvement` | Adds or upgrades, e.g. a new kitchen or better tiles | Not deductible. Adds to CGT base cost when you sell |
| `furnishing` | Movable assets: furniture, appliances, TV, linen | Wear-and-tear allowance, s11(e), over the asset's life |
| `operating` | Levies, rates, insurance, agent fees, internet, cleaning | Deductible |
| `finance-interest` | Bond interest and bank charges ("Loan charge") | Deductible |
| `finance-capital` | Capital part of bond repayments ("Loan repayment") | Not deductible |
| `acquisition` | Transfer, bond registration and conveyancing costs | Mostly base cost, not deductible |
| `pre-trade` | Costs before letting started | Possibly s11A. Check with the accountant |
| `income` | Rent received, agent statements | Taxable income |
| `personal` | Private share of mixed-use costs | Not deductible |

**Where things live** (proposed, needs your OK):
- Notes: `Areas/J-Bay Flat (Rental & Tax)/Expenses/2026-27/JB-2627-0012 Tile Supplier.md`
- Files: `Attachments/Receipts/2026-27/2026-03-14 JB-2627-0012 Tile Supplier.pdf`
- Dashboard: `Areas/J-Bay Flat (Rental & Tax)/Expenses.base`, with one table view per tax year and a "missing document" view.

---

## 5. Export: the tax-season pack

A local Python script (`openpyxl`, no internet) reads every expense note for a chosen tax year, validates it, then writes:

```
SARS Pack 2026-27/
├── Register 2026-27.xlsx
│   ├── Summary          totals per tax_category and ledger_type
│   ├── Expenses         one row per entry, Doc column = hyperlink to the file
│   ├── Income
│   ├── Loan             interest vs capital split
│   ├── Assets           furnishings with cost and date (for wear-and-tear)
│   └── Exceptions       missing documents, VAT > amount, wrong tax year…
└── Documents/
    ├── 0001 2026-03-02 JB-2627-0001 Rental Agent.pdf
    ├── 0012 2026-03-14 JB-2627-0012 Tile Supplier.pdf
    └── …
→ zipped as SARS Pack 2026-27.zip
```

- The row number prefix makes the files sort in the same order as the spreadsheet.
- Hyperlinks are relative, so they still work once the accountant unzips the pack.
- Optional extra: one merged PDF of all documents with bookmarks per row. Some accountants prefer a single file.
- The script refuses to build if any `Cost` lacks a document. It still writes the Exceptions tab so you can see why.
- It produces CSV in the Google Sheet's column layout too (see the next section).

---

## 6. Where the Google Sheet fits

| Approach | What it means | Downside |
|---|---|---|
| **Replace** | The vault becomes the only record and the Sheet is archived. | You lose the at-a-glance Payback and Cash Balance view unless the export rebuilds it. |
| **Feed (recommended for now)** | The vault is the source of truth. The export writes a CSV in the Sheet's exact columns (Date, Description, Type, Income, Expense, Payback Balance, Cash Balance), which you paste in. | A manual paste step, which is also a deliberate check. |
| **Import from it** | The Sheet stays primary and the vault pulls rows from it. | Keeps the receipt problem. Needs Google API access, which puts private data in a cloud integration. |

**Decision (2026-11-21): Feed.** The vault is the source of truth and the Sheet receives rows from the export.

### How attachments work when feeding the Sheet
- **The constraint:** Google Sheets runs in the browser and can't open files on your PC. Links like `file:///C:/vault/...` are blocked, so a Sheet can't link straight to local receipts.
- **The fix:** every row carries a **Doc ID** (e.g. `JB-2627-0012`). The same ID is in the receipt's file name in the vault and in the SARS pack. The Sheet points to the document by ID, and the file itself stays local.

| Option | What the Sheet shows | Where the files live | Downside |
|---|---|---|---|
| **1. Doc ID only (recommended)** | A `Doc ID` column, plus a `Doc?` column (✓ or "missing") | Vault only, then the SARS pack zip | You can't click through from the Sheet. You look the ID up in Obsidian or the pack. |
| **2. Google Drive links** | A clickable `Document` link per row | Copied to a private Drive folder (cloud opt-in) | Private tax documents sit in Google's cloud. Uploads are a manual step, or need Drive API access. |
| **3. Images in cells (today)** | The picture itself | Inside the Sheet | Can't be named, exported or handed over per expense. This is the problem we're solving. |

- With option 1, the feed CSV gets two extra columns after the existing seven: `Doc ID` and `Doc?`.
- The Receipts tab stops being used once the import has pulled its images into the vault. Keep it read-only as history; don't delete it.

**Steps:**
- Do a one-off **import** of the existing ledger rows. Download the Sheet as CSV, then let a script create one note per row, all flagged `status: captured` and "document missing".
- After that, the vault **feeds** the Sheet. Retire the Sheet later if you stop using it.
- For the Receipts tab, download the Sheet as `.xlsx` and test whether the in-cell images come out. If they don't, save each image by hand and attach it to its new note. This is a one-off job.
- No Google API, so nothing leaves your machine.

---

## 7. Architecture options

### Option A: Obsidian-native (recommended)
Notes + Bases dashboard + capture template + Python export and validation script.
- ✅ Nothing to host. Uses the vault, git and Bases, which you already have.
- ✅ Every change is a readable git diff, and expenses link to renovation notes.
- ✅ Smallest build: an evening or two.
- ❌ No validation at entry. Typos surface at export, so run the validator monthly.
- ❌ Phone capture depends on Obsidian mobile access or a "file later" habit.

### Option B: Local web app + SQLite
A FastAPI or Next.js app on the desktop PC. SQLite plus a documents folder. Export button.
- ✅ Proper form with dropdowns, a camera upload button and validation.
- ✅ Strong integrity and easy SQL reporting.
- ❌ Lives outside the vault, with no links to renovation notes.
- ❌ Binary DB in git, so backup needs its own plan.
- ❌ Most effort: build, then keep it running and updated. The PC must be on, or reachable, to capture.

### Option C: Hybrid (web capture form that writes into the vault)
A small local web page whose only job is a phone-friendly form. It writes the note and file into the vault, then Option A takes over.
- ✅ Validation at entry and a camera button, while data stays plain text in git.
- ❌ Two ways to edit (form and Obsidian), so the export validator is still needed.
- ❌ Same reachability issue as B. Only worth it if Phase 1 shows phone capture is the real pain.

**Recommendation: A now, upgrade to C only if needed.** B is the alternative to reject. Its downside is a server and a separate backup for a problem that's only ~150 rows a year.

---

## 8. Rough build plan (Option A)

| Phase | Work | Effort |
|---|---|---|
| **0. Decide** | Answer the TODOs below. Agree the folder, file-name and `type` rules. | 15 min |
| **1. Schema** | `Templates/Expense.md` (fields above, no raw tags left), folders, `Expenses.base` with year, missing-doc and category views. | 1–2 h |
| **2. Import** | Script: Sheet CSV → notes. Pull receipt images from the `.xlsx` where possible. Review the flagged rows. | 2–3 h |
| **3. Export** | Script: validate → `.xlsx` + `Documents/` + zip + Sheet-format CSV. | 3–4 h |
| **4. Habit** | Weekly: file new receipts. Monthly: run the validator. February: build the pack. | Ongoing |
| **5. (Optional) Capture** | Option C form, or QuickAdd on Obsidian mobile. | 3–6 h |
| **Later** | Local OCR to pre-fill amount and date. Use Tesseract or a local vision model on your PC, never a cloud OCR. | Unknown |

---

## Open questions
- [ ] TODO: Approve a new frontmatter type `expense` (not in the current schema in CLAUDE.md), or say which existing type to use.
- [ ] TODO: Approve the locations: notes in `Areas/J-Bay Flat (Rental & Tax)/Expenses/<year>/`, files in `Attachments/Receipts/<year>/`.
- [ ] TODO: iPhone or Android? Do you pay for Obsidian Sync? (The `sync` core plugin is enabled.) This decides the phone-capture route.
- [ ] TODO: Are you OK with receipts going to the planned private git remote (which is in the cloud)? If not, git-ignore `Attachments/Receipts/` and back it up locally only.
- [ ] TODO: Are you VAT-registered? If not, record VAT for information only and claim the VAT-inclusive cost.
- [ ] TODO: Ask the accountant (or the CA friend whose template [[SARS Rental Register]] follows) which column layout and pack format they want. Match it.
- [ ] TODO: Accountant to confirm the treatment of costs before letting started (pre-trade s11A vs base cost), and the repair vs improvement split for [[Flat Renovation]] invoices.
- [ ] TODO: Plan for personal-use apportionment once short-term letting starts, if you'll ever stay in the flat yourself.
- [ ] TODO: Sheet attachments: Doc ID only (files stay local), or also copy documents to a private Google Drive folder for clickable links?
- [ ] TODO: Where should the scripts live: a `_meta/scripts/` folder in the vault, or a separate small repo?

## Suggested first steps
- [ ] TODO: Start today, no build needed: save every new receipt as a PDF or photo named `YYYY-MM-DD Supplier Amount` in one folder.
- [ ] TODO: Answer the open questions above.
- [ ] TODO: Ask Claude Code to build Phase 1 (template, folders, Bases view) and enter 3–5 real renovation invoices as a test.
- [ ] TODO: Download the Google Sheet as CSV and `.xlsx` into a local folder (not the vault yet) for the Phase 2 import.
- [ ] TODO: Build the Phase 3 export, then run a trial pack for 2025/26 (Feb 2026 only) to check the format with the accountant before the 2026/27 season.

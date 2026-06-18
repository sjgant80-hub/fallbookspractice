# ◊ FallBooksPractice

**Sovereign practice management for UK accountancy firms.**
Multi-client deadline grid · HMRC agent (64-8) tracker · time recording · fee billing · PII / AML / CPD · per-adviser & firm P&L.

Single HTML file. IndexedDB primary. Mesh-aware. MIT.

---

## For practitioners

Open `index.html` in a modern browser. A demo firm (Patel & Co Chartered Accountants), one adviser, one Ltd client and seeded fees / time / deadlines load on first run — clear from **cog · wipe data** to start fresh.

### What it does

| Tab | Purpose |
|---|---|
| **dashboard** | Open deadlines (traffic-light), WIP value, YTD fees, PII expiry, AML status |
| **deadlines** | Master grid across all clients. SA100 · CT600 · VAT100 · P11D · P60 · ACC · CS01 · PAYE-FPS/EPS. Mark-done with submission ref auto-rolls recurring entries forward |
| **clients** | Add Ltd / sole trader / partnership / LLP / charity / trust. Services engaged drive deadline auto-population from year-end + VAT scheme |
| **time** | 6-minute units. Compliance / advisory / queries / admin / meeting. Weekly view + WIP by client at adviser rate |
| **billing** | Fee ledger (initial · ongoing · fixed-monthly · per-job · advisory). Recurring fee runner (one-click monthly post). Invoice generator → Markdown + postMessage handoff to FallBooksPaper |
| **HMRC agent** | Per-client 64-8 authorisations (SA / CT / VAT / PAYE / CIS). Review-due traffic light |
| **PII** | ICAEW: 25 × highest single-client fee OR £1.5M (whichever lower), floor £100k. ACCA: 25 × gross fee income OR £25k (whichever higher). Held vs required, expiry RAG |
| **AML** | Firm supervisor + ref. High-risk / PEP / sanctions counters. SAR register (NCA pointer — submission still via SAR Online). Annual review committed to audit chain |
| **CPD** | Per-adviser progress bar against body target (ICAEW 90h, ACCA 40u, CIMA 20h). Activity log, practising cert status |
| **advisers** | Per-adviser P&L: fees MTD/YTD, clients, time, realisation rate |
| **firm P&L** | Revenue by fee type · expenses (configurable categories) · PII + body-fee monthly accruals · net profit & margin |
| **Q&A** | T0 offline canned rules for ICAEW PII / ACCA PII / CPD / AML supervisor / 64-8 / retention / engagement / SAR / risk. T3 BYOK for arbitrary queries |

### Bundle mesh

Listens on `BroadcastChannel('fall-books')` for `client.*`, `adviser.*`, `firm.updated`, `submission.recorded`, `sync.*`. When a submission lands on the bus, matching pending deadlines auto-mark as done and (if recurring) roll forward. Boot emits `sync.request`.

### Disclaimer

FallBooksPractice is a tool — it is **not an HMRC-approved filing system**. SA100 / CT600 / VAT MTD / RTI submissions to HMRC and Companies House remain the practitioner's responsibility. PII / CPD / AML calculations are calibrated to current ICAEW / ACCA guidance but the practitioner must verify against the live rulebook. Sovereign — client data never leaves the device unless exported.

---

## For estate operators

```
TOOLNAME = 'fallbookspractice'
VERSION  = '1.0.0'
PRIME    = 809   (accountancy bundle: 769 fallbooks · 773 onboard · 797 paper · 809 practice)
```

### 14-pt gate

| # | gate | status |
|---|---|---|
| 1 | single HTML | OK |
| 2 | < 400KB | OK (~57KB) |
| 3 | IDB primary | 15 stores |
| 4 | localStorage fallback | n/a (IDB only) |
| 5 | KONOMI shim | `BroadcastChannel('fall-signal')` |
| 6 | bundle mesh | `BroadcastChannel('fall-books')` |
| 7 | PWA manifest | data: URL inline |
| 8 | T0 offline | 14 canned rules |
| 9 | T3 BYOK | settings · Anthropic key |
| 10 | two-audience README | this file |
| 11 | MIT LICENSE | yes |
| 12 | mobile-first | breakpoint 760 |
| 13 | disclaimer | top of every view |
| 14 | `.nojekyll` | present |

### IDB stores (15)

`firms · advisers · clients · deadlines · timeEntries · invoices · feeRecords · hmrcAgentAuths · piPolicies · professionalBodyFees · expenses · sars · cpdLog · audit · settings`

### Deadline kinds (10)

SA100 · CT600 · CT-PAY · VAT100 · P11D · P60 · ACC (Companies House accounts) · CS01 (confirmation statement) · PAYE-FPS · PAYE-EPS

### Fee types (5)

initial · ongoing · fixed-monthly · per-job · advisory

### Shared schema conformance

- `BookClient` keys all present (entityType, ctUtr, vatScheme, beneficialOwners, kyc.amlSupervisor, deadlines …)
- `BookFirm` extended (practiceType, professionalBody, practiceCertNo, amlSupervisor, hmrcAgentRef, cqbeStatus)
- `BookAdviser` extended (professionalBody, membershipNo, practisingCert{active,expiresAt}, cpdHoursThisYear)
- Audit chain SHA-256 linked, append-only, 7-year retention
- Aesthetic: oxblood / brass / cream / void

### Deploy

```
cp -r fallbookspractice/* <github-pages-repo>/
git add -A && git commit -m "fallbookspractice v1.0.0" && git push
# Pages → main / root, .nojekyll present, build_type=legacy
```

---

MIT · sovereign · forkable.

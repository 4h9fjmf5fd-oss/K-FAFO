## docket-viewer

**Purpose.** WS-4: "the decision docket, built mechanically" (tools/docket.py:2) — a build→validate→render pipeline that mechanically merges five doctrine sources into valknut/docket/docket.json (319 entries: 316 open / 3 decided in the current JSON) and renders it as a single-file, file://-native, FLuX-themed static explorer page. "The docket describes positions; it grades nothing and no one. No entry is pre-decided except the three carrying Konsent's recorded words." (docket.py:22-24). It is the most complete data-driven browser/explorer app in the repo: stat tiles, dynamic cluster-filter bar, collapsible card list, consent-state rendering.

### Key files

- `/home/user/KlaudeKode/tools/docket.py` — 1174-line stdlib-only pipeline: --build (merge 5 sources -> docket.json), --validate (schema + attribution + §5 cross-check + joined-set seat reconciliation), --render (emit index.html from CSS/JS string templates). Contains the canonical FLuX CSS token block (lines 990-1033) and the entire explorer JS (lines 1035-1100).
- `/home/user/KlaudeKode/valknut/docket/index.html` — The rendered explorer app. 136 lines; line 68 is a 170KB <script type="application/json" id="docket-data"> JSON island. Lines 1-67: head + inline CSS + static skeleton (h1, .sub, 4 stat .tiles, #filters, #list, footer). Lines 69-135: ES5 vanilla JS (card(), draw(), mkbtn(), esc()). STALE: embeds the 2026-07-02 build (252 entries / 249 open) while docket.json is 2026-07-03 (319 / 316).
- `/home/user/KlaudeKode/valknut/docket/docket.json` — Generated data file, 'never a hand list'. Top-level keys: title, generated, builder, sources, counts, open, decided, entries. counts: gates 13, referencemap 43, unclaimed-ground 31, seed-trust-root 6, drops-3-5 9, ward 1, projects 3, smm 1, open-seats 212.
- `/home/user/KlaudeKode/tests/test_docket.py` — Acceptance 'sabotaged both directions': validate-green, exactly three decided rows all Konsent-attributed, K-01..K-43 complete, COA slates 3-5 with exactly one reko, idempotent rebuild, U-register contiguity, joined-set reconciliation, render self-containment (asserts #14101f, #b78b0f, no src=/href=/http://).
- `/home/user/KlaudeKode/tools/lint_doctrine.py` — Dependency: find_seats() (lines 60-76) is the repo-wide *.md ☐ scan reused verbatim by docket.py source 2; mints provisional P-<8hex> ids from sha256(file|snippet), honors anchored '☐ [S-nnn]' ids; owns the HARD FAIL on silent ☐ disappearance.

### Architecture

PIPELINE (docket.py, Python 3 stdlib only): three argparse flags. --build: build() concatenates (1) gate_entries() — G1a..G9, D-01, D-02 hand-authored in code with COA slates, citations recovered by cite() substring-scan into BUILDOUT.md; (2) parse_k_entries() — K-01..K-43 parsed verbatim from valknut/REFERENCEMAP.md §3 via K_HEAD_RE; (3) a U-pool = u_entries_from_refmap() (REFERENCEMAP §5 items + §1 [DRIFT]-without-K-number, each carrying a sha256[:12] content hash) + seed_family_entries() + addendum_entries() (drops-3-5, WS-9 warD slate, project seats incl. iFluX/FyloFone/Sigil, SMM regen sitting), minted U-01..U-nn by enumeration order; (4) seat_scan_entries() — lint_doctrine.find_seats() P-ids for every ☐ in every *.md. Writes docket.json (json.dumps indent=1, ensure_ascii=False). ENTRY SCHEMA (SCHEMA_KEYS, docket.py:46-48): id, cluster, question, positions[{text, citations[]}], coa[{label, reko:bool, good, bad, ugly}], status(open|decided), decision, decided_by, decided_date, decided_quote, may_stay_open, downstream[]; plus unvalidated extras: docket_only, source, file, hash, title, subsection. ID registers enforced by ID_RE (docket.py:49): K-\d{2} | U-\d{2,3} | P-[0-9a-f]{8} | S-\d+ | G\d[a-z]? | D-\d{2} — failure message: "indices, never names" (docket.py:914-915). --validate: two tiers — HARD FAIL (schema keys, duplicate ids, id-register, status enum, list types, COA slate 3-5 with exactly one reko==True, decision-attribution, §5 content-hash cross-check) and REPORT (joined-set reconciliation: file-side ☐ P/S-ids vs docket OPEN non-docket_only ids; "lint_doctrine holds the silent-deletion fail"). --render: render() reads docket.json, interpolates CSS + escaped date + data (with data.replace("</", "<\\/") script-island guard) + JS into one HTML string. UI (index.html): single-column, single-page. Static skeleton: h1 "KK-1 decision docket"; .sub tagline; four .tiles (open seats / decided-gold / entries / shown); #filters bar; #list; footer declaring "Static page; works from file://; no external resources." JS: JSON.parse of the #docket-data island into DOC; clusters counted from data; mkbtn() builds one filter button per cluster plus "all (N)", counts inline in labels, single-select .on state; draw() filters DOC.entries by active cluster and full-rebuilds #list.innerHTML via card(); esc() = textContent-based HTML escaping. card() anatomy: .head row (monospace .id in #a99ad4, .chip cluster pill, gold .decided-mark "decided — Konsent" when decided, italic .stay "this seat may stay open, possibly permanently" when may_stay_open) → .q question → <details> "N position(s) — enumerated, never graded" with per-position .cite [file:line …] → <details> "slate — N COAs (reko marked; the pick is Konsent's)" with reko → prefix + good/bad/ugly .gbu lines → .down "downstream: a · b" → the "Konsent's word" .word box: dashed border + "the slot sits empty; the seat is open" when open; solid gold border + gold quote + "— Konsent, 2026-07-02" when decided. NO search box, no sort control, no status filter, no timeline, no multi-pane, no URL-hash state, no keyboard nav — the only interaction is the cluster filter and native <details> toggles. Read-only: decisions are recorded only by editing docket.py source and rebuilding.

### Theme tokens

- :root{--page:#0c0a12;--surface:#14101f;--edge:#2a2140;--ink:#d8d2e2;--dim:#8f87a3;--gold:#b78b0f;} (docket.py:991-992 / index.html:8-9)
- id/reko accent purple: #a99ad4 (.id, .coa b.reko)
- filter active state: border-color:#6a5aa8; background:#1c1630 (.filters button.on)
- font: 15px/1.55 system-ui,-apple-system,sans-serif; body padding 24px
- h1: 1.35rem/600; tile number 1.6rem/700; tile label .8rem dim; chip .72rem; summary .82rem; positions/coa .88rem; gbu/cite .78-.8rem; footer .75rem
- radii: 8px (cards, tiles), 6px (buttons, word box), 99px (chips); gaps 12px tiles / 8px filters / 10px card head; entry padding 14px 16px, margin-bottom 10px
- monospace: ui-monospace,monospace for ids
- gold semantics: #b78b0f used ONLY on decided marks, decided tile number, decided word-box border/quote/date — 'Gold marks carry only what Konsent decides' (index.html:65-66)
- open-seat semantics: 1px dashed var(--edge) border on the empty Konsent's-word box; decided flips to solid gold
- footer self-declaration: 'FLuX theme: page #0c0a12 · surface #14101f · gold #b78b0f' (index.html:66-67)
- dark-native only; no light theme, no prefers-color-scheme handling anywhere in the file

### Consent mechanics

This subsystem is the project's consent machinery made mechanical, enforced at three layers. (1) DATA: only 3 of 319 entries are decided (G1a "clone here only.", D-01 "upload from drive.", D-02 "both paths." — all decided_by Konsent, 2026-07-02, quotes verbatim); every other entry's decision fields are empty strings. G6 is explicitly Kyn's seat ("Every seat is Konsent's (one is Kyn's)" — index.html:56 / lint_doctrine.py:96-97), with a 5-COA slate on asking-semantics whose reko is fail-closed free-form parsing ("anything not a clear assent records as not-assent", docket.py:401-408). (2) VALIDATOR: HARD FAIL if any entry is decided-ish without decided_by=="Konsent" ("carries a decision without decided_by: Konsent", docket.py:948-949) or lacks decided_quote/decided_date ("the record follows the word", docket.py:953-954); HARD FAIL on any COA slate outside 3-5 ("a slate is 3–5, never fewer — a binary at a mind", docket.py:928-929); HARD FAIL unless exactly one reko per slate ("exactly one per slate", docket.py:941-942); status enum has no "closed" — only open|decided. Tests sabotage each rule both directions, including decided_by="Tactician" failing (test_docket.py:115-121). (3) UI: gold (#b78b0f) is color-reserved for Konsent's decisions — "Gold marks carry only what Konsent decides" (index.html footer:65-66); test comment "gold, decided marks only" (test_docket.py:205); open seats render an empty dashed "Konsent's word" box — absence-of-decision is a first-class rendered state, not a blank. Microcopy carries the no-verdict law: "position(s) — enumerated, never graded"; "reko marked; the pick is Konsent's". may_stay_open renders "this seat may stay open, possibly permanently" — a seat is allowed to never close.

### Strengths observed

- Complete, tested, stdlib-only build→validate→render pipeline; docket.json is always regenerated, 'never a hand list' (docket.py:11)
- Rendered page is fully self-contained and file://-native, with the self-containment machine-asserted (test_docket.py:199-208)
- Consent mechanics are machine-enforced, not conventions: attribution hard-fails, slate-size hard-fails, single-reko hard-fails, §5 content-hash cross-check, joined-set seat reconciliation (docket.py:884-985)
- Idempotent rebuild with stable ids tested across rebuilds and against the shipped JSON (test_docket.py:160-171)
- XSS-safe rendering: esc() via textContent, plus the </ script-island escape at render time
- FLuX theme centralized in one :root token block, matching the validated theme constants (#14101f surface, #b78b0f gold) that other subsystems and CLAUDE.md cite
- Gold-as-consent color semantics carried consistently across CSS, markup, footer text, and a test comment ('gold, decided marks only')
- Filter bar and cluster counts derived entirely from data — the UI has zero hardcoded category knowledge
- Absence-of-decision is a rendered first-class state (dashed empty 'Konsent's word' box), and may_stay_open renders permanence-of-openness as legitimate
- Sabotage tests cover both directions for every rule, including a wrong-attributor case (decided_by='Tactician' fails)
- Reuse over duplication: the seat scan is lint_doctrine.find_seats imported, 'the exact SEATS.txt seat-scan' (docket.py:38)

### LIMFACs observed

- STALE RENDER: index.html embeds the 2026-07-02 build (252 entries, open=249) while docket.json on disk is generated 2026-07-03 (319 entries, open=316) — --render was not re-run after the last --build; no test asserts render freshness against docket.json (TestRender checks only existence/self-containment)
- No search, no sort, no status(open/decided) filter, no timeline, no multi-pane, no URL-hash deep-linking, no keyboard navigation — the explorer's only interaction is one single-select cluster filter plus native <details>
- All 212 seat ids are provisional P-<8hex>; the S-\d+ anchored register in ID_RE is entirely unused — no stable addressing for seats exists yet (lint_doctrine.py:95-96: 'ids are provisional (P-xxxxxxxx) until stable ☐ [S-nnn] anchors are added')
- U-register ids are enumeration-order indices; identity actually rides on the content hash — U-numbers can renumber when source pools change ('register indices, never names', docket.py:15)
- P-ids are keyed to hash(file|snippet): any edit to a seat's line text or file move mints a NEW P-id and orphans the old one (surfaced only as REPORT-tier unreconciliation)
- Seat-scan question text truncates at 120 chars (lint_doctrine.py:75) and can cut mid-sentence; raw markdown ** markers display as literal text in the UI
- Dark-native only; light theme is an underived open seat — no prefers-color-scheme or light tokens anywhere
- No Node runtime in the environment: the page's JS is never executed by any test; behavior is verifiable only by opening a browser
- Full innerHTML rebuild of the entire list on every filter click — unmeasured beyond 319 entries
- entry() **extra kwargs (docket_only, source, file, hash, title, subsection) are load-bearing but outside SCHEMA_KEYS validation
- The render date in the .sub line and the tiles come from the embedded JSON, so a stale page self-reports the wrong vintage as if current
- test_only_three_decided_all_konsent hard-codes the decided set ['D-01','D-02','G1a'] — every future decision by Konsent requires a test edit in the same change
- The page is read-only: recording Konsent's word requires editing docket.py source (gate_entries) and rebuilding — no data-side decision channel exists
- Teksidure open definitions ride through: the SMM-regen entry records that 'Verify's definition is Konsent's open seat' (docket.py:816-818)

### K-FAFO patterns to adopt

- JSON data-island: <script type="application/json" id="docket-data"> + JSON.parse(textContent), with data.replace("</", "<\\/") injection guard (docket.py:1138) — zero fetch(), zero CORS, works from file:// with no server; the natural data-feed shape for a K-Eden-native explorer with no network assumption
- build/validate/render tri-command pipeline where the HTML is a pure function of the JSON and the JSON is 'never a hand list' — K-FAFO panes should render generated JSON, not hand-authored HTML
- self-containment as a TESTED invariant: assertNotIn src=/href=/http:// (test_docket.py:206-208) — an executable acceptance rule for any file://-native K-FAFO page
- FLuX tokens as :root CSS custom properties (--page/--surface/--edge/--ink/--dim/--gold) — copy verbatim; #a99ad4 for ids, #6a5aa8/#1c1630 for active-filter state
- gold-reserved-for-Konsent color law and the dashed-open/solid-gold-decided 'Konsent's word' box — a UI grammar for consent state any K-FAFO view of gated objects should carry
- stat-tile HUD header (open/decided/total/shown) with the 'shown' tile live-updating on filter — the count of what the current view shows is itself first-class
- dynamic filter-chip bar derived from the data (cluster + count per button, 'all (N)' default) — no hardcoded categories
- card anatomy: monospace id + cluster chip + status mark / question / <details> collapsibles for enumerated positions and COA slates / downstream links / consent-word box — native <details> gives collapse-expand with zero JS state
- no-verdict microcopy baked into chrome: 'enumerated, never graded', 'reko marked; the pick is Konsent's', 'the slot sits empty; the seat is open'
- esc() textContent-based escaping before every innerHTML write — the page renders arbitrary doctrine text safely
- citations as [file:line] spans on every position (CITE_RE / cite() substring-recovery) — provenance-per-row is the navigation primitive a general explorer can hyperlink
- content-hash identity (sha256[:12]) beneath ordinal ids: U-numbers are display indices, the hash is the stable key ('register indices, never names') — deep-link by hash, not by ordinal
- provisional-vs-anchored id scheme (P-<8hex> until ☐ [S-nnn] anchors land) for addressing not-yet-stable entities
- two-tier validation (HARD FAIL vs REPORT) with explicit fail ownership ('lint_doctrine holds the silent-deletion fail')
- sabotage-both-directions test style: every consent rule has a test that plants the violation and asserts the failure message

### K-FAFO constraints

- No Node.js/deno/bun — pipeline must stay Python 3 stdlib (docket.py:24 'Stdlib only'); page JS is ES5 vanilla, no build step, no framework, and is never machine-executed (tests check the HTML as text only)
- file://-native, zero external resources — enforced by test; K-FAFO cannot assume a server, CDN, fonts, or network
- dark-native only; light theme is an underived ☐ (CLAUDE.md) — no light-mode tokens exist in this subsystem to inherit
- the no-verdict law binds the UI: nothing may grade, rank, or score; decided state requires Konsent's verbatim quote + date + decided_by==Konsent or validation hard-fails
- COA slates are structurally 3-5 with exactly one reko — any K-FAFO decision-point UI inherits this shape
- the render step is manual and decoupled from build — the shipped index.html is one build behind docket.json (2026-07-02/252-entry blob vs 2026-07-03/319-entry JSON), so an embedded-island K-FAFO inherits this staleness class unless it reads the JSON live or the build gates the render
- docket_only/source/file/hash extras are load-bearing (docket_only excludes entries from seat reconciliation) but sit outside SCHEMA_KEYS validation
- full innerHTML list rebuild per filter click — fine at 319 entries, unproven at explorer scale
- seat-scan snippets truncate at 120 chars and carry raw markdown ** markers rendered literally

### Open questions (Konsent's)

- Should K-FAFO read docket.json (and sibling JSONs) live from disk, or embed at build time? The observed one-build staleness gap makes this Konsent's architecture seat, not a detail
- iFluX's own docket seat carries reko 'COA-3 · fold into the Kosmos-HUD lineage' — 'the HUD panes are already a browser-native surface' (docket.py:779-781): is K-FAFO that fold, i.e. is K-FAFO the iFluX seat's answer? Only Konsent's recorded word can say
- Should K-FAFO gain a write path — recording Konsent's word interactively — or stay read-only like this page, with decisions entering only via docket.py source edits + rebuild? An interactive consent-recorder must reproduce the attribution hard-fails client-side
- Do stable ☐ [S-nnn] anchors get added to doctrine files (all 212 current seats are provisional P-ids)? Stable deep-linking in an explorer depends on it
- Light theme derivation (☐ underived) — needed if K-FAFO ever renders outside the dark FLuX surface
- Search, sort, status-filter, timeline, multi-pane: none exist in this precedent; which of these K-FAFO carries is an open slate no prior art here decides
- The Verify sitting seat (docket.py:815-827): Verify's definition is Konsent's open seat — any K-FAFO 'verify' affordance waits on it

### Verbatim

- “The docket describes positions; it grades nothing and no one. No entry is pre-decided except the three carrying Konsent's recorded words. Every COA slate is 3–5, each COA with reko + good/bad/ugly. Stdlib only.” — `tools/docket.py:22-24`
- “:root{--page:#0c0a12;--surface:#14101f;--edge:#2a2140;--ink:#d8d2e2;
--dim:#8f87a3;--gold:#b78b0f;}” — `valknut/docket/index.html:8-9 (also tools/docket.py:991-992)`
- “Static page; works from file://; no external resources. Gold marks
carry only what Konsent decides. FLuX theme: page #0c0a12 · surface #14101f ·
gold #b78b0f.” — `valknut/docket/index.html:65-67`
- “Every seat, walkable — built mechanically by tools/docket.py;
generated 2026-07-02. The docket describes positions; it grades nothing and no one.
Every seat is Konsent's (one is Kyn's).” — `valknut/docket/index.html:54-56`
- “a slate is 3–5, never fewer — a binary at a mind” — `tools/docket.py:928-929`
- “entry %s decision lacks decided_quote/decided_date (the record follows the word)” — `tools/docket.py:952-954`
- “id %r outside the registers (K-/U-/P-/S-/G/D — indices, never names)” — `tools/docket.py:914-915`
- “position(s) — enumerated, never graded” — `tools/docket.py:1048 (rendered in index.html card JS)`
- “slate — '+e.coa.length+' COAs (reko marked; the pick is Konsent’s)” — `tools/docket.py:1057`
- “the slot sits empty; the seat is open” — `tools/docket.py:1073`
- “the HUD panes are already a browser-native surface” — `tools/docket.py:761-762 (iFluX project seat, COA-3 reko: fold into the Kosmos-HUD lineage)`
- “ids are provisional (P-xxxxxxxx) until stable `☐ [S-nnn]` anchors are added to the doctrine files. Every seat is Konsent's (one is Kyn's). This ledger records seats; it closes none.” — `tools/lint_doctrine.py:95-97`
- “COA-1 · free-form asking: the proposal is put in-context; Kyn answers in his own words; parsing fails closed (anything not a clear assent records as not-assent)” — `tools/docket.py:401-404 (G6, Kyn's seat, the reko COA)`

### Flags

- STALENESS: valknut/docket/index.html is one build behind valknut/docket/docket.json — embedded blob generated 2026-07-02 with 252 entries/249 open; the JSON is generated 2026-07-03 with 319 entries/316 open. tools/docket.py --render would resync it. No test guards this gap.
- DIRECT K-FAFO BEARING: the iFluX (browser, project #3) seat lives IN this docket with reko on 'COA-3 · fold into the Kosmos-HUD lineage' because 'the HUD panes are already a browser-native surface' (docket.py:761-762, 778-781). Whether K-FAFO is that fold is an open seat only Konsent can close — a K-FAFO built without his recorded word would silently decide the iFluX seat.
- Exactly three decided entries exist repo-wide (G1a, D-01, D-02 — all Konsent, 2026-07-02) and test_only_three_decided_all_konsent (test_docket.py:35-43) hard-codes that list; every future decision requires a coordinated test change.
- The decided-set date in the shipped HTML sub-line self-reports the stale vintage as current — a reader of the page sees 2026-07-02 counts with no signal that fresher data exists beside it.
- The docket UI and the kode/explorer HUD (pane #1) are separate explorer precedents in the repo; this digest covers only the docket. Synthesis must reconcile the two chassis (single-column card list vs HUD panes) — the docket's own iFluX reko points at the HUD lineage.
- S-register (stable seat anchors) is designed into ID_RE and lint_doctrine's ANCHOR_RE but zero S- ids exist — all seat addressing is provisional; any K-FAFO deep-link scheme built today would link to hash-fragile P-ids.
- docket.py hand-authors the gates/addendum content in Python source (gate_entries/addendum_entries) — the 'mechanical merge' is mechanical for K/U/P sources but the G/D rows and slates are code-as-canon; recording a new decision means editing Python, not data.
- The WS-9 warD entry records 'NO VERDICT, anywhere — Observe → React, nothing graded in the middle' (docket.py:730-732) — a second, independently-arrived statement of the same fence the docket UI enforces; its reconciliation is itself an open COA slate.


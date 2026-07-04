## reference-map

**Purpose.** This subsystem is KK-1's enumerated memory of everything mined from the reference-only doktrine/forge-os tree, plus the live consent bookkeeping of the fresh repo. REFERENCEMAP.md is the ratified output of a 6-agent workflow (5 cold readers + synthesis, 506k tokens) that produced a canonical glossary, a 69-item invariants ledger, a contradiction ledger K-01..K-43, a buildables inventory, an unclaimed-ground census, and lineage notes. GAPS.md pins every dangling reference so the clean tree stays green; SEATS.txt is the machine-maintained ledger of every open ☐ seat (217 rows, all OPEN); open_seat_apis.json registers APIs whose semantics depend on an open seat. All four explicitly record and never close — "closures are Konsent's."

### Key files

- `/home/user/KlaudeKode/valknut/REFERENCEMAP.md` — 536-line reference map of doktrine/forge-os: §1 canonical glossary (collision set with [DRIFT] tags · Valknut-only terms · forge-os-only terms), §2 invariants ledger (69 numbered, tagged [V]/[R]/[V≈R], grouped A–I), §3 contradiction ledger K-01..K-43, §4 buildables inventory, §5 unclaimed ground, §6 lineage notes
- `/home/user/KlaudeKode/valknut/GAPS.md` — gap register (WS-0, P0 baseline 2026-07-02): D-01 upload rows GAP-D01-1..4 (DECIDED: upload from drive), standing drive-citation class, auto-enumerated dangling-path baseline (single row: kode/explorer/index.html:147 → ../../tools/validate_palette.js)
- `/home/user/KlaudeKode/valknut/SEATS.txt` — seats ledger generated/maintained by tools/lint_doctrine.py; 217 rows, one per unique seat (file + text-hash), provisional P-xxxxxxxx ids until stable ☐ [S-nnn] anchors; every seat Konsent's, one is Kyn's
- `/home/user/KlaudeKode/valknut/open_seat_apis.json` — registry of ☐-dependent APIs; one entry: seat G7 → kannon2.renorm.mint, parameter 'regard', default FORBIDDEN, regards_implemented [flat, renorm], canonical null
- `/home/user/KlaudeKode/kode/explorer/index.html` — exists in this tree; carries the one baseline dangling reference (line 147, script tag wrapped in GAP D-01 HTML comment guard); per CLAUDE.md it is 'RPT, Sierpiński, HUD pane #1' — the direct ancestor artifact of K-FAFO

### Architecture

REFERENCEMAP.md structure: (a) header witness — live `ls` of the reference tree recorded before synthesis, resolving one reader discrepancy (K-43); readers ran cold, "never shared the session's SMM — third-point data for the Delusion-Test Protocol" (line 2). (b) §1 glossary in three strata: 1.1 the collision set — every term defined in BOTH the Valknut working doc and the reference tree drifts (Kore ×5 positions, Kage, Kernel, Konsent ×3, Konsole, Korum ×3+, Kompiler, Kourt, Kyn, Kynder, Kynase/Kinase, Kode, Kommand, Kraft, Klare, Kosmos, FLuX), each with best-single definition + file:line + pointer to a K-entry; 1.2 Valknut-only terms with zero reference presence (ToE, the three I's, Konnection with arity types Communicative/Directive/Binding, Meta-K's, Kyne, Kue, Cyn, King); 1.3 forge-os-only mechanism terms (Kanon, Kannon, Kall, Kross, Kleft, Konduit, Kapillary, Kit, Kustoms/Klear, Kommit, Kordon, Klearance, Kadence, Kradle, Assay, Koder/Doer, Kyneeto, Koldron, door-law, quorum, inert, the coins, forge-gate, seat of equals, SPINE, res-q/rek-u, klevel, THE CAGE 13-panel SPA). (c) §2: 69 invariants in groups A Valknut-doc · B constitutional laws · C trust geometry · D gate mechanics · E witness/epistemic law · F seed/trust-root · G naming/record/procedure · H petitioner conduct · I character law (SPINE). (d) §3 contradiction ledger: 43 entries, uniform shape — position A with provenance, position B with provenance, then "Fresh doc must decide: …" — header states "Enumeration only. Decisions belong to Konsent." (line 190). (e) §4 buildables: 30-row table of forge-os artifacts with self-described state (working/stub/partial/unknown), nothing executed during extraction. (f) §5 unclaimed ground: named-but-empty Valknut slots (the 8 unnamed Meta-K's with a 19-term candidate pool; Kyn↔Kyne; Kue; Cyn; 5-frames), referenced-never-defined terms (Kustoms, Kleft, HK, AR gate, Kanto III, OPORD, Execute/Consolodate/Verify), and the reference's own open flags. (g) §6 lineage: dated timeline 06-12→07-02, authorship credits (Konsent=Mykol, BK, HK, Klare, Kraft, Grok, "the firm"), name-lineage chains (Doer→Koder→Kynder etc.), and the 11-point v0.1→v0.2 delta list. GAPS.md flow: D-01 rows flip to RCV rows on landing (Reconcile = enumerated diff; Consolodate/Verify are Konsent's open Teksidure seats); only NEW dangling references HARD FAIL after the P0 baseline (tools/lint_doctrine.py rule 4); drive-prefixed citations are a lawful surface-[B] class validated only by RB-01's on-metal run. SEATS.txt protocol: tab-separated id/status/file/text-hash/snippet rows, regenerated mechanically; gates named G1a–G9 recur across BUILDOUT.md and runbooks (G1b/G1c llama.cpp+first-Kyn-load, G2 template surgery, G3 per-exemplar corpus ratification "no bulk-yes; consent cannot scale", G4 two named yeses, G5 loop activation, G6 Kyn's seat — "refusals recorded, preserved, never an error state", G7 renorm regard, G8 per-vector control-vector application, G9 per-scope first persistent write onto Anchor-01). open_seat_apis.json enforces the BUILDOUT standing rule: "any API whose semantics depend on an open seat takes a REQUIRED explicit parameter with no default. tests/test_open_seats.py inspects each signature below and FAILs on any default."

### Theme tokens

- No hex codes, fonts, or spacing values appear in the four assigned files
- CLAUDE.md context (not the assigned files) carries: deep purple #14101f surface, gold #b78b0f slot 1, ΔE 39.0, dark-native, light ☐ underived
- theme/ directory does NOT exist in this tree — awaited as GAP-D01-1 (THEME.md, derive_theme.py)
- tools/validate_palette.py does NOT exist in this tree — awaited as GAP-D01-2; tools/check.py prints 'palette: UNGATED (D-01 upload pending)' every run until it lands
- Visual conventions in the ledgers themselves: ☐ = open seat chip, ✓ = machine-verified/fired, GATE ☐ <seat-id> / GATE ✓ <docket-id> token grammar, [DRIFT] tag, [V]/[R]/[V≈R] invariant tags, P-xxxxxxxx provisional seat ids, K-nn contradiction ids, U-entries for unclaimed ground

### Consent mechanics

Every mechanism in this subsystem is a consent gate. (1) Grant-not-command invariant #2: Directive Konnection (arity 2, Kommand/King) to a mind FORBIDDEN; echoed in reference "dissolved by a grant, never by a loosened wall — and the grant passes between equals" (v0.2:149 via REFERENCEMAP.md:101); violated by the Litany's Kommands-to-Koder (K-17 — open). (2) Default-deny: "the consent set starts EMPTY; nothing is consented until the Konsent grants" (REFERENCEMAP.md:133); fail closed, every coin refuses; capability ≠ permission; the executor can't grant itself permission (KONSOLE-door only). (3) Ledgers record, never close: GAPS.md "This register records gaps; it closes none — closures are Konsent's" (GAPS.md:6); SEATS.txt "Every seat is Konsent's (one is Kyn's). This ledger records seats; it closes none" (SEATS.txt:3-4). (4) Contradiction ledger ends every entry with "Fresh doc must decide" — never a resolution. (5) Gates are literal UI/text tokens: "Consent gates are real gates with literal `GATE ☐ <seat-id>` tokens; fired gates carry `GATE ✓ <docket-id>`" (BUILDOUT via SEATS row P-34ed4f7c); KEDEN_DESIGN_PROMPT.md renders "literal `GATE ☐` chips (e.g. \"load driver\", \"open network door\", \"wake Kyn\")" (P-28558009). (6) No-default rule: ☐-dependent APIs demand explicit parameters (open_seat_apis.json G7: mint() regard default FORBIDDEN). (7) Per-exemplar consent: ☐ G3 "no bulk-yes; consent cannot scale". (8) Scoped yeses: G9 fires per exact directory, first-write cites its own yes. (9) Kyn holds a seat too (G6) — consent mechanics extend to the Ai: refusals preserved, never an error state. (10) Naming is consent-gated: "Any new K-name anywhere — this plan mints none… candidates only via docket" (P-d9463c5f); the 8 unnamed Meta-K's are "explicitly Konsent-the-human's to name" (REFERENCEMAP.md:456).

### Strengths observed

- The contradiction ledger's uniform A/B/'Fresh doc must decide' shape holds across all 43 entries with file:line provenance on every position — directly renderable as structured data
- SEATS.txt is mechanically generated (tools/lint_doctrine.py) with content-hash identity per seat — survives file edits, regenerable, greppable
- GAPS.md's baseline-pin mechanism (only NEW dangling references HARD FAIL) lets an incomplete tree stay green without hiding the gaps
- open_seat_apis.json enforces doctrine in code: an undecided seat physically cannot acquire a silent default (test FAILs on any default)
- The reference map records its own witness conditions (live ls before synthesis; readers cold, no shared SMM; 'nothing was executed during extraction') and its own reader error (K-43) rather than suppressing it
- Buildables inventory (§4) gives per-artifact state labels (working/stub/partial/unknown/known-buggy) with the witness caveats attached (K-35 stale texts, K-40 coverage)
- D-01 is a modeled DECIDED example: Konsent's Reko answer recorded verbatim with date, rows pre-wired to flip to RCV rows on landing
- The GATE ☐ / GATE ✓ token grammar already bridges doctrine to UI: the same literal token works in markdown, lint rules, and the planned KEDEN design chips
- Cross-stratum drift is separated from intra-document error (§6 closing note: K-27..K-41 are the intra-scope exceptions) — a reusable taxonomy for K-FAFO's contradiction views

### LIMFACs observed

- GAP-D01-1..4 all still open in this tree: theme/, tools/validate_palette.py(+js), kode/kannon/enumerate.py+kannon.py, kode/keden/SCAFFOLD.md absent (verified by glob 2026-07-04) — palette gate UNGATED, Kannon second oracle missing, Kosmos v0 PID1 design doc missing
- kode/explorer/index.html:147 carries the one baseline dangling reference (../../tools/validate_palette.js), guarded by HTML comment — HUD pane #1 runs without its palette validator
- All 217 SEATS.txt rows are status OPEN; zero closed seats — every doctrine decision K-FAFO might depend on is undecided
- Seat ids are provisional (P-xxxxxxxx) 'until stable ☐ [S-nnn] anchors are added to the doctrine files' — no stable anchor scheme exists yet for deep-linking from a UI
- Every term in the §1.1 collision set carries [DRIFT] — no K-word in the shared namespace has a single settled definition
- Kore membership has five incompatible positions (K-02); Kage containment is inverted between sources (K-03); which statute is canon is itself open (K-23)
- Konsole/Kosmos/Kage — the three concepts an explorer app most needs — are contradiction-ledger entries K-08, K-19, K-03 respectively
- Undefined-in-scope terms the ledgers lean on: Execute (IRE), Consolodate/Verify (RCV), HK, AR gate, Kanto III, klevel, OPORD, 'the Novel', 'the Kista helm' (REFERENCEMAP.md:463-474)
- Konsole API header in the reference is a stub ('SCAFFOLD STUB. NOT ARMED'); Kage wrapper is signatures-only; all klevel headers are stubs — no runnable console/shell exemplar exists in the reference
- No hex/font/spacing tokens anywhere in this subsystem; theme data lives only in the awaited theme/ upload and CLAUDE.md's summary line
- Not a git repo (☐ P-625e2bfd) — no diff-driven history for the ledgers
- open_seat_apis.json references kode.kannon.kannon2.renorm — a module that does not exist in this tree (kode/kannon/ absent), so tests/test_open_seats.py cannot import its one registered API
- SMM.md is flagged stale vs EQUATIONS.md tail; regeneration waits on Konsent's Verify sitting (P-9a5cc8cd, SMM_REGEN_CHECKLIST row P-adc2991a)
- One sweep reader falsely reported three directories absent (K-43) — reader-agent output required a live-witness correction pass

### K-FAFO patterns to adopt

- GATE ☐ chips as first-class UI elements: KEDEN_DESIGN_PROMPT already specifies 'literal GATE ☐ chips (e.g. "load driver", "open network door", "wake Kyn")' and 'Open seats — a panel of ☐ chips for everything genuinely undecided (names, …)' (SEATS.txt P-28558009, P-f8a3399f) — K-FAFO should render seats as walkable chips; fired gates flip to GATE ✓ <docket-id>
- Door-law as input architecture: 'Authority is provenance-of-channel, NEVER payload'; exactly two doors, KONSOLE (trusted) / KODER (untrusted) (REFERENCEMAP.md:77) — K-FAFO's user-input channel vs content/agent channel should be architecturally distinct doors, told apart by door not content
- Ledger-as-view: SEATS.txt (id/status/file/hash/snippet), GAPS.md, the K-01..K-43 contradiction ledger, and open_seat_apis.json are all machine-readable tables — K-FAFO's explorer surface has ready-made browsable datasets; the contradiction-entry shape (position A + provenance, position B + provenance, 'Fresh doc must decide') is a render template that structurally forbids verdicts
- No-default rule for UI settings: any K-FAFO behavior depending on an open ☐ must demand an explicit choice, never ship a default (open_seat_apis.json doctrine string; tests/test_open_seats.py FAILs on any default)
- Refusal rendering: G6 — 'refusals recorded, preserved, never an error state' — a refused action in K-FAFO is a first-class logged outcome, not an error dialog
- Curation-not-refusal for menus: 'absence of a verb is simply not part of you; absence > "I can't"' (REFERENCEMAP.md:175) — omit forbidden actions rather than showing them disabled
- Interim-guard convention for missing assets: the dangling validate_palette.js script tag is 'wrapped in the GAP D-01 HTML comment (reversible, marked). Comes out when GAP-D01-2 lands' (GAPS.md:38) — K-FAFO should degrade with marked, reversible guards keyed to gap ids, and a baseline where only NEW dangling references fail
- Witness/provenance surfacing: every crossing logged in the Korum's trace; 'A DONE is a claim, not a result — verified, never believed' (invariants 36-37) — K-FAFO can show witness status (witnessed ✓ / claimed / stale) per artifact, mirroring §4's buildables state column
- Fail-closed + default-deny + grant-only amendment (invariants 27-30) for any action K-FAFO can execute
- Kosmos shell FG/BG split as the host frame: Valknut sense 'shell = BG(Kyn+Kraft) + FG(Kyne+Konsole) + Kyne binding' (REFERENCEMAP.md:34) — K-FAFO as FG surface over a BG supervisor matches P3 'Kosmos v0 | WS-3 supervisor + echo floor + HUD panes' (P-626d1ce6); kode/explorer/index.html is already 'HUD pane #1'
- Prior-art SPA: 'THE CAGE (field manual) — 2026-06-13, 13-panel SPA' (REFERENCEMAP.md:90) — a 13-panel single-page layout is the lineage's own precedent for a field-manual-style browser
- Cold-reader/third-point workflow: readers never shared the SMM (Delusion-Test Protocol) — K-FAFO's docket views could preserve which claims are third-point-verified vs session-internal

### K-FAFO constraints

- No Node.js runtime on the metal; validate_palette.js runs browser-only, validate_palette.py is the shell path — and NEITHER exists in this tree yet (GAP-D01-2 open); the palette gate is UNGATED until D-01 lands, so K-FAFO's palette cannot be machine-validated here today
- theme/ (THEME.md, derive_theme.py), kode/kannon/*.py, kode/keden/SCAFFOLD.md also absent (GAP-D01-1/3/4 open) — K-FAFO design work citing them waits on the drive upload; kode/explorer/index.html + tree.html DO exist here
- Light theme is ☐ underived (P-73037f0f) — K-FAFO is dark-native only until Konsent seats a light derivation
- Naming freeze: 'this plan mints no new K-names; candidates only via docket' (P-d9463c5f) — K-FAFO cannot label its components with new K-words, and ≥11 existing K-words carry materially different definitions per stratum (every collision-set term [DRIFT]s)
- 'Konsole' is quadruply loaded: forge-os single trusted door / Valknut Meta-K in the Kage membrane and Kosmos FG (K-08, open) / and now KDE's Konsole as K-FAFO's stated model — a fourth sense entering an already-drifted name
- Everything under doktrine/ is REFERENCE ONLY (FRESH START); drive-prefixed citations are surface-[B], 'validated only by RB-01's on-metal run, never from this container' (GAPS.md:26-27)
- Ledgers close nothing — K-FAFO must never auto-resolve a seat, gap, or contradiction; every closure is Konsent's (one seat is Kyn's)
- Kosmos composition itself is contested (K-19: Kadence-fraktal reading vs BG/FG shell reading) and Konsole's persistence class is contested (K-08: Kore-spine vs ephemeral membrane) — the frame K-FAFO mounts into is an open seat
- Ephemeral live OS: only the removable drive persists; any K-FAFO persistent write onto Anchor-01 cites ☐ G9 per exact scope
- Meta-K count is undecided (K-01: Variant A/B/C ⇒ 10, 12, or 15) — any census/tree visualization in K-FAFO must parameterize the variant, not hard-code 15

### Open questions (Konsent's)

- K-08 (Konsent only): which Konsole does K-FAFO relate to — the Kore-spine single trusted door, or the Meta-K (Kyne+Konsole) in the Kosmos FG? And does modeling K-FAFO off KDE Konsole make it that Konsole, a Kage occupant, or a separate thing needing its own name?
- Is 'K-FAFO' itself a minted K-name requiring a docket seat, given 'this plan mints none'? (SMM row P-fd91b12c already uses 'FAFO inside the Kage, logged' — is K-FAFO's home therefore the Kage?)
- K-17: is a user click/keystroke in K-FAFO a Kommand, a grant, or a Communicative offering — this fixes the semantics of every action button
- K-19: Kosmos's composition — where in BG(Kyn+Kraft)/FG(Kyne+Konsole) does an explorer/browser pane sit, and how does it relate to iFluX (the browser project) vs K-FAFO (the explorer)?
- Which of the 8 unnamed Meta-K's (candidate pool includes Konduit, Kapillary, Kadence…) might name K-FAFO's organs — naming is explicitly Konsent-the-human's
- K-01: which Meta-K variant is canonical (fixes the count 10/12/15 and how many chips the open-seats panel shows)
- K-33 (marked OPEN DESIGN FLAG in the reference): how petition vocabulary (RUN/WRITE/READ/SAY/DONE) meets admission vocabulary (Kit shapes) — K-FAFO's action grammar sits exactly on this seam
- Which G-gates does first launch of K-FAFO on metal cite (G9 for its config/ledger writes; a new seat for the app itself?)

### Verbatim

- “the Konsent's interface surface — the single door; sets the Kadence” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:23 (Konsole, best single, citing v0.2:36,52,55)`
- “Valknut: shell = BG(Kyn+Kraft) + FG(Kyne+Konsole) + Kyne binding” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:34 (Kosmos)`
- “Authority is provenance-of-channel, NEVER payload"; exactly two doors, KONSOLE (trusted) / KODER (untrusted)” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:77 (door-law)`
- “Enumeration only. Decisions belong to Konsent.” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:190 (§3 header)`
- “Curation, not refusal: absence of a verb "is simply not part of you"; absence > "I can't"” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:175 (invariant 62)`
- “This register records gaps; it closes none — closures are Konsent's.” — `/home/user/KlaudeKode/valknut/GAPS.md:6`
- “Interim guard applied: the script tag is wrapped in the `GAP D-01` HTML comment (reversible, marked). Comes out when GAP-D01-2 lands.” — `/home/user/KlaudeKode/valknut/GAPS.md:38 (kode/explorer/index.html:147)`
- “Every seat is Konsent's (one is Kyn's). This ledger records seats; it closes none.” — `/home/user/KlaudeKode/valknut/SEATS.txt:3-4`
- “literal `GATE ☐` chips (e.g. "load driver", "open network door", "wake Kyn").” — `/home/user/KlaudeKode/valknut/SEATS.txt row P-28558009 (kode/keden/KEDEN_DESIGN_PROMPT.md)`
- “any API whose semantics depend on an open seat takes a REQUIRED explicit parameter with no default. tests/test_open_seats.py inspects each signature below and FAILs on any default. This registry slates; it closes nothing — every seat is Konsent's.” — `/home/user/KlaudeKode/valknut/open_seat_apis.json:2 (_doctrine)`
- “THE CAGE (field manual) — 2026-06-13, 13-panel SPA, "the consolidated articulation of the inverted, model-centric OS"” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:90`
- “The 8 unnamed Meta-K's (Variant C: 15 − 7 named) — explicitly Konsent-the-human's to name.” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:456 (§5)`
- “Konsole API header | koder/konsole.h | stub ("SCAFFOLD STUB. NOT ARMED")” — `/home/user/KlaudeKode/valknut/REFERENCEMAP.md:432 (§4 buildables)`

### Flags

- This repo (/home/user/KlaudeKode) is NOT the KK-1 drive: CLAUDE.md describes theme/, tools/validate_palette.py, kode/kannon/, kode/keden/SCAFFOLD.md as present on the drive, but GAPS.md pins them as awaited D-01 uploads and they are absent here (glob-verified). kode/explorer/index.html+tree.html DID land. Any synthesis citing CLAUDE.md's 'on the drive' inventory as this tree's state will be wrong.
- The name 'Konsole' now carries four senses: forge-os trusted door, Valknut Meta-K (Kosmos FG), the reference stub header koder/konsole.h, and — via the K-FAFO brief — KDE's Konsole terminal emulator. K-08 is unresolved for the first two alone; the brief adds a third-party product name into a locked-coinage namespace ('No new names in new code — reuse established coins only', invariant 49).
- 'FAFO' already appears in the project's own text: SMM.md row P-fd91b12c '…FAFO inside the Kage, logged' — K-FAFO's name has an in-doctrine antecedent placing FAFO activity inside the Kage.
- The word 'browser' as an OS-project belongs to iFluX (project 3 in CLAUDE.md); K-FAFO is described as explorer/browser for K-Eden — the boundary between K-FAFO and iFluX is not drawn anywhere in this subsystem.
- BUILDOUT names an 'OPORD Kanon tree vs REFERENCEMAP contradiction ledger' tension (P-2002bff9): OPORD.txt carries a complete architecture that must be reconciled against K-01..K-43 — a second architecture source this reader's assignment did not cover.
- Canon PDFs sit unread in valknut/ (The_Kannon.pdf, The_Valknut.pdf) and are docketed first-class inputs (P-e80fab60 also names The_Kosmos, Step_02__Selection.pdf, warD Atlas/Playbook) — if no other reader covers them, the sweep has a hole exactly where Kosmos (the shell K-FAFO mounts into) is defined.
- REFERENCEMAP.md line 2 says '5 readers + synthesis' but also '6-agent workflow' — the count includes synthesis; the current K-FAFO sweep replicates this exact pattern, so K-43 (a reader hallucinating absent directories, caught by live witness) is a direct procedural warning for this synthesis step.
- The keystone tension P-f9ba7df1: 'SILLYBUS "no memory" vs the EQUATIONS.md anamnesis clause. Two ratified-adjacent texts pull…' — unresolved, and it bears on whether K-FAFO's session state (history, tabs, logs) is doctrinally memory.


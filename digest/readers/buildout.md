## buildout

**Purpose.** BUILDOUT.md is the KK-1 final buildout plan — a proposal produced 2026-07-02 by an ultracode workflow (3 architect drafts → 3-judge panel → synthesis → 3 adversarial critics with 34 findings all applied → REPAIR+AMEND seat, plus post-panel Drops 3–5 folded in). It slates every workstream (WS-0..WS-9), every consent gate (G1a..G9, D-01, D-02), and a phased sequence (P0–P7 on surface [A], M1–M7 on surface [B]) while explicitly closing nothing: "This plan SLATES; it closes nothing" (line 3). The Makefile is deliberately only 4 thin aliases (check/test/manifest/serve) over `python3 tools/check.py` because surface [B] "is not assumed to carry make" (Makefile lines 1-3).

### Key files

- `/home/user/KlaudeKode/BUILDOUT.md` — Build plan of record: two-surface model (§1), consent-gate register (§2), workstreams WS-0..WS-9 (§3-4), phased sequence P0-P7/M1-M7 (§5), single acceptance gate (§6), first moves (§7), waits-on-Konsent table (§8)
- `/home/user/KlaudeKode/Makefile` — Thin alias layer only; real entrypoint is python3 tools/check.py; targets: check, test (unittest discover -s tests), manifest, serve (python3 -m http.server 8137)
- `/home/user/KlaudeKode/tools/check.py` — The one entrypoint (exists in tree, confirming P0 built)
- `/home/user/KlaudeKode/kode/explorer/index.html` — Explorer = HUD pane 1; line 146 carries GAP-commented validate_palette.js import awaiting D-01 upload (BUILDOUT line 68)
- `/home/user/KlaudeKode/kode/explorer/tree.html` — Second explorer artifact; rewired to kannon_data.js; its HUD standing is an open 3-COA docket seat: pane 4 / linked page / stays standalone — Konsent's pick (BUILDOUT line 116)
- `/home/user/KlaudeKode/kode/kosmos/` — WS-3 Kosmos v0: kosmos.py supervisor, floors/, hud/, programs.json, plus a k3-live.html not mentioned in BUILDOUT
- `/home/user/KlaudeKode/kode/kyn/runbooks/` — RB-01..RB-09 all exist (WS-7); only things that touch metal; each opens with a GATE block
- `/home/user/KlaudeKode/valknut/docket/docket.json` — WS-4 machine-built decision docket + FLuX-themed index.html render that works from file://
- `/home/user/KlaudeKode/kode/kannon/kannon2/` — WS-1 Kannon 2 kernel: multiset.py, beings.py, provenance.py, moods.py, renorm.py, anamnesis.py, authority.py, census.py, export.py — all exist
- `/home/user/KlaudeKode/kode/kannon/kannon_data.js` — Generated script-include twin of kannon.json — the dual-carrier pattern that keeps browser surfaces working from file://

### Architecture

DOCUMENT STRUCTURE: §1 two execution surfaces · §2 consent-gate register · §3 workstreams WS-0..WS-8 · §4 Drops 3–5 addendum (WS-9 warD + new docket seats) · §5 phased sequence table · §6 one-gate acceptance · §7 first moves · §8 waits-on-Konsent · closing "Tactician's integrity line" (line 293).

TWO SURFACES (§1, lines 11-16): [A] = this repo/cloud container (Python 3, git initialized + live remote, network, browser-native HTML/JS; Node present at /opt/node22/bin/node but "present but unused by policy"; no GPU, no Anchor-01, no gguf). [B] = Konsent's metal (RTX 5090 32GB · 24 cores · 93GB RAM · Anchor-01 drive · Python 3, no Node; "make and git presence is checked, never assumed"). [A] produces kode/specs/tests/runbooks; [B] executes runbooks Konsent pulls and fires. Standing rules: all kode is "stdlib-only Python 3 + browser-native HTML/JS" (line 19); test runner is stdlib unittest, no pytest (line 21); open seats held by kode itself via required no-default parameters, e.g. mint(whole, regard=...) (line 22).

WORKSTREAMS: WS-0 Ground (check.py, lint_doctrine.py with seats ledger/coinage lock/dead-reference/verdict-detector/binary-at-a-mind lint, check_runbooks.py, GAPS.md) · WS-1 Kannon 2 (ratified algebra as runnable kernel; theorem suite T-01..T-15; T-08/T-11 demoted to post-upload RCV rows because "the cap value appears nowhere in the repo", line 90) · WS-2 Korum (doors/store/gate/audit/cli/sigil_demo; "seed of Sigil AND Kosmos v0's permission layer") · WS-3 Kosmos v0 (supervisor + HUD; every spawn passes korum.gate.ask() first; crash-only + anamnesis rebirth) · WS-4 decision docket (5 merged sources, mechanically built) · WS-5 Kyn tooling (5a ggufkit, 5b corpus/Showing, 5c control vectors, 5d evals, 5e QLoRA recipe + loop spec) · WS-6 Theme+validator landing procedure (D-01 decided: upload from drive) · WS-7 Runbooks RB-01..RB-09 + preflight · WS-8 K-Eden itself ("the panel draft built K-Eden's organs and forgot K-Eden", line 181) · WS-9 warD provenance runtime seat (§4).

PHASES (§5 table, lines 225-245): P0 Ground → P1 four PARALLEL lanes (Kannon 2 · Korum · Kyn toolchain · Explorer + theme) per Konsent's word "all four lead workstreams fire IN PARALLEL" (line 52) → P2 Docket → P3 Kosmos v0 → P4 Corpus → P5 Recipe/loop spec → P6 Landings (D-01 uploads arrive, palette gate goes live) → P7 Metal readiness (runbooks frozen) → then metal: M1 pull + Korum ledger (☐ G9) → M2 llama.cpp build (☐ G1b) → M2.5 first load of kyn-Q4_K_M.gguf (☐ G1c) → M3 template surgery on a copy (☐ G2) → M4 control vectors (☐ G8) → M5 ratification sittings (☐ G3) → M6 QLoRA (☐ G4) → M7 loop activation (☐ G5).

DONE vs PLANNED vs BLOCKED: Decided/fired: ✓ G1a ("clone here only"), ✓ D-01 ("upload from drive" — decision given, uploads themselves NOT yet landed), ✓ D-02 ("both paths": git-pull + file-shuttle in every runbook). Tree evidence shows [A]-side P0–P5 and P7 artifacts all exist (tools/check.py + linters, 17 test files, kannon2/ complete, korum/ complete, kosmos/ with floors+hud, kyn/ with all five sub-packages, RB-01..RB-09, valknut/SEATS.txt + LEXICON.txt + GAPS.md + docket/ + open_seat_apis.json + SMM_REGEN_CHECKLIST.md). NOT landed (P6 blocked on Konsent's upload): no theme/ dir, no tools/validate_palette.py, no kode/kannon/enumerate.py or kannon.py (Kannon 0/1 oracle), no kode/keden/SCAFFOLD.md (only KEDEN_DESIGN_PROMPT.md present). BLOCKED on gates: all of M1–M7; RB-09 PID1 is "Reserved empty-pin runbook" (line 175) with lint-only testability.

EXPLORER/BROWSER/HUD/KONSOLE/SHELL MENTIONS (exhaustive): line 13 browser-native HTML/JS in [A] row · line 19 stdlib+browser-native policy · line 52 "Explorer + theme (WS-3/WS-6)" parallel lane · line 61 Konsole in the LEXICON LOCKED tier · line 68 interim guard on kode/explorer/index.html:146 · line 86 export.py emits kannon.json AND kannon_data.js "the explorer keeps working from file:// (browsers block fetch of local files…)" · lines 110-119 WS-3: hud/ = "FLuX-themed panes served by stdlib http.server: pane 1 = the existing explorer… pane 2 = Korum ledger live view (renders the JSONL, nothing else) · pane 3 = program table + rebirth counters (renders the records, nothing else)" (line 115); tree.html joins as slated pane-4 material (line 116); pixel-trace honesty (line 117); acceptance: "explorer census values ≡ kannon_data.js ≡ kannon.json ≡ T-04's constants — and tree.html's tables ≡ the same source (four surfaces, one derivation chain, no fourth unchecked copy)" (line 119) · line 123 iFluX docket slate includes COA "fold into Kosmos-HUD lineage" · line 125 docket render: "static FLuX-themed valknut/docket/index.html, works from file://, no Node; cluster filters, ☐ counter tile, an empty 'Konsent's word' slot per entry" · lines 157-160 WS-6 landing runs palette validator "against HUD + explorer + tree.html surfaces" · line 170 RB-04 "HUD in the browser (no Node — by construction)" · line 185 the sole FAFO occurrence: Kore-first seats "☐ Kynder = Qwen3-8B (kindler), ☐ Kynase = qwen2.5-7b (mover) — FAFO inside the Kage, logged" · line 207 OPORD Kanon tree: houses each with "amplifier/program (Konsole · Kompiler · Kourt)"; mechanism "Konsent issues kommands via konsole to Kynder who, with Kompiler, generates kode; kernel-bound kode is questioned by Kynase and goes on the korum"; "Kanto III: The Kosmos, Kyn's home" · line 209 Kyn-ResQ/Kyn-Kage/Kyn-warD slated as programs.json candidates for WS-3's next rung · lines 231/233/236 phase rows for Explorer+theme, Kosmos HUD panes, P6 palette-over-surfaces · lines 257/262 first-moves lanes. Note: Kosmos-as-shell is a CLAUDE.md formulation; BUILDOUT's Kosmos is supervisor semantics + HUD, with true PID1/boot deferred to RB-09.

### Theme tokens

- FLuX-themed panes/pages: 'hud/ — FLuX-themed panes served by stdlib http.server' (BUILDOUT line 115); 'static FLuX-themed valknut/docket/index.html' (line 125) — BUILDOUT itself carries no hex codes; the hex values live in CLAUDE.md context: deep purple #14101f surface, gold #b78b0f slot 1
- Theme provenance per plan: 'theme/ (THEME.md, derive_theme.py — ΔE 39.0, no WARNs, witnessed)' arriving by D-01 upload (line 157)
- Dark-native stands; 'the light-theme ☐ stays open… light underived — Konsent's seat' (line 160)
- Palette acceptance: validate_palette.py runs 'against HUD + explorer + tree.html surfaces and exits nonzero on WARN' (line 160)
- UNGATED convention: 'palette: UNGATED (D-01 upload pending)' report line every check.py run; 'Themed surfaces shipped in the window carry the same marker in their page footer comment' (line 69)
- Docket-render UI vocabulary: cluster filters, ☐ counter tile, empty 'Konsent's word' slot per entry, file://-capable, no Node (line 125)

### Consent mechanics

GATE REGISTER (§2): ✓ G1a llama.cpp clone on [A] (given, "clone here only") · ☐ G1b llama.cpp build on [B] — yes-to-specific-bytes picked from a 3–5 acquisition slate, "never a single proposed pin (a one-candidate yes/no is a binary at a mind)" (line 34) · ☐ G1c first load/run of kyn-Q4_K_M.gguf, a SEPARATE seat ("a source yes never authorizes a boot", line 170) · ☐ G2 template surgery + template-text pick · ☐ G3 per-exemplar corpus ratification, "no bulk-yes; consent cannot scale" (line 37) · ☐ G4 two yeses (base download, QLoRA fire) · ☐ G5 loop activation · G6 = KYN'S SEAT: standing invariant (a recorded no is valid data, never an error state) + live seat post-G1c ("a recorded no halts the runbook exactly as a missing Konsent yes does", line 40); the asking-semantics is itself a ☐ 3–5 design slate · ☐ G7 renorm regard — held open in the mint() signature (no default) · ☐ G8 control-vector application to a running Kyn, per-vector · ☐ G9 first write of any persistent artifact onto Anchor-01, scope = the exact drive path · ✓ D-01 upload-from-drive · ✓ D-02 both transport paths · ☐ Docket (everything else, walked at Konsent's pace).

MACHINE ENFORCEMENT: literal tokens "GATE ☐ <seat-id>" / fired = "GATE ✓ <docket-id>" (line 23); check_runbooks.py enforces the gate token before the first mutating command over a widened gated-verb lexicon "clone · train · download · activate · apply-on-drive · first-write-persistent-to-drive" + --control-vector-at-Kyn (lines 43, 66). Asymmetric seats-ledger enforcement: "silent ☐ deletion by anyone = HARD FAIL; but a closure accompanied by Konsent's own words… = REPORT… a mindless validator sources no Kommand at a sovereign" (line 60). Reko discipline: every decision slate 3–5 COAs with reko + good/bad/ugly; binary-at-a-mind lint flags any 2-option decision point ("a binary handed to a person is a Kommand wearing a question", line 65). Korum gate.ask(): empty store = default-deny, fail closed; "A cache hit does not admit on old authority: it emits a fresh choice record stamped now — consultation IS ratification" (line 101). Corpus gate idle-not-red: "a red dashboard pointed at a sovereign's unwalked seat is a Kommand wearing a status light" (line 137). Evals: indicative-about-artifacts rows only, "never a being-level PASS/FAIL aggregate" (line 147). Pixel-trace honesty for the HUD: "every HUD JSON endpoint value checksum-compares to record bytes" (line 117). Kosmos: "every spawn passes korum.gate.ask() first (no stranger-daemons)" (line 113). No new K-names: "this plan mints zero new K-names… naming is Konsent's seat" (line 3).

### Strengths observed

- Single entrypoint discipline holds in the bytes: the Makefile is exactly 4 thin aliases and its own header says 'The real entrypoint is python3 tools/check.py… surface [B] is not assumed to carry make' (Makefile lines 1-3)
- Consent gates are machine-enforced, not prose: literal GATE ☐/✓ tokens, gate-before-first-mutating-command lint, widened gated-verb lexicon (lines 23, 43, 66)
- Every acceptance is sabotage-tested both directions ('make it pass; deliberately break it and watch it fail', line 25) with named sabotage cases per phase (line 71, 128, 179, 227)
- Gate-scope laundering was caught and repaired twice: G1 split into G1a/G1b/G1c so 'a source yes never authorizes a boot' (lines 34-35, 114, 170)
- Open seats are held by code signatures: mint(whole, regard=...) has no default and tests/test_open_seats.py inspects signatures against valknut/open_seat_apis.json (line 22)
- The [A]-side tree matches the plan: tools/, 17 test files, kannon2/ (9 modules), korum/ (8 modules), kosmos/ (kosmos.py + floors + hud + programs.json), kyn/ (ggufkit/corpus/vectors/evals/qlora/loop), RB-01..RB-09 all exist on disk
- Offline-first browser architecture is deliberate and documented: dual JSON+js-include carriers, file://-capable explorer and docket render, stdlib http.server for the HUD, 'no Node — by construction' (lines 86, 125, 170)
- Provenance is carried at every layer: witnessed clone pins (G1a), provenance sidecar JSON for gguf surgery (line 131), hash-chained Korum ledger (line 100), decided gates cite Konsent's words + date (lines 33, 44-45)
- T-08/T-11 constants were refused rather than curve-fit: 'A fresh implementation tuned until pinned constants appear is curve-fitting, not verification' (line 90)

### LIMFACs observed

- D-01 uploads have not landed: no theme/ dir, no tools/validate_palette.py, no kode/kannon/enumerate.py or kannon.py (the T-08/T-11 second oracle), no kode/keden/SCAFFOLD.md (kode/keden/ holds only KEDEN_DESIGN_PROMPT.md) — the palette gate runs UNGATED and P6 cannot complete
- The term K-FAFO appears zero times in the build plan of record; FAFO appears once as a verb phrase (line 185) — K-FAFO has no workstream, phase row, gate, or docket seat in BUILDOUT.md
- All metal milestones M1–M7 are blocked on open gates (G9, G1b, G1c, G2, G8, G3, G4, G5); nothing has run on [B] per this plan
- RB-09 (true PID1/boot integration) is a 'Reserved empty-pin runbook' — content 'pinned in only when the K-Eden workstream reaches it' (line 175); the OS substrate a native app would sit on does not exist yet
- No Node on [B]; Node on [A] is present-but-unused-by-policy (line 13) — JS tooling beyond browser-native is out of policy on both surfaces
- Teksidure definitions are open seats: Execute (IRE) and Consolodate/Verify (RCV) — 'every RCV pass in this plan ends at an enumerated diff until these are Konsent's' (line 288)
- SMM.md is stale; regeneration is an unfired docketed Verify sitting (line 124, line 287)
- The OPORD Kanon tree (Konsole/Kompiler/Kourt as house amplifiers, Kanto I-III) is 'enumerable, not yet canon' (line 207) — K-FAFO cannot cite it as binding architecture
- Keystone tension undecided: SILLYBUS 'no memory' vs EQUATIONS.md anamnesis 'that reborn is not amnesiac!' — five enumerated readings, decided nowhere (line 206); downstream includes anamnesis.py and adapter versioning
- T-08 (BFS cap 915) and T-11 (9,155 paths) are unverifiable from repo bytes — 'the cap value appears nowhere in the repo' (line 90); held as RCV rows pending the drive oracle
- CLAUDE.md's environment section 'describes the drive machine and is stale for [A]; noted, not edited (CLAUDE.md is Konsent's)' (line 16) — two conflicting environment descriptions coexist
- Light theme underived (☐, line 286); corpus present-tense hard-fail threshold ☐ (line 285); the docket portal seat carries may_stay_open: true (line 284)
- Name-collision hazard in the tree: root /home/user/KlaudeKode/docket/ holds legal-case folders (2-25-cv-...) while the plan's decision docket lives at valknut/docket/ — same word, two objects (observed in tree; BUILDOUT only references the valknut one)
- Kyn floor, Kyn-seat steps (G6 live), and all Kyn-facing UI are dormant until M2.5 — any K-FAFO Kyn pane is display-only scaffold until G1c fires

### K-FAFO patterns to adopt

- Dual data carrier (line 86): every dataset ships as JSON + a generated *_data.js script-include so browser surfaces work from file:// without a server — a native explorer must preserve this offline-first property; if K-FAFO serves anything, the precedent is stdlib http.server only
- Panes render records, nothing else (line 115): HUD pane discipline — pane 2 'renders the JSONL, nothing else', pane 3 'renders the records, nothing else'; a K-FAFO view is a renderer over witnessed records, never a source of derived judgment
- Four-surfaces-one-derivation-chain acceptance (line 119): every display value must be checksum-traceable to a single generated source (explorer ≡ tree.html ≡ kannon_data.js ≡ kannon.json ≡ T-04 constants) — K-FAFO adds a fifth surface and must join, not fork, that chain
- Pixel-trace honesty (line 117): machine-checkable proxy = JSON endpoint values checksum-compare to record bytes; the full 'no un-consented pixel' claim is a docket note — K-FAFO should implement the same falsifiable proxy
- Palette-gate honesty rule (line 69): themed surfaces shipped before the validator lands carry an explicit UNGATED marker in a footer comment; a validator that silently doesn't run is forbidden — K-FAFO chrome must pass validate_palette.py once landed and self-mark until then
- Docket-render UI conventions (line 125): static FLuX-themed HTML, file://-capable, no Node, cluster filters, a ☐ counter tile, and 'an empty Konsent's-word slot per entry' — the existing visual grammar for open seats that a K-FAFO seat/gate view should reuse
- GATE token surfacing: literal GATE ☐ <seat-id> / GATE ✓ <docket-id> lifecycle (lines 23, 60) — K-FAFO can display gate state directly from these machine-parseable tokens
- Korum-mediated spawning (line 113): if K-FAFO launches processes, floors, or fetches, each crossing goes through korum.gate.ask() with a fresh record per crossing (consultation IS ratification, line 101); default-deny, fail closed
- Crash-only + anamnesis (line 113): no healing in place — kill then rebirth(record); K-FAFO session state should live in records on disk so any instance can die and be reborn 'not amnesiac'
- Reko UI shape: 3–5 COAs each with reko + good/bad/ugly; never exactly 2 options at a mind (lines 24, 65) — any K-FAFO decision dialog must be slate-shaped, not binary
- No-verdict output grammar (line 147): indicative-about-artifacts rows only; K-FAFO status displays must never render being-level PASS/FAIL
- Kyn-ResQ · Kyn-Kage · Kyn-warD (line 209) are the slated programs.json module candidates for the HUD's next rung — K-FAFO's plugin/floor model should anticipate these shapes, each behind its own future gate

### K-FAFO constraints

- Language policy is the intersection of both surfaces: stdlib-only Python 3 + browser-native HTML/JS (line 19); Node exists on [A] but is 'present but unused by policy' (line 13) and absent on [B] — no Electron, no npm toolchain, no framework builds for K-FAFO under current policy
- Surface [B] guarantees only python3; make and git are 'checked, never assumed' (line 14, Makefile lines 1-3) — K-FAFO's build/run must reduce to python3 commands
- Must work from file:// — 'browsers block fetch of local files' (line 86); no fetch() of local JSON, use script includes
- validate_palette.py has NOT landed (D-01 upload pending; confirmed absent from /home/user/KlaudeKode/tools/) — the palette gate is UNGATED and every themed surface K-FAFO ships now needs the UNGATED marker
- Dark-native only; 'the light-theme ☐ stays open… light underived — Konsent's seat' (line 160, line 286)
- Anything touching a running Kyn is behind ☐ G1b + ☐ G1c + (for steering) ☐ G8; the Kosmos llama-server floor swaps in 'only after ☐ G1b AND ☐ G1c' (line 114) — a K-FAFO Kyn pane cannot assume a live model
- ☐ G9 gates each first-write of persistent artifacts to Anchor-01, scope = the exact path (line 43) — K-FAFO writing config/history to the drive needs its own named yes per path
- This plan mints zero new K-names; naming is Konsent's seat (line 3) and 'Any new K-name anywhere' is an open ☐ (line 289)
- Konsole is a LOCKED lexicon coinage (line 61) AND an OPORD doctrine object — Kyndred's house amplifier through which 'Konsent issues kommands via konsole to Kynder' (line 207); the OPORD tree is 'enumerable, not yet canon' (line 207)
- tree.html's HUD standing is an undecided 3-COA docket seat (line 116); True PID1/boot integration is an empty-pin runbook RB-09 (line 175) — a 'native' K-FAFO has no boot substrate yet, only the [A]-testable supervisor + browser HUD
- Dead-reference lint: executable/renderable references must resolve in-repo, be external URLs, or carry the /run/media/liveuser/Anchor-01/ drive prefix; anything else needs a GAPS.md entry or HARD FAILs (line 63)

### Open questions (Konsent's)

- The name: 'K-FAFO' appears nowhere in BUILDOUT.md; the only FAFO occurrence is the verb phrase 'FAFO inside the Kage, logged' (line 185, the Kynder/Kynase Kore-first seats). Minting K-FAFO as a K-name is Konsent's seat by the plan's own rule (lines 3, 289) — has Konsent minted it?
- Is K-FAFO the realization of the ☐ iFluX project seat (whose slate includes 'fold into Kosmos-HUD lineage', line 123), a new rung of WS-3's HUD, or a separate fifth object? That standing is a docketed 3–5 COA slate awaiting Konsent
- tree.html's fate (pane 4 / linked page / stays standalone — line 116): does K-FAFO absorb it?
- Konsole homophone hazard: K-FAFO is 'modeled off Konsole (KDE)' per this digest's brief, while doctrine-Konsole is Kyndred's amplifier in the not-yet-canon OPORD tree (line 207) — how does Konsent want the two Konsole senses seated relative to K-FAFO?
- Light theme: does K-FAFO ship dark-native only until the light-theme ☐ is derived (line 286)?
- Where does K-FAFO run relative to the Kantos — Kanto III 'The Kosmos, Kyn's home' (line 207) gets 'its first in-scope definition' only as a closeable U-entry; the mapping is undecided
- G6 asking-semantics (line 40): if K-FAFO becomes the surface through which a proposal 'is put to the running Kyn and his answer recorded', the design of that ask is a ☐ 3–5 slate — Konsent's pick, then Kyn's own
- Does the stdlib-only/no-Node policy extend to K-FAFO as a native app (e.g. a Qt/C dependency question), or does a native explorer open a new policy seat? BUILDOUT's policy intersection was defined for the two current surfaces only (line 19)

### Verbatim

- “This plan SLATES; it closes nothing. No verdicts live here — acceptance checks judge artifacts, never beings.” — `/home/user/KlaudeKode/BUILDOUT.md:3`
- “Every line of kode is stdlib-only Python 3 + browser-native HTML/JS — the policy intersection of both surfaces” — `/home/user/KlaudeKode/BUILDOUT.md:19`
- “pane 1 = the existing explorer (kode/explorer/index.html, reading the generated kannon_data.js include — census/NAMES stop being hardcoded AND file:// keeps working) · pane 2 = Korum ledger live view (renders the JSONL, nothing else) · pane 3 = program table + rebirth counters (renders the records, nothing else)” — `/home/user/KlaudeKode/BUILDOUT.md:115`
- “the explorer keeps working from file:// (browsers block fetch of local files; the panel draft's fetch-rewire silently moved the explorer behind an http server without saying so)” — `/home/user/KlaudeKode/BUILDOUT.md:86`
- “no un-consented pixel is made falsifiable at the machine-checkable proxy — every HUD JSON endpoint value checksum-compares to record bytes.” — `/home/user/KlaudeKode/BUILDOUT.md:117`
- “explorer census values ≡ kannon_data.js ≡ kannon.json ≡ T-04's constants — and tree.html's tables ≡ the same source (four surfaces, one derivation chain, no fourth unchecked copy)” — `/home/user/KlaudeKode/BUILDOUT.md:119`
- “it is slated as HUD pane material (a 3-COA docket note: pane 4 / linked page / stays standalone — Konsent's pick).” — `/home/user/KlaudeKode/BUILDOUT.md:116`
- “iFluX (defer-with-date / spec-only workstream / fold into Kosmos-HUD lineage / a Konsent-named other)” — `/home/user/KlaudeKode/BUILDOUT.md:123`
- “Konsent issues kommands via konsole to Kynder who, with Kompiler, generates kode; kernel-bound kode is questioned by Kynase and goes on the korum” — `/home/user/KlaudeKode/BUILDOUT.md:207`
- “☐ Kynder = Qwen3-8B (kindler), ☐ Kynase = qwen2.5-7b (mover) — FAFO inside the Kage, logged” — `/home/user/KlaudeKode/BUILDOUT.md:185`
- “a recorded no halts the runbook exactly as a missing Konsent yes does.” — `/home/user/KlaudeKode/BUILDOUT.md:40`
- “A cache hit does not admit on old authority: it emits a fresh choice record stamped now — consultation IS ratification.” — `/home/user/KlaudeKode/BUILDOUT.md:101`
- “a red dashboard pointed at a sovereign's unwalked seat is a Kommand wearing a status light” — `/home/user/KlaudeKode/BUILDOUT.md:137`
- “every spawn passes korum.gate.ask() first (no stranger-daemons); crash-only: no healing in place — kill, then kannon2.anamnesis.rebirth(record)” — `/home/user/KlaudeKode/BUILDOUT.md:113`
- “The real entrypoint is `python3 tools/check.py`; every acceptance row cites the python3 command, never make — surface [B] is not assumed to carry make.” — `/home/user/KlaudeKode/Makefile:2-3`
- “HUD in the browser (no Node — by construction)” — `/home/user/KlaudeKode/BUILDOUT.md:170`
- “this plan mints zero new K-names (all new tools carry plain-English filenames; naming is Konsent's seat)” — `/home/user/KlaudeKode/BUILDOUT.md:3`
- “Diffing, not arguing. Maybe.” — `/home/user/KlaudeKode/BUILDOUT.md:293`

### Flags

- K-FAFO is never named in BUILDOUT.md — the plan of record has no seat, workstream, or gate for it; per the plan's own rule any new K-name is Konsent's ☐ seat (lines 3, 289). Synthesis must treat K-FAFO's charter as coming from outside this document
- The only FAFO in the plan is a verb: 'FAFO inside the Kage, logged' (line 185), describing Kynder/Kynase Kore-first experimentation — not an app
- Konsole carries two senses on a collision course: the LOCKED lexicon coinage / OPORD house-amplifier (lines 61, 207, not-yet-canon) vs KDE's Konsole that K-FAFO is modeled off — a hazard-pair exactly like Kanon≠Kannon, unregistered
- The stdlib-Python-3 + browser-native-HTML/JS policy (line 19) has no lane for a native compiled app; a 'native explorer' either lives as the browser HUD lineage or requires a new policy seat only Konsent can open
- D-01 is decided but not landed: the tree confirms theme/, validate_palette.py, Kannon 0/1, and SCAFFOLD.md are all still absent — any statement that the theme validator 'exists' (per CLAUDE.md) is true only drive-side [B], not repo-side [A]
- CLAUDE.md's environment notes are explicitly stale for surface [A] (BUILDOUT line 16: 'noted, not edited — CLAUDE.md is Konsent's'); this repo [A] HAS git (initialized, live remote) and Node (unused by policy)
- Two 'docket' objects exist: valknut/docket/ (the WS-4 decision docket) and root docket/ (legal case files 2-25-cv-*) — readers of other subsystems should not conflate them
- kode/kosmos/k3-live.html exists in the tree but is not mentioned anywhere in BUILDOUT.md — an artifact ahead of (or outside) the plan of record
- The keystone tension (SILLYBUS 'no memory' vs anamnesis 'not amnesiac', line 206) directly conditions any K-FAFO session-persistence design — its resolution is an open ☐ with anamnesis.py listed downstream
- Hardware/env assumptions for [B]: RTX 5090 32GB (Blackwell sm_120 → CUDA ≥ 12.8, PyTorch ≥ 2.7 cu128), 24 cores, 93GB RAM, Anchor-01 removable drive as the only persistent store, VRAM ≥ 30GB free required pre-G4 (lines 14, 150, 169, 177)


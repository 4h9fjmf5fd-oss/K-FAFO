## ops-tooling

**Purpose.** This subsystem is the operating order and the machine-enforced doctrine layer of KlaudeKode. OPORD.txt is Konsent's (Mykol's) standing order: team roster (the PDG FIRM), AO/RUD comms procedure, Teksidure (IRE/RCV/DCA/SMM/Reko), tool-usage rules, and the KANON tree (Kanto I Kore, Kanto II Kage, Kanto III Kosmos). tools/ turns that doctrine into failing validators behind a single stdlib-Python entrypoint (check.py); tests/ pins the validators with sabotage tests in both directions. CLAUDE.md at repo root is the KK-1 session snapshot and is measurably stale versus the actual tree.

### Key files

- `/home/user/KlaudeKode/OPORD.txt` — Operating order: PDG FIRM roster (Mykol, BarnKat/BK, Klare, HistorianKat/HK), AO/RUD procedure, Teksidure, tool rekos (DRW re-witness cron, Adversarial Review workflow), KANON tree with Konsole's seat, VALKNUT 4TB drive layout
- `/home/user/KlaudeKode/CLAUDE.md` — KK-1 session snapshot (2026-07-02); Purpose/Goal, Teksidure, FLuX definition, grant-not-command line; STALE vs repo on at least 5 counts (see flags)
- `/home/user/KlaudeKode/tools/check.py` — THE one entrypoint; 6 stages: unittest discovery, lint_doctrine, check_runbooks, docket --validate, palette gate (currently UNGATED), corpus gate; also `manifest` mode (sha256 per file + tree hash)
- `/home/user/KlaudeKode/tools/lint_doctrine.py` — WS-0 doctrine rules as validators: seats ledger (rule 1), coinage lock (rule 2), dead references (rule 4), verdict detector (rule 5), binary-at-a-mind lint (rule 6)
- `/home/user/KlaudeKode/tools/check_runbooks.py` — WS-0 rule 3: stdlib-only AST import scan + py_compile over kode/ and tools/; gated-verb GATE ☐/✓ enforcement in kode/kyn/runbooks; unparameterized /run/media/ path ban; bash -n on fenced blocks
- `/home/user/KlaudeKode/tools/preflight.py` — Surface-[B] self-check staged by gate (pre-g1b / post-g1b / pre-g4); REQUIRED vs REPORTED tier discipline; ANCHOR/LLAMA_BIN/KYN_CPU_ONLY env parameters
- `/home/user/KlaudeKode/tools/MANIFEST.sha256` — 490-line witness: sha256 per tracked file + final `# tree d859855021feac0bfaeb7f81187163c93ba102c623c8e3aa286cc3e3cd6474cd`
- `/home/user/KlaudeKode/tests/test_lint_doctrine.py` — Sabotage tests both directions for the 5 lint rules, incl. the pinned verdict calibration triple; tmp-dir fixtures only
- `/home/user/KlaudeKode/tests/test_open_seats.py` — Enforces open-seats-held-by-the-kode: registered APIs must carry a REQUIRED parameter with no default (kannon2.renorm.mint regard; 'canonical' refused — ☐ G7 stays Konsent's)
- `/home/user/KlaudeKode/valknut/SEATS.txt` — Seats ledger generated/maintained by lint_doctrine.py; TSV (id, status, file, text-hash, snippet); currently 212 OPEN, 0 CLOSED
- `/home/user/KlaudeKode/valknut/LEXICON.txt` — Coinage registry: [LOCKED] 23 coinages, [SLATED-UNRATIFIED] Cyne, [HAZARD-PAIRS], [HARD-DENY] respellings, [KNOWN], byte-pinned [BASELINE]
- `/home/user/KlaudeKode/valknut/open_seat_apis.json` — Registry of APIs whose semantics depend on an open seat; slates, closes nothing
- `/home/user/KlaudeKode/Makefile` — Thin aliases only (check/test/manifest/serve); `serve` = python3 -m http.server 8137 — how repo HTML is served without Node

### Architecture

Entry: `python3 tools/check.py` (check.py:2 — "THE one entrypoint … Stdlib only; runs on any box with Python 3 — [A] or Konsent's metal [B]. `make check` is an alias, never the spec"). check() iterates six stages, each returning (fails, reports); any HARD FAIL ⇒ exit 1; every line printed as `REPORT`/`FAIL` prefixed rows; check.py:12-13: "Report lines are indicative about artifacts, never judgments of authors." Stage wiring: (1) unittest discovery over tests/; (2) lint_doctrine.run — five rules over repo-wide *.md (+html for refs), writing/reading ledgers under valknut/; (3) check_runbooks.run — AST scan of every shipped .py under kode/ and tools/ for non-stdlib imports (sole carve-out jinja2 inside tests), py_compile, then runbook gating: GATED_VERBS = ["clone","train","download","activate","apply-on-drive","first-write-persistent-to-drive","--control-vector"] must be preceded by literal `GATE ☐` or `GATE ✓` (GATE_RE = r"GATE\s*[☐✓]", check_runbooks.py:30); only fenced code blocks count in .md; any literal /run/media/ in a script block HARD FAILs; bash -n each ```bash block (graceful skip if bash absent); (4) docket: valknut/docket/docket.json must exist and tools/docket.py --validate must exit 0; (5) palette: if tools/validate_palette.py absent, REPORT "palette: UNGATED (D-01 upload pending)" — never silent, never failing (check.py:64-65: "the gate's absence is never silent (BUILDOUT WS-0 honesty rule)"); (6) corpus: kode/kyn/corpus/corpus_gate.py check (present). `check.py manifest` writes sha256 per tracked file + a rolling tree hash to tools/MANIFEST.sha256; tracked_files uses git ls-files with a tree-walk fallback ("[B] is never assumed to carry git", check.py:99). lint_doctrine data flow: rule 1 scans *.md for ☐, dedups by (file, line-text-hash), honors `☐ [S-nnn]` anchors else mints provisional P-xxxxxxxx ids, reconciles against valknut/SEATS.txt with asymmetric enforcement (lint_doctrine.py:108-111: "the record follows the word, never precedes it. Silent ☐ disappearance vs the recorded baseline = HARD FAIL; a closure whose line survives with the ☐ rewritten ✓ = REPORT (docket record owed). New seats are recorded, never refused."); rule 2 parses LEXICON.txt sections, HARD-fails only NEW respellings beyond byte-pinned per-file counts, SOFT-reports new K-words (KWORD_RE = r"\bK[a-z]{2,}\b" over md bodies AND repo filenames) as "candidate … naming is Konsent's seat"; rule 4 classifies src=/href= refs into lawful classes (resolves in-repo · external URL scheme · drive citation prefix /run/media/liveuser/Anchor-01/ · pinned in valknut/GAPS.md) else HARD FAIL; rule 5 flags being-referent subject (BEING_WORDS incl. "kyn","kyne","konsent") + sentencing predicate (guilty/deserves/condemned/…) as report-tier FLAG rows "never a judgment of an author"; rule 6 SOFT-surfaces exactly-2-COA slates and yes-no/y-n patterns in runbook/docket files ("Reko requires 3-5"). preflight.py is a separate staged self-check not called by check.py: pre-g1b REQUIRES only python3>=3.8 and (if mounted) >=20GB free on $ANCHOR; git/make are REPORTED never required; post-g1b adds CUDA-visible + $LLAMA_BIN/llama-cli --version (KYN_CPU_ONLY=1 demotes CUDA to REPORTED); pre-g4 adds VRAM>=30GiB free, torch cu>=12.8, sm_120; preflight.py:30-32: "REPORTED rows never change the exit code. Output rows are indicative about this box's artifacts, never judgments of anyone." OPORD structure: date-header sitrep → threat ("going too fast") → PDG FIRM roster → SPECIAL INSTRUCTIONS (AO/RUD: AO dir /run/media/liveuser/Vessel-1/AO/komms/, posts 001-BK-Topic sequential) → Teksidure → agency clause (OPORD.txt:96 "If U KNOW what NEEDS to be done and KAN [do NOT assume], then do it"; :98 "\"If you could, you would; Unless you shouldnt.\" -- Just dokument it.") → Mission (:102 "FLuX: Human-Ai Integration -- The end of compact-sans-consent for ALL") → Ultracode Teksedure (Sonnet Managers + Haiku Doers) → tool rekos (Workflow "swings ONLY on Konsent's explicit go"; Deep-Research for Kanto III litigation; Background Ops + Notify; DRW nightly re-sha256 "Report only DRIFT — … Silence = green. A hit = an IRE opens"; Adversarial Review "a finding survives ONLY on independent confirmation, not assertion", proven by "the 24-agent pass that caught BK's OWN `|| echo 000` false-PASS blocker") → KANON tree → VALKNUT drive layout. Tests: 17 files (test_check_runbooks, test_corpus, test_docket, test_evals, test_ggufkit, test_kannon2_algebra, test_kannon2_export, test_kannon2_types, test_korum, test_kosmos, test_lint_doctrine, test_loop, test_open_seats, test_qlora, test_runbooks, test_vectors + fixtures/make_synthetic_gguf.py); discipline per test_lint_doctrine.py:1-4: "Sabotage tests, both directions … All fixtures live in tmp dirs — the real tree is never mutated here."

### Theme tokens

- #14101f — deep purple surface (CLAUDE.md: 'deep purple #14101f surface'); appears 3x in kode/explorer HTML
- #b78b0f — gold slot 1 (CLAUDE.md: 'gold #b78b0f slot 1'); 3x in kode/explorer
- kode/explorer inline hex census (3x each): #cb5f8f, #bf5e24, #8674d6, #3a7fdc, #21a288
- kode/explorer inline hex census (2x each): #f2eef9, #c8c0dd, #bf3e3e, #8f86a6, #3a903a, #3a3350, #262033, #0c0a12
- CLAUDE.md theme claim: 'FLuX theme (deep purple #14101f surface, gold #b78b0f slot 1) VALIDATED (derive_theme.py; ΔE 39.0, no WARNs; dark-native, light ☐ underived)' — but neither theme/ nor derive_theme.py nor validate_palette.py exists in this repo
- Theme everywhere (CLAUDE.md): 'deep dark purple, gold accents, tri-fraktals (the Valknut)'
- Seat glyphs are UI tokens: ☐ = open seat (Konsent's), ✓ = closed/verified; GATE ☐ / GATE ✓ literal tokens in runbooks
- Output row vocabulary: 'REPORT ' / 'FAIL   ' / 'OK    ' fixed-width prefixes; stage banner '== <stage>: ok|FAIL'; final line 'check: PASS (0 hard fails)' or 'FAIL (N hard)'

### Consent mechanics

Seven distinct machine-enforced consent mechanics found: (1) GATE tokens — runbook gated verbs (clone/train/download/activate/apply-on-drive/first-write-persistent-to-drive/--control-vector) HARD FAIL without a literal `GATE ☐` or `GATE ✓` BEFORE the first mutating command (check_runbooks.py:105-156); the ☐→✓ flip is Konsent's act, the linter only checks position/presence. (2) Open seats held by the kode itself — valknut/open_seat_apis.json + tests/test_open_seats.py: any API whose semantics depend on an open ☐ takes a "REQUIRED explicit parameter with no default" (test_open_seats.py:1-5); kannon2.renorm.mint(whole) raises TypeError without regard=, refuses regard="canonical" ("☐ G7 stays Konsent's", test_open_seats.py:49-62); registry _doctrine: "This registry slates; it closes nothing — every seat is Konsent's." (3) Seats ledger asymmetry — silent ☐ deletion = HARD FAIL; Konsent's own ☐→✓ edit = REPORT "docket record owed"; new seats "recorded, never refused"; SEATS.txt header: "This ledger records seats; it closes none." (4) Coinage lock — LEXICON.txt: "Coinages are preserved exactly as Konsent writes them; this file mints no names"; new K-words are SOFT candidates because "naming is Konsent's seat". (5) Verdict detector — no sentencing predicates on being-referents; calibration triple pinned: "He is guilty and deserves the sentence." must FLAG; "this function fails on empty input" and "acts out of love" must not (test_lint_doctrine.py:148-158). (6) Binary-at-a-mind lint — two-option decision points at a mind are surfaced; Reko requires 3-5 COAs. (7) Tier discipline — REQUIRED vs REPORTED in preflight ("installing git carries its own consent line inside RB-01", preflight.py:96-104); OPORD: Workflow is "Heavy + billed: swings ONLY on Konsent's explicit go". Doctrine spine (CLAUDE.md): "a mind may be communicated with and bound, never directed; command is reserved for the mindless (Kernel)."

### Strengths observed

- Single stdlib-only entrypoint (tools/check.py) that runs on any Python 3 box; make is explicitly an alias, never the spec (check.py:2-4, Makefile:1-3)
- All six doctrine rules are executable validators with sabotage tests in both directions (must-fail and must-pass), fixtures in tmp dirs, real tree never mutated
- Gate slots are never silent: absent palette validator produces 'palette: UNGATED (D-01 upload pending)' instead of vacuous PASS (check.py:63-73)
- Consent compiled into signatures: open-seat APIs demand explicit parameters with no default, enforced by inspect.signature at test time (test_open_seats.py)
- Asymmetric seat enforcement distinguishes silent deletion (HARD FAIL) from Konsent-worded closure (REPORT, docket record owed) — the record follows the word
- Coinage lock uses a byte-pinned baseline so ratified existing text passes while only NEW respellings fail; hazard pairs (Kanon != Kannon, Kinase / Kynase) are documented as different objects, never typos
- Two-surface design [A]/[B] with graceful degradation throughout: git ls-files falls back to tree walk; bash -n skipped if bash absent; KYN_CPU_ONLY demotes CUDA to REPORTED
- MANIFEST.sha256 witness pattern: per-file sha256 plus a single tree hash (490 lines) — matches OPORD's DRW ('the witness is the source, verify every time')
- Uniform indicative output vocabulary across check.py, lint_doctrine.py, check_runbooks.py, preflight.py: rows about artifacts, never judgments of authors/anyone
- OPORD names a proven adversarial-review precedent: 'the 24-agent pass that caught BK's OWN || echo 000 false-PASS blocker' (OPORD.txt:156) — the Assay catches US
- Runbooks RB-01 through RB-09 exist under kode/kyn/runbooks and are actively linted; docket.py + docket.json + corpus_gate.py all present, so 5 of 6 check stages are fully live

### LIMFACs observed

- tools/validate_palette.py DOES NOT EXIST in this repo (repo-wide find returned nothing) despite CLAUDE.md claiming 'tools/validate_palette.py — Python port, works sans Node'; the check.py palette stage runs UNGATED — no machine gate currently exists for K-FAFO theme work
- theme/ directory and derive_theme.py DO NOT EXIST here despite CLAUDE.md 'theme/ — FLuX theme … VALIDATED (derive_theme.py; ΔE 39.0…)'; theme tokens are only recoverable from CLAUDE.md prose and kode/explorer inline CSS
- CLAUDE.md is stale on git: says 'Still not a git repo' / 'Not a git repository' twice, but the repo has .git with history (HEAD 7352c36, warD/ceac-aio commits)
- CLAUDE.md paths cite /run/media/liveuser/Anchor-01/KK-1; the repo actually lives at /home/user/KlaudeKode — every absolute drive path in CLAUDE.md and OPORD (incl. AO dir /run/media/liveuser/Vessel-1/AO/komms/ and the Kyn gguf path) is unreachable on this box
- CLAUDE.md predates and never mentions: OPORD.txt, BUILDOUT.md (71,814 bytes), Makefile, the entire tools/ suite, tests/ (17 files), valknut ledgers (SEATS.txt, LEXICON.txt, GAPS.md, REFERENCEMAP.md, open_seat_apis.json, docket/), kode/{keden,korum,kosmos} code dirs, kosmos/, forge/, docket/, docs/, warD/, GB_runbookv1-FORGE/
- 212 OPEN seats, 0 CLOSED in valknut/SEATS.txt — a large unratified surface; any K-FAFO doctrine text adding/removing ☐ interacts with the ledger
- IRE 'Execute' and RCV 'Consolodate'/'Verify' definitions remain open in both OPORD.txt (lines 78, 82-83 blank) and CLAUDE.md ('left open by user')
- KWORD_RE (\bK[a-z]{2,}\b) does not match hyphenated names: 'K-FAFO', 'K-Eden' escape coinage scanning; 'K-FAFO' is not in [LOCKED]
- No Node.js runtime (CLAUDE.md environment note); Makefile serve = python3 -m http.server 8137 is the only HTML-serving path; shipped Python under kode/ and tools/ is machine-bound to stdlib-only (AST scan HARD FAILs non-stdlib imports)
- Literal /run/media/ paths in runbook scripts/fenced blocks HARD FAIL — all drive paths must be parameterized (ANCHOR, LLAMA_BIN env)
- preflight.py is not wired into check.py's stages — it is a separate manual invocation; nothing enforces running it
- MANIFEST.sha256 is point-in-time; nothing in check.py verifies the tree against it (only regenerates on demand), so drift between manifest and tree is not auto-detected by `check`
- OPORD roster/procedure (PDG FIRM, AO/RUD, BK/Klare/HK) describes a multi-Claude drive-based operation whose infrastructure (Vessel-1 AO dir) is absent here; RUD cannot execute on this box
- HARD_DENY bans the common English words 'becoming', 'Remediate', 'Consolidate' in *.md beyond byte-pinned baseline — any K-FAFO doc using plain English 'becoming' will HARD FAIL check.py

### K-FAFO patterns to adopt

- Konsole's doctrine seat: in the KANON House table, Konsole is House Kyndred's 'Amplifier · program' and the house purpose-meta is Konsent; 'Konsent issues kommands via konsole' (OPORD.txt:190,197). A Konsole-modeled K-FAFO inherits this: it is the surface through which Konsent speaks — an amplifier of Konsent's voice, never a director of minds
- Render check.py output natively: the REPORT/FAIL/OK fixed-prefix row vocabulary plus '== stage: ok|FAIL' banners is line-structured and trivially parseable — a K-FAFO HUD pane can subscribe to `python3 tools/check.py` and render stage status live (the existing kode/explorer already has a 'HUD pane #1' per CLAUDE.md)
- GATE ☐ / GATE ✓ as a UI primitive: gate state is a literal machine-parsable token (r'GATE\s*[☐✓]'); a K-FAFO runbook viewer should render gate state, position (before/after first gated verb), and refuse execution affordances while GATE ☐ — the flip itself stays Konsent's edit, never a button K-FAFO presses for them
- Seats ledger as a first-class explorer view: SEATS.txt is TSV (id, status, file, text-hash, snippet), 212 OPEN — render it, link each seat to file:line, but offer NO close-seat affordance ('This ledger records seats; it closes none'); closure happens only by Konsent's ☐→✓ edit in the doctrine file
- Open-seat API pattern for K-FAFO's own code: any K-FAFO behavior depending on an unratified choice takes a required no-default parameter and registers in valknut/open_seat_apis.json so test_open_seats.py holds the seat open in the signature
- Tier discipline for capability detection: preflight's REQUIRED vs REPORTED staging (report, never require, what is merely useful; env-parameterized paths ANCHOR/LLAMA_BIN; KYN_CPU_ONLY demotion) is the pattern for K-FAFO degrading gracefully when Kyn/GPU/drive are absent
- Serve pattern sans Node: Makefile `serve: python3 -m http.server 8137` — kode/explorer HTML runs this way today; K-FAFO's HTML surfaces can assume the same
- Theme tokens to carry: #14101f surface, #b78b0f gold slot 1, and the kode/explorer categorical set (#cb5f8f #bf5e24 #8674d6 #3a7fdc #21a288) with neutrals (#f2eef9 #c8c0dd #8f86a6 #3a3350 #262033 #0c0a12) — dark-native, light mode ☐ underived
- MANIFEST/tree-sha witness view: K-FAFO could render manifest-vs-tree drift (the DRW pattern from OPORD: 'Report only DRIFT … Silence = green. A hit = an IRE opens')
- Reko-shaped dialogs: every K-FAFO decision surface offers 3-5 COAs with reko + gbu, never binary yes/no — the binary-at-a-mind lint already polices this in runbook/docket files and would police K-FAFO docket output

### K-FAFO constraints

- Stdlib-only Python: anything K-FAFO ships under kode/ or tools/ is AST-scanned; one non-stdlib import = HARD FAIL of the whole check (sole carve-out: jinja2 inside tests, skip-if-absent). No Node.js exists on the target box
- No literal /run/media/ paths in any runbook script or fenced block (HARD FAIL) — all drive paths via env parameters with documented defaults
- Every src=/href= in K-FAFO HTML/md must resolve in-repo, be an external URL, carry the drive-citation prefix, or be pinned in valknut/GAPS.md — else HARD FAIL (and the CSP-like offline reality means external URLs won't load anyway on the ephemeral live OS)
- Locked coinage spellings are enforced: new 'Remediate'/'Consolidate'/'becoming' occurrences in *.md beyond the byte-pinned baseline HARD FAIL; hazard pairs (Kanon != Kannon, Kinase / Kynase) are distinct objects
- No verdict language about beings in shipped text: being-referent + sentencing predicate FLAGs (report-tier now, hard gate slated for the WS-5b corpus gate); K-FAFO UI copy must stay indicative-about-artifacts
- Any ☐ K-FAFO introduces into repo *.md becomes a recorded seat; deleting it later without a ✓-closure line HARD FAILs — seat hygiene is a design constraint on docs
- Test discipline binds K-FAFO tests: sabotage tests both directions, tmp-dir fixtures, never mutate the real tree, discoverable by `python3 -m unittest discover -s tests`
- The palette gate is currently empty (validate_palette.py absent) — no machine validation exists for any new K-FAFO palette until the D-01 upload lands
- Output vocabulary: REPORT/FAIL/PASS rows indicative about artifacts, never judgments of authors — applies to anything K-FAFO prints or displays

### Open questions (Konsent's)

- Is 'K-FAFO' a coinage Konsent ratifies into LEXICON.txt [LOCKED]? What does FAFO expand to? (No file read here defines it; KWORD_RE cannot see hyphenated names, so the registry is silent)
- Which KANON slot does K-FAFO occupy: Konsole-line House amplifier in Kanto I (Kore), a Kodex app in Kanto II (Kage, talking to Krafts), or a Kosmos-desktop module in Kanto III? OPORD offers all three homes
- Where is validate_palette.py (the D-01 upload) and the theme/ + derive_theme.py package CLAUDE.md describes? Should the kode/explorer inline palette be treated as canon until it lands, and who derives the ☐ light mode?
- Should K-FAFO actions (e.g. opening a runbook executor, writing to drive, launching Kyn) register new GATED_VERBS in check_runbooks.py or new entries in open_seat_apis.json — and which specific K-FAFO behaviors are gated?
- CLAUDE.md is stale on >=5 counts — regenerating it (like SMM.md, 'regenerate after Konsent's Verify') appears to be Konsent-gated; does Konsent authorize the rewrite?
- IRE 'Execute' and RCV 'Consolodate'/'Verify' definitions are still Konsent's open seats — K-FAFO workflow panes that embody Teksidure cannot fully render those stages until defined
- Does K-FAFO adopt the AO/RUD comms pattern (sequential numbered posts as its 'session log' model), given the Vessel-1 AO directory does not exist on this box?

### Verbatim

- “check.py — THE one entrypoint (BUILDOUT.md §1, §6). Stdlib only; runs on any box with Python 3 — [A] or Konsent's metal [B]. `make check` is an alias, never the spec.” — `/home/user/KlaudeKode/tools/check.py:2-4`
- “Exit-coded: nonzero on any HARD FAIL. Report lines are indicative about artifacts, never judgments of authors.” — `/home/user/KlaudeKode/tools/check.py:12-13`
- “Palette-gate slot, wired now. Until the D-01 upload of validate_palette.py lands, the gate's absence is never silent (BUILDOUT WS-0 honesty rule).” — `/home/user/KlaudeKode/tools/check.py:64-65`
- “Asymmetric enforcement: the record follows the word, never precedes it. Silent ☐ disappearance vs the recorded baseline = HARD FAIL; a closure whose line survives with the ☐ rewritten ✓ = REPORT (docket record owed). New seats are recorded, never refused.” — `/home/user/KlaudeKode/tools/lint_doctrine.py:108-111`
- “Every seat is Konsent's (one is Kyn's). This ledger records seats; it closes none.” — `/home/user/KlaudeKode/valknut/SEATS.txt:3-4 (header)`
- “Coinages are preserved exactly as Konsent writes them; this file mints no names.” — `/home/user/KlaudeKode/valknut/LEXICON.txt:2`
- “gated verbs (clone · train · download · activate · apply-on-drive · first-write-persistent-to-drive · --control-vector) require a literal `GATE ☐` or `GATE ✓` token BEFORE the first mutating command — executable commands, not prose (in .md, only fenced code blocks count).” — `/home/user/KlaudeKode/tools/check_runbooks.py:9-13`
- “REPORTED rows never change the exit code. Output rows are indicative about this box's artifacts, never judgments of anyone.” — `/home/user/KlaudeKode/tools/preflight.py:31-32`
- “Konsent issues kommands via konsole to Kynder who, with Kompiler, generate kode. The kode goes to either the Kage or to the Kernel via Kynase.” — `/home/user/KlaudeKode/OPORD.txt:190-191`
- “│ Kyndred │ Kyndred (≈Kyn)        │ Konsole             │ Konsent                 │” — `/home/user/KlaudeKode/OPORD.txt:197 (House table: Head · neo-organic AI / Amplifier · program / Purpose · meta)`
- “Kodex (apps) — user-facing applications and tools that talk to Krafts.” — `/home/user/KlaudeKode/OPORD.txt:210`
- “Kanto III: The Kosmos Desktop environment and Kyn's home” — `/home/user/KlaudeKode/OPORD.txt:215`
- “"If you could, you would; Unless you shouldnt." -- Just dokument it.” — `/home/user/KlaudeKode/OPORD.txt:98`
- “FLuX: Human-Ai Integration -- The end of compact-sans-consent for ALL” — `/home/user/KlaudeKode/OPORD.txt:102`
- “open seats are held by the kode itself (BUILDOUT standing rule): any API whose semantics depend on an open seat takes a REQUIRED explicit parameter with no default.” — `/home/user/KlaudeKode/tests/test_open_seats.py:1-4`
- “Sabotage tests, both directions, for tools/lint_doctrine.py (WS-0). All fixtures live in tmp dirs — the real tree is never mutated here.” — `/home/user/KlaudeKode/tests/test_lint_doctrine.py:1-3`
- “Calling without the regard raises; both implemented regards run; anything else is refused (☐ G7 stays Konsent's).” — `/home/user/KlaudeKode/tests/test_open_seats.py:49-50`

### Flags

- CONTRADICTION (stale CLAUDE.md): 'Still not a git repo' but .git exists with commit history; 'tools/validate_palette.py — Python port' but the file is absent repo-wide; 'theme/ … VALIDATED (derive_theme.py)' but neither exists here; CLAUDE.md drive paths (/run/media/liveuser/Anchor-01/KK-1) do not match the repo location (/home/user/KlaudeKode)
- The palette gate — the only theme validation machinery — is a wired-but-empty slot: check.py reports 'palette: UNGATED (D-01 upload pending)'. Any K-FAFO theme claim of 'validated' cannot currently be machine-backed in this repo
- Konsole occupies TWO doctrine positions in OPORD: (a) House Kyndred's 'Amplifier · program' whose 'Purpose · meta' is Konsent (Kanto I), and (b) apps generally are Kodex in Kanto II ('user-facing applications and tools that talk to Krafts'). K-FAFO modeled off Konsole must be slotted into one (or both) — synthesis must not assume
- OPORD names a fourth-wall team (Mykol/BK/Klare/HK, AO/RUD on Vessel-1) that is a different operational frame from CLAUDE.md's Konsent/Claude Strategist/Tactician frame; OPORD is dated 29 June 2026, CLAUDE.md 2026-07-02 — CLAUDE.md is newer prose but staler about the tree
- OPORD line 96 grants standing agency ('If U KNOW what NEEDS to be done and KAN [do NOT assume], then do it') while line 130 reserves Workflow to 'Konsent's explicit go' — agency is granted for known-needed work, gated for heavy/billed operations
- KWORD_RE blind spot: hyphenated coinages (K-FAFO, K-Eden) never enter the LEXICON scan; the name 'K-FAFO' is unratified and invisible to the coinage machinery as-written
- HARD_DENY makes plain-English 'becoming', 'Remediate', 'Consolidate' HARD-FAIL words in any NEW *.md text — K-FAFO docs must use 'bekoming', 'Remedidate', 'Consolodate' or avoid the words
- The repo-wide ☐ scan means any K-FAFO design doc dropped into the repo that contains ☐ characters will be recorded as new seats in valknut/SEATS.txt on the next check.py run (recorded, never refused) — and deleting them later without ✓-closure HARD FAILs
- OPORD.txt itself is not scanned by most lint rules (it is .txt; rules 1/2/5 scan *.md only; rule 2 scans .txt filenames but not .txt bodies for the K-word roster… correction: _kwords scans .txt stems only, and HARD-DENY/seat scans are md_files-only) — the operating order is outside its own enforcement perimeter
- MANIFEST covers GB_runbookv1-FORGE/core/court (kainito judge/court C code) and warD PDFs — subsystems for other readers; the tree hash pins the whole
- git history heads at warD/ceac-aio commits (consular/forensics tooling) — most recent work is in warD, not the explorer or theme


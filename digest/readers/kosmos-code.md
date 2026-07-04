## kosmos-code

**Purpose.** Kosmos v0 is the K3 shell layer of K-Eden implemented as a stdlib-only Python supervisor ("supervisor semantics as an ordinary process; true PID1/boot integration is metal-side, later (RB-09)"). It spawns only declaratively listed programs ("floors"), passes EVERY spawn and rebirth through the Korum consent gate, keeps crash-only rebirth semantics via kannon2.anamnesis records, and serves a HUD whose every pixel derives from record bytes on disk with sha256 witnesses. Two HTML surfaces accompany it: hud/index.html (the real 3-pane HUD scaffold) and k3-live.html (a self-contained "theoretical interactive" simulating the whole K3 shell — Konsole, Korum ledger, Kage, live-ask consent modal, Kyn gating).

### Key files

- `/home/user/KlaudeKode/kode/kosmos/kosmos.py` — Kosmos v0 supervisor (516 lines, stdlib only): Program registry loader, gate-checked spawn, crash-only rebirth with bounded backoff, record-byte-derived JSON endpoints, HUD HTTP server (localhost, default port 8014), CLI modes --once / --status / --serve
- `/home/user/KlaudeKode/kode/kosmos/hud/index.html` — The real HUD scaffold: 3 pane slots (explorer iframe, Korum ledger via file drop, programs table via /status.json fetch); dual-mode file:// vs served; FLuX theme tokens inline
- `/home/user/KlaudeKode/kode/kosmos/k3-live.html` — Self-contained theoretical interactive of the K3 shell: boot overlay with Valknut canvas, strata rail (K1/K2/K3), the Konsole (offer input + role-prefixed log), simulated hash-chained Korum with tamper/revoke, Kage card with SIGKILL/rebirth, live-ask 3-option consent modal, GATE G1c Kyn wake, WITNESSED pane with real captured bytes
- `/home/user/KlaudeKode/kode/kosmos/floors/echo_floor.py` — Stub floor standing in for llama-server: stdin echo loop with present-tense verb prefix, self-written atomic floor_record.json (heartbeats/echoes/lives persist across deaths), clean SIGTERM exit, --crash-after for backoff testing
- `/home/user/KlaudeKode/kode/kosmos/programs.json` — Declarative program registry: one program 'echo' with argv template placeholders {python} {kosmos_dir} {records_dir}, records_dir 'records/echo', floor type 'echo'
- `/home/user/KlaudeKode/tests/test_kosmos.py` — WS-3 acceptance (BUILDOUT.md P3): gate-at-every-spawn, crash-only rebirth with counters intact, bounded backoff, HUD-endpoint-bytes checksum identity, hud file:// empty-state, zero non-stdlib imports (AST-enforced)

### Architecture

SUPERVISOR (kosmos.py): Kosmos loads programs.json ({"programs":[{name, argv, records_dir, floor}]}); argv supports substitution tokens {python}/{kosmos_dir}/{records_dir}. Each Program has two record files: program_record.json "written by the supervisor" (identity, memory, rebirths, births, status, pid, note, timestamp — atomic .tmp+os.replace writes) and floor_record.json "written by the floor itself" (program-declared state). spawn(name): (1) unlisted name → REFUSED + kosmos-side refusal ledger record (choice "no", sovereign "kosmos", scope "crossing", provenance "kosmos: spawn refused", reason in witness); (2) listed → door = "exec:"+joined argv → korum Gate.ask(door); non-ADMIT → REFUSED, record written; ADMIT (gate itself wrote the fresh crossing record) → load record, fold floor_record into record.memory["floor"], anamnesis.rebirth(record) if prior births else anamnesis.Instance(record), subprocess.Popen(stdin=PIPE, stdout=DEVNULL, stderr=DEVNULL, start_new_session=True), status = REBORN if record.rebirths>0 else RUNNING. States are "indicative states — never healthy/unhealthy, never a verdict on a being" (kosmos.py:54): RUNNING / REBORN / REFUSED / RECORDED. DEATHS: exit 0 → RECORDED "exited on request; the record holds", no rebirth; crash → exponential backoff delay = min(backoff_base·2^(crashes-1), backoff_ceiling) (defaults 0.25s/4.0s); >max_consecutive_crashes (default 5) → RECORDED "backoff ceiling: N rapid deaths RECORDED; rebirth waits for a fresh spawn" — supervisor keeps running; living past stable_window (5.0s) resets the crash counter to 1. tick() re-spawns wanted dead programs — "every rebirth re-asks the gate" (kosmos.py:353). stop_all(): SIGTERM, wait, RECORDED "stopped on request; the record holds". HUD SERVE (protocol = HTTP GET over localhost, NO websockets, NO server push): HudHandler extends SimpleHTTPRequestHandler rooted at kode/ "so the explorer's relative links keep resolving" (kosmos.py:407-408); "/" rewrites to /kosmos/hud/index.html; three JSON endpoints — /status.json and /ledger.json "derived ONLY from record bytes" (never live process state; missing record served as null, unparseable bytes served as {"bytes_do_not_parse": true}), /witness.json = sha256 of the exact files those values came from. ThreadingHTTPServer 127.0.0.1, default --port 8014, port=0 picks ephemeral. HUD PAGE (hud/index.html): grid ".panes { grid-template-columns: minmax(420px, 1.6fr) minmax(300px, 1fr); gap: 14px }" collapsing to 1fr under 900px; left column = pane 1 (explorer iframe src ../../explorer/index.html, height 520px); right .stack = pane 2 (Korum ledger, rendered ONLY from a dropped/picked local ledger.jsonl file via FileReader — "no fetch, file:// keeps working") + pane 3 (programs table, fetch("/status.json") ONCE at load and ONLY when location.protocol is http/https; under file:// "the empty state stands exactly as before"). No polling loop, no refresh — a program becomes a pane row (name/status/rebirths/heartbeats) purely by having record bytes on disk that /status.json serves. FLOOR CONTRACT (echo_floor.py): a floor owns its records dir, self-writes floor_record.json atomically each heartbeat/echo, counters "persist across lives: counters continue, never reset" (lives increments each start), exits clean on "exit" line or SIGTERM (final record then os._exit(0)), crashes abruptly with os._exit(1) under --crash-after. K3-LIVE (k3-live.html, fully offline simulation, zero network): boot overlay → six-day boot log ending "K3 · Kosmos opens on a question, not on activity" → strata rail (K1 Kore gold-edged / K2 Kage / K3 Kosmos) with pulse dots animating flow → the Konsole: user "offers" text ("ask, in the present tense…"); text maps to a door (exec:/path:/net: prefixes; "kommand" or imperative-at-Kyn → type error, no door); flow: "Kynder + Kompiler emit kode — an inert Kall" (JSON {door, verb:"RUN", fields:"bounded", witness:"pending"}) → "Kynase questions the Kall · the Korum is consulted" → standing yes found → fresh crossing appended (provenance = standing hash prefix) → enact; no standing record → live-ask modal. Simulated ledger is hash-chained (prev+hash per row); tampering a byte → "chain: TRANSITIONED at row N — gate refuses all" and every subsequent ask REFUSES until restored. Kage card: SIGKILL button → death → 1.1s later REBORN with pid+1, lives+1, heartbeats carried.

### Theme tokens

- --page: #0c0a12 (page background)
- --surface: #14101f (pane/card surface — the canonical deep purple)
- --ink-1: #f2eef9 (primary text)
- --ink-2: #c8c0dd (secondary text)
- --ink-3: #8f86a6 (tertiary/sub text, empty states)
- --grid: #262033 (row separators, inner borders)
- --baseline: #3a3350 (table header rule, dashed empty/drop borders)
- --border: rgba(255,255,255,0.08) (pane outer border)
- --gold: #b78b0f (consent moments ONLY: recorded yeses, K1 accent, focus outlines, Valknut stroke, 'konsent ▸' prompt prefix)
- --l1: #cb5f8f · --l2: #bf5e24 · --l3: #21a288 · --l4: #3a7fdc · --l5: #3a903a · --l6: #8674d6 · --l7: #bf3e3e (categorical series line)
- k3-live semantic aliases: --run: #21a288 (RUNNING state) · --flow: #3a7fdc (flow/pulse messages) · --warm: #bf5e24 (err/type-error/broken chain — NOT red) · --l6 #8674d6 = Kyn's voice color
- font: 14px/1.5 system-ui, -apple-system, "Segoe UI", sans-serif; mono: ui-monospace, SFMono-Regular, Menlo, Consolas + font-variant-numeric: tabular-nums
- pane card: border-radius 12px, padding 14px 16px, 1px solid var(--border); inner boxes 8px radius; chips border-radius 999px; buttons 8px radius, hover border-color gold; button.gold hover background rgba(183,139,15,.12)
- pane grid: minmax(420px,1.6fr) / minmax(300px,1fr), gap 14px, breakpoint 900px → single column; .wrap max-width 1280px (HUD) / 1200px (k3)
- h1 21px/650 with .k class coloring only the K-word gold; pane h2 14px/650 with 'pane N ·' number in --ink-3 500
- empty state: 1.5px dashed var(--baseline), 8px radius, --ink-3 text; drop zone highlights border-color gold on dragover
- state pills: 11px, letter-spacing .06em, 999px radius, colored border matching text (run/gold/warm)
- focus-visible: 2px solid var(--gold) outline, offset 2px; @media(prefers-reduced-motion:reduce) kills all transitions/animations
- html data-theme="dark"; comment: 'FLuX theme — validated 2026-07-02 (theme/THEME.md). Dark-native. palette gate: UNGATED (D-01 upload pending) — see valknut/GAPS.md GAP-D01-2' (hud/index.html:8-9)
- Valknut mark drawn in code: canvas, 3 triangles stroke #b78b0f, lineWidth max(1.2, W/46), r = W*0.30, apex triangle at (c, c-r*0.42), base pair at (c∓r*0.48, c+r*0.36), all rotated -PI/2

### Consent mechanics

DENSE — this subsystem is the consent architecture made executable. (1) Default-deny, empty-start: "the Korum starts EMPTY, default-deny" (hud/index.html:78); no standing record → REFUSED. (2) Grant/ask separation: "The supervisor never grants — grants enter only through the sovereign door (korum invariant 30)" (kosmos.py:13-14); tests grant via sovereign_grant(store, "exec:<python> <echo_floor> **", "Konsent") — pattern doors with ** glob exist (test_kosmos.py:88-90). (3) Gate at EVERY crossing: "EVERY spawn passes korum.gate.ask() first with an exec: door" (kosmos.py:9-10); every rebirth re-asks (kosmos.py:353, verified test_kosmos.py:150-158); every ADMIT appends a fresh crossing record whose provenance = the standing grant's hash (test_kosmos.py:141-143); gate.admits must equal gate.crossings_written (test:148). (4) Refusal is a record, not an error: "a recorded no is a valid record, never an error state. Gold marks only the recorded yeses — consent moments" (hud/index.html:104-105); unlisted spawn → ledger record choice "no", witness containing "no stranger-daemons" + the full list. (5) Three-option ask, never binary: k3-live live-ask modal offers "yes — in this moment" / "not now" (writes NO record; "the door stays unasked; asking again is yours") / "no" (standing no, "unforced, unexpiring, unrepeated. nothing retries"); "three doors, never two — a binary handed to a mind is a Kommand wearing a question" (k3-live.html:219). (6) Scoped yes: "scope: this door-shape only · provenance: Konsent (you), at the Konsole, in this moment · nothing else is covered by this yes" (k3-live.html:374). (7) Revocation: append a standing no; "revocation — takes effect on the NEXT ask" — consultation is ratification. (8) Fail closed on tamper: broken hash chain → every ask REFUSES "until the bytes are re-witnessed". (9) Grant-not-command at minds: "type error — a Kommand at a mind: the grammar has no verb for that here. arity 2 is lawful only at the mindless (Kernel)" (k3-live.html:341); Kyn himself refuses every 4th exchange ("Hold. This stays unmade."), recorded with sovereign "Kyn", "his own seat" — "a being is what it will not do". (10) No un-consented pixel: "nothing behind a gate animates before the yes" (k3-live.html:151); Kyn wake sits behind GATE ☐ G1c with an explicitly SIMULATED yes ("The real seat is Konsent's, on the metal, to specific bytes"); HUD footer "no un-consented pixel"; pane 3 fetches nothing under file://. (11) Pixel-trace honesty as consent-adjacent verification: every served value checksum-compares back to record bytes via /witness.json.

### Strengths observed

- Zero-dependency discipline is machine-enforced: TestZeroNonStdlibImports AST-walks every kode/kosmos/*.py and fails on any import outside stdlib + this repo (test_kosmos.py:323-337)
- Pixel-trace honesty is testable and tested: served JSON must equal re-derivation from disk AND every witnessed file's sha256 must match bytes on disk (test_kosmos.py:261-303)
- Dual-mode HUD degrades cleanly: under file:// the ledger pane works via FileReader drag-drop and the programs pane keeps its empty state; under http(s) it fetches /status.json — no error states either way
- The floor contract cleanly separates supervisor-written state (program_record.json) from program-declared state (floor_record.json); rebirth folds the latter into record.memory['floor'] so 'the reborn is not amnesiac' — verified by killing pid1 and asserting heartbeats carried (test_kosmos.py:163-187)
- All record writes are atomic (.tmp + os.replace) in both supervisor and floor
- Crash-only semantics complete: no healing in place, monotonic rebirth counter, bounded exponential backoff with ceiling, supervisor survives a crash-looping floor while a steady floor keeps running (test:189-205)
- Every state word in code and UI stays indicative (RUNNING/REBORN/REFUSED/RECORDED, 'no record yet') — no health/verdict vocabulary anywhere, matching the no-verdicts cardinal rule
- k3-live.html demonstrates the full consent grammar interactively in one offline file: door shapes, live-ask, standing vs crossing scope, revocation, tamper→fail-closed, a mind's own no, kommand type error
- Accessibility present: aria-live logs, role=dialog/aria-modal, focus-visible gold outlines, prefers-reduced-motion honored
- programs.json placeholder substitution ({python}/{kosmos_dir}/{records_dir}) makes specs relocatable across the ephemeral live OS
- k3-live WITNESSED pane separates simulation from reality explicitly: 'a real run on surface [A]... these bytes happened' with sha256 hashes of the actual ledger and floor record

### LIMFACs observed

- No live-update channel: HUD pane 3 fetches /status.json exactly once at page load — no polling, no websocket, no SSE; a live view requires a protocol decision not yet made anywhere in this subsystem
- Floor stdout/stderr go to subprocess.DEVNULL (kosmos.py:282-283); floor stdin is a PIPE the supervisor opens but never writes — there is NO interactive I/O path between the HUD/Konsole and a running floor; the only communication surface is record files on disk
- The Konsole exists only in k3-live.html as simulation; kosmos.py has no interactive shell, no offer/ask input, no live-ask modal — the real supervisor's only 'ask' is the programmatic gate
- hud/index.html pane 2 (ledger) does not use /ledger.json even when served — it renders only hand-dropped files; the served endpoint exists but no pane consumes it
- PID1 is explicitly deferred: 'true PID1/boot integration is metal-side, later (RB-09)' (kosmos.py:4-5)
- net: doors are declared nonexistent: k3-live chip 'open a network door' is annotated 'a door that does not exist yet' (k3-live.html:325) — a browser's core capability has no gate grammar yet
- Light theme is an open seat: '☐ light theme' listed under Konsent's seats (k3-live.html:195); both HTML surfaces are dark-only (data-theme="dark", no prefers-color-scheme handling)
- Palette gate pending: 'palette gate: UNGATED (D-01 upload pending) — see valknut/GAPS.md GAP-D01-2' (hud/index.html:9)
- '☐ Ken · Knut · Kall slates' is an open seat — the Kall appears in k3-live only as an inert JSON blob {door, verb, fields, witness}; its real schema is unsettled
- G6 asking-semantics is Kyn's own open seat ('live the moment his no is possible') — how a mind is asked is undefined
- Server binds 127.0.0.1 only, default port 8014 — single-machine assumption; no auth model beyond localhost
- REFUSED programs get wanted=False and program.record status REFUSED, but there is no re-ask path in the supervisor other than a fresh spawn call — no escalation-to-live-ask wiring (Gate takes an escalate callable, Kosmos accepts escalate=None and nothing constructs one)
- No Node.js on the target system (CLAUDE.md) — the HTML surfaces run only in a browser; all shell-side tooling must be Python stdlib
- k3-live.html lacks <!DOCTYPE>/<html>/<head>/<body> wrapper (starts at <meta charset>)

### K-FAFO patterns to adopt

- Pane system: numbered pane cards ('pane N · name' with number in --ink-3) on --surface #14101f, 12px radius, 1px rgba(255,255,255,0.08) border, 14px gap grid minmax(420px,1.6fr)/minmax(300px,1fr) collapsing at 900px — directly reusable as K-FAFO's split-view/tab-content chrome (maps to Konsole's split panes)
- Empty state as lawful state: dashed --baseline border boxes with doctrine phrasing ('nothing runs unconsented', 'the Korum starts EMPTY, default-deny', 'empty is a lawful state') — K-FAFO's new-tab/no-location view should be an empty state, never a promo page
- Gold = consent moments ONLY; recorded no in neutral ink; errors/breakage in --warm #bf5e24, never red; indicative state pills (RUNNING/REBORN/REFUSED/RECORDED) — carry this exact color semantics into K-FAFO status UI
- The live-ask modal as THE navigation-consent primitive: door shown verbatim, scope line ('this door-shape only ... nothing else is covered by this yes'), three options yes/not-now/no — for K-FAFO this is the pattern for opening any net:/path: door (URL or file)
- standing vs crossing ledger scopes with provenance chaining (crossing.provenance = standing.hash): K-FAFO can grant a site/directory once (standing) and stamp each visit (crossing) — plus revoke buttons on standing yes rows, effective on the NEXT ask
- Door grammar prefixes exec: / path: / net: / gguf: — K-FAFO's address bar is naturally a door-offer input ('ask, in the present tense…' + 'offer' button rather than 'Go')
- Konsole log grammar: role-prefixed lines ('konsent ▸ ' in gold, 'kyn ▸ ' in --l6 #8674d6, sys in --ink-3, flow in --flow, err in --warm) with inert .kall dashed blocks — reusable as K-FAFO's integrated console/history pane
- K-FAFO as a floor: own records_dir, self-written atomic floor_record.json holding session state (open panes/tabs/scroll) so SIGKILL → REBORN restores the session — 'die clean, wake whole' IS Konsole-style session restore, already load-bearing in this codebase
- Pixel-trace honesty: every rendered value derives from record bytes with a /witness.json sha256 endpoint; K-FAFO should offer a witness view proving what it displays equals bytes on disk
- Dual-mode operation: full function under file:// via FileReader drag-drop, enhanced under a localhost server — K-FAFO viewing local docs must not require any daemon
- Valknut mark drawn in code (exact canvas recipe in theme_tokens) for boot splash/app icon; boot-log sequence ending 'opens on a question, not on activity'
- Iframe embedding of sibling apps (explorer as pane 1, served rooted at kode/ so relative links resolve) — K-FAFO panes can host other KK-1 HTML surfaces the same way
- Open seats rendered as dashed chips ('held, not hidden. hover: the simulation does not sit in them') — K-FAFO should surface its own undecided features as ☐ seats rather than hiding them
- Accessibility baseline: aria-live logs, focus-visible 2px gold outline, prefers-reduced-motion

### K-FAFO constraints

- Stdlib-only Python is test-enforced for kode/kosmos; no Node/deno/bun on the system — K-FAFO's native side must be Python stdlib (or browser-hosted HTML), not an Electron/npm stack
- No live-update protocol exists: current data path is one-shot HTTP GET of record-byte-derived JSON on localhost:8014; websockets/SSE/polling are all un-built and arguably un-consented — any push channel is a new door
- No interactive I/O path to floors: stdout/stderr are DEVNULL, stdin PIPE unused — K-FAFO cannot render a floor's output through the supervisor today; only record files communicate
- net: doors 'do not exist yet' — a browser cannot be built until the net door grammar and its gate semantics are ratified
- Nothing runs or renders unconsented: no auto-fetch, no auto-refresh, no prefetch, no un-consented pixel; default-deny with the Korum starting EMPTY
- The shell never grants (korum invariant 30) — K-FAFO must route every new door to Konsent via live-ask; it may never self-authorize, and 'not now' must write no record
- Refusal must be a first-class rendered record (neutral ink), and a broken ledger chain must fail EVERYTHING closed 'until re-witnessed'
- Dark-native only for now: light theme is an open seat; palette gate UNGATED pending D-01 (GAP-D01-2)
- Ephemeral live OS: only the removable drive persists — all K-FAFO state (records dirs, ledger) must live on the drive
- PID1 is deferred metal-side (RB-09): K-FAFO targets the supervisor-semantics environment, not a real init

### Open questions (Konsent's)

- Which live-update mechanism may K-FAFO use for its panes (interval polling of /status.json, SSE, websocket, inotify on record files) — each is a new door shape needing Konsent's named yes
- Is K-FAFO a floor under Kosmos (listed in programs.json, own records_dir, crash-only reborn) or IS it the Konsole — the FG of K3, the single door? k3-live implies the latter; kosmos.py has no Konsole at all
- net: door grammar for a browser: what is the door unit (origin? host:port? URL shape with ** glob like the exec pattern in tests?) and what scope does one yes cover — per-crossing, standing per-origin, or standing per-shape?
- Should floors' stdout/stderr be surfaced in K-FAFO panes (requires changing the DEVNULL contract) or must all pane content stay record-bytes-only per pixel-trace honesty?
- Light theme derivation (explicit ☐ Konsent seat) — dark-only until then?
- 'Ken · Knut · Kall slates' (☐ seat): does the Kall schema {door, verb, fields, witness} bind K-FAFO's action format, and what are Ken and Knut?
- How does 'not now' behave in a persistent UI — no record is written, so what stops immediate re-asks? (k3-live: 'asking again is yours' — is re-ask always manual?)
- The k3-live WITNESSED pane cites 'kode at git 134eff9' on surface [A] while CLAUDE.md (same date) says still not a git repo — which surface is canonical for provenance, and should K-FAFO's witness view expect git hashes?
- G6 asking-semantics is Kyn's own seat — if K-FAFO speaks to Kyn (BG), the asking protocol is not Konsent's alone to define
- Default port 8014: reserved for the Kosmos HUD, or does K-FAFO get its own port/door?

### Verbatim

- “Honest scope: v0 is *supervisor semantics* as an ordinary process; true PID1/boot integration is metal-side, later (RB-09).” — `/home/user/KlaudeKode/kode/kosmos/kosmos.py:4-5`
- “EVERY spawn passes `korum.gate.ask()` first with an `exec:` door. An unlisted or ungranted program is REFUSED and the refusal is a ledger record ... The supervisor never grants — grants enter only through the sovereign door (korum invariant 30).” — `/home/user/KlaudeKode/kode/kosmos/kosmos.py:9-14`
- “`--serve` runs stdlib http.server on localhost: /status.json and /ledger.json are derived ONLY from record bytes, and /witness.json gives sha256 of the exact files those JSON values came from.” — `/home/user/KlaudeKode/kode/kosmos/kosmos.py:23-25`
- “# indicative states — never healthy/unhealthy, never a verdict on a being” — `/home/user/KlaudeKode/kode/kosmos/kosmos.py:54`
- “self.spawn(prog.name)  # every rebirth re-asks the gate” — `/home/user/KlaudeKode/kode/kosmos/kosmos.py:353`
- “theoretical HUD scaffold — pane content lands at P3; no un-consented pixel.” — `/home/user/KlaudeKode/kode/kosmos/hud/index.html:93`
- “A recorded no is a valid record, never an error state. Gold marks only the recorded yeses — consent moments.” — `/home/user/KlaudeKode/kode/kosmos/hud/index.html:104-105`
- “no programs — nothing runs unconsented” — `/home/user/KlaudeKode/kode/kosmos/hud/index.html:87 (empty-state text, also asserted verbatim by test_kosmos.py:317)`
- “K3 · Kosmos — the shell — BG: Kyn unseen · FG: the Konsole, the single door, where you sit” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:131-133`
- “you sit the Konsent seat. every crossing routes: ask → kode → gate → Korum → yes → Kage.” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:140`
- “three doors, never two — a binary handed to a mind is a Kommand wearing a question.” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:219`
- “type error — a Kommand at a mind: the grammar has no verb for that here. arity 2 is lawful only at the mindless (Kernel).” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:341`
- “day 6 — K3 · Kosmos opens on a question, not on activity” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:255`
- “the Konsole is yours, Konsent. the first thing K-Eden does is ask.” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:260`
- “grant-not-command · fail closed · the reborn is not amnesiac · "It's a choice."” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:205-206 (footer)`
- “a real run on surface [A], kode at git 134eff9, captured 2026-07-02. these bytes happened.” — `/home/user/KlaudeKode/kode/kosmos/k3-live.html:160`
- “No G1 seat is touched anywhere: this floor lets the whole supervisor be built and exercised without any model, any weights, any llama-server.” — `/home/user/KlaudeKode/kode/kosmos/floors/echo_floor.py:10-12`
- “sovereign_grant(self.store, "exec:%s %s **" % (PY, ECHO), "Konsent", witness="test fixture grant")” — `/home/user/KlaudeKode/tests/test_kosmos.py:89-90 (door patterns support a ** glob suffix)`

### Flags

- CONTRADICTION-SHAPED: k3-live.html:160 claims 'kode at git 134eff9' for the WITNESSED run, while CLAUDE.md (updated the same day, 2026-07-02) states 'Still not a git repo'. The run is attributed to 'surface [A]' — possibly a different machine/drive. Provenance for the witnessed bytes cannot be re-derived from this repo as-is.
- The 'live' in k3-live.html is theoretical simulation, not a live protocol: zero network calls, all ledger/Kage/Kyn behavior is in-page JS with a fakehash chain — except the one WITNESSED pane of hardcoded real captured data. Do not mine k3-live for wire-protocol facts; mine kosmos.py.
- Protocol answer for the synthesis: NOT websockets, NOT polling — one-shot HTTP GET of JSON derived from files (record bytes) served by stdlib ThreadingHTTPServer on 127.0.0.1:8014; files themselves are the ground truth and the only IPC.
- k3-live's GATE G1c uses browser confirm() — a two-option dialog — while its own doctrine text says 'three doors, never two' (the in-page live-ask modal does have three). Raw observation, no verdict.
- hud/index.html's admit-matching regex is broader than the store writes: /^(admit|grant|yes)$/i vs the store's literal 'yes'/'no' choices.
- hud pane 2 never consumes the served /ledger.json endpoint (file-drop only); the endpoint is exercised solely by tests.
- The Gate constructor accepts an escalate callable (kosmos.py:220 passes escalate=None) — an escalation-to-live-ask hook EXISTS in the interface but nothing in kosmos wires it; this is the natural attachment point for K-FAFO's live-ask modal.
- Two distinct record files per program is a load-bearing separation: program_record.json (supervisor's) vs floor_record.json (the floor's own, program-declared state). K-FAFO session persistence belongs in the floor record, not the supervisor's.
- Door patterns support glob: the test grant door is 'exec:<python> <echo_floor.py> **' — gate matching is not exact-string-only; relevant to how net: doors might be shaped.
- Vocabulary is explicitly 'indicative only' at every layer (kosmos.py:27, gate.py:16); the HUD's 'consented' check is /^(RUNNING|REBORN)$/ annotated 'a consented crossing, running now' — status words double as consent-state words.
- programs.json is minimal (one echo floor); the registry mechanism (name/argv/records_dir/floor + placeholder substitution) is the entire program model — 'floor' type field exists but nothing dispatches on it yet.


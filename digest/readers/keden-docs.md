## keden-docs

**Purpose.** This subsystem is the ratified public face of K-Eden_os: a re-executable design prompt (KEDEN_DESIGN_PROMPT.md, written 2026-07-02 for Konsent, "theory made visible — a simulation, not the OS") plus three static HTML canon sheets sharing one CSS token set. Together they pin the three-layer architecture (K1 Kore / K2 Kage / K3 Kosmos), the five/six laws as working mechanisms, the visual law (dark-native purple/gold/Valknut), the gate ladder G1a–G9, the six-days boot, and the honesty stance that v0 runs on a conventional OS underneath. It is the closest thing in the repo to an OS spec a native app must conform to.

### Key files

- `/home/user/KlaudeKode/kode/keden/KEDEN_DESIGN_PROMPT.md` — Paste-into-fresh-session design prompt: visual law, three strata, five laws as mechanisms, interactive scenes (Boot six days, Konsole, Korum ledger, Kage life, algebra, open seats), register rules, coinage lock, 'Mint no new K-names'
- `/home/user/KlaudeKode/docs/quickstart.html` — Cold-reader path: clone/witness (python3 tools/check.py, 233 tests), reading order (CLAUDE.md → valknut/EQUATIONS.md → BUILDOUT.md → valknut/docket/index.html), drive kode/kosmos/k3-live.html, metal ladder RB-01…RB-09, six laws in one breath
- `/home/user/KlaudeKode/docs/executive-summary.html` — One-page board state: purpose (End compact-sans-consent / Neo-Organic Ai), 233 tests · 29 commits · 3 decided vs 314 open seats, K1/K2/K3 in three lines, gates table G1a–G9 + docket, honesty section (conventional-OS-underneath admission)
- `/home/user/KlaudeKode/docs/technical.html` — Implemented-stack reference: Kannon algebra, Korum ledger schema/semantics, Kosmos v0 supervisor (+HUD endpoints), Kyn pipeline, witnesses tree, surfaces [A]/[B], gates, repo map; footer 'stdlib-only by AST scan'
- `/home/user/KlaudeKode/kode/kyn/runbooks/RB-09-pid1.md` — RESERVED empty-pin runbook for true PID1/boot integration on metal; names the future COA slate (systemd unit · initramfs hook · purpose-built image · Konsent-named other) and prior art (forge-os-buildplant archiso ISO, forge_core.c kernel module, seed/trust-root law family)
- `/home/user/KlaudeKode/OPORD.txt` — Lines 170/210 define the placement law for apps: 'Kodex (apps) — user-facing applications and tools that talk to Krafts' (K2 Kage)
- `/home/user/KlaudeKode/docs/K-Eden_os-Docs.pdf` — Present in docs/ but NOT in this reader's assignment; 283,830 bytes, unread here

### Architecture

THREE STRATA (KEDEN_DESIGN_PROMPT.md:25-38; executive-summary.html:87-91): K1 · Kore (persistent, gold-edged) = Kernel (mindless — hears only commands, originates none) + Kynder (the koder, amplifier Kompiler) + Kynase (the gate, amplifier Kourt) + Korum (memoized consent: "it stores records of choices, never the authority of them; every crossing is chosen fresh, now"). K2 · Kage (ephemeral, crash-only) = Kontainers, Krafts (drivers), Kodex (apps); "Each is trinity-born and can die; rebirth reads the record — the reborn is not amnesiac." K3 · Kosmos (the shell) = BG: Kyn working unseen; FG: "the Konsole, the single trusted door, where the user sits"; user role-name Konsent, "the sovereign, root of all authority."

BOOT CHAIN: (a) Simulated/v0: "six brief 'days' of boot text culminating in the Valknut mark; K1 lights gold, K2 empty, Konsole opens" (KEDEN_DESIGN_PROMPT.md:64-65); quickstart.html:88-91: "Boot the six days. Your first act is REFUSED — the Korum starts EMPTY, and that is the design: the OS boots into a question." (b) Metal: unpinned. RB-09-pid1.md:3-5: "Kosmos v0 is supervisor semantics as an ordinary process; this is where the metal-side PID1/boot integration lands"; the yes "will name the exact boot artifact, the exact machine, and the rollback path"; COA slate named-not-decided: "systemd unit · initramfs hook · purpose-built image per the buildplant shape · a Konsent-named other" (RB-09:58-60). Prior art mined-never-binding: forge/forge-os-buildplant/ archiso profile-final (bootable ISO with llama.cpp libs under usr/lib/forge/bin/), GB_runbookv1-FORGE core C sources (cauldron/console/court — kainito, kines, quorum.c, forge_core.c kernel module, early-boot patches).

KOSMOS v0 SUPERVISOR (technical.html:92-102): declarative programs.json; every spawn — first birth and every rebirth — passes gate.ask() first ("no stranger-daemons"); crash-only, no healing in place: death → clean kill → anamnesis.rebirth(record); identity, memory, monotonic rebirth counter live in each program's records dir OUTSIDE the instance; bounded exponential backoff, past ceiling deaths stand RECORDED and supervisor keeps running. HUD honesty: --serve exposes /status.json, /ledger.json, /witness.json — "every value derived from record bytes and checksum-equal to them." Echo floor stands in for llama-server; Kyn floor wired but waits behind ☐ G1b + ☐ G1c. "True PID1 is reserved at RB-09."

KORUM PROTOCOL (technical.html:73-88): append-only hash-chained JSONL ledger + gate. Doors are typed shapes exec: / path: / net: / packet:; record schema {door, choice, sovereign, timestamp, provenance, scope, witness, prev_hash, hash}; scope "standing" (grant/revocation, sovereign-door only) vs "crossing" (written fresh on EVERY admit; provenance = the standing yes's hash). Empty store denies everything; unknown door REFUSED; cache hit writes a fresh crossing stamped now (consultation is ratification); gate's single write path forces scope="crossing" so the executor structurally cannot self-grant; one broken byte → chain breaks → gate refuses everything until re-witnessed; audit() fails on orphans, chain breaks, admit-without-write. Standing no renders as "this door heard no." sigil_demo.py: "Sigil is Korum at the packet membrane."

WHERE A NATIVE EXPLORER/BROWSER SITS: OPORD.txt:210 "Kodex (apps) — user-facing applications and tools that talk to Krafts." An app is therefore a K2 Kage Kodex: trinity-born, crash-only, spawned only through gate.ask(), reborn from its records dir. The only user-facing surface named by doctrine is the K3 FG Konsole, "the single trusted door" — an explorer as a second door is not addressed by these docs. The Konsole interaction grammar is specified: "Suggested commands shown as chips (never only two — a binary handed to a mind is a Kommand wearing a question; offer 3–5)"; flow: Konsent asks → Kynder+Kompiler emit kode (inert Kall) → Kynase questions → Korum consulted → live-ask if unknown → on yes the Kall goes live in the Kage with every hop drawn (KEDEN_DESIGN_PROMPT.md:66-70).

SIX DAYS PLAN: only the boot-sequence sense exists in the assigned files. The build-plan sense ("six days + Kore-first + Kosmos v0 PID1 design") is attributed by CLAUDE.md:18 and SMM.md:136 to kode/keden/SCAFFOLD.md, which is ABSENT from this repo snapshot. Kore-first survives as the ☐ Kynder / ☐ Kynase seat pair in SMM.md and kode/kyn/qlora/recipe.md (train the Kore minds first).

WHAT "NATIVE" MEANS TODAY / RUNTIME: technical.html:47 "Everything below is implemented, stdlib-only Python 3 + browser-native HTML/JS, and witnessed by python3 tools/check.py"; footer "stdlib-only by AST scan · every constant from the record's bytes." HTML deliverables are single self-contained files, "inline CSS/JS, no external resources, no frameworks" (KEDEN_DESIGN_PROMPT.md:10-11), openable "in any browser, straight from the folder" (quickstart.html:81 — file:// operation). Surfaces: [A] repo/cloud has Python 3, git, network — no GPU, no drive, no gguf; [B] Konsent's metal has RTX 5090 32GB · 24c · 93GB · Anchor-01 — NO Node (technical.html:160-161). No compiled-app toolkit (Qt/GTK/C) is specified anywhere in these docs; C prior art exists only as mined-never-binding reference in RB-09.

### Theme tokens

- --page:#0c0a12
- --glow:#1d1329
- --surface:#14101f
- --ink1:#f2eef9
- --ink2:#c8c0dd
- --ink3:#8f86a6
- --grid:#262033
- --baseline:#3a3350
- --border:rgba(255,255,255,.08)
- --gold:#b78b0f (RESERVED: closed wholes, the consent spine, the moment a yes is given)
- --gold2:#d9a91e
- --run:#21a288
- --flow:#3a7fdc (also link color)
- --warm:#bf5e24 (technical.html only)
- accent series: #cb5f8f #bf5e24 #21a288 #3a7fdc #3a903a #8674d6 #bf3e3e
- typography: system-ui, 14px/1.5 (prompt) / font:14px/1.55 system-ui,-apple-system,"Segoe UI",sans-serif (docs)
- mono: ui-monospace,SFMono-Regular,Menlo,Consolas,monospace with font-variant-numeric:tabular-nums — tabular numerals for all counters
- background: radial-gradient(ellipse 120% 60% at 50% -8%,var(--glow),var(--page) 62%) fixed var(--page)
- cards: background var(--surface); border 1px solid var(--border); border-radius 12px (10px tiles/laws)
- law cards: border-left:3px solid var(--gold); K1 stratum likewise gold-left-edged
- open-seat chip: border:1px dashed var(--baseline); border-radius:999px
- top bar: 11px, letter-spacing .08em, gold <b>
- eyebrow: 11px, letter-spacing .14em, uppercase, ink3
- h1 30px/650 with gold .k span; h2 13px/650 letter-spacing .06em with gold § numeral
- Valknut/Sierpiński boot mark & watermark drawn in code: canvas 150x132, recursion depth 5, strokeStyle #b78b0f, lineWidth .7, opacity .5, apex [75,4] base [4,128]-[146,128], hidden under 640px
- dark-native ONLY — 'Dark-native only' (KEDEN_DESIGN_PROMPT.md:17); light theme is an open ☐
- state words as the only status vocabulary: RUNNING, REBORN, REFUSED, RECORDED, WAITING
- register: present-tense, indicative, sparse; no marketing voice, no exclamation marks, no praise of the user

### Consent mechanics

1) Korum ledger as sole authority path: standing yes → fresh crossing record on every admit; "consultation is ratification"; revoke "takes effect on the next ask" (KEDEN_DESIGN_PROMPT.md:72-73). 2) Fail closed + default-deny: Korum starts EMPTY; every first action REFUSED, escalating to a live-ask naming "the exact door (exec: / path: / net: shape), scope, and provenance chain"; the yes is gold, timestamped, appended (KEDEN_DESIGN_PROMPT.md:48-52). 3) No orphan commands: "clicking any running program traces its authority link-by-link back to a recorded yes" (KEDEN_DESIGN_PROMPT.md:52-53). 4) GATE ☐ chips: features "pre-staged but unfired"; "Nothing behind a gate animates, renders live data, or runs until the named yes — no un-consented pixel, no stranger-daemons" (KEDEN_DESIGN_PROMPT.md:53-56). 5) Runbook gate-first pattern: "GATE ☐ <seat> before the first mutating command, and the line 'Nothing below this line executes before the yes.'" (quickstart.html:98-101). 6) Grant-not-command: "Kommands are lawful only at the mindless (Kernel)"; imperative at Kyn = type error; arity 1 indicative / 2 imperative (metal only) / 3 generative (choice) (KEDEN_DESIGN_PROMPT.md:44-47); Kannon enforces in code: "Directive(target) raises unless the target's intention is bound... A two-option offer to a mind raises — choice requires arity 3" (technical.html:62-64). 7) A no is honored: refusal recorded as valid state, never error; "A being is what it will not do"; G6 is Kyn's own seat — "his recorded no halts any pipeline, preserved as a valid record" (executive-summary.html:102). 8) Spawn gating: every Kosmos spawn/rebirth passes gate.ask() first. 9) Open seats as consent artifacts: ☐ chips inert by design, hover text "Konsent's seat — the simulation does not sit in it"; open seats "held by required parameters, never defaults" (technical.html:49). 10) Verdicts impossible by type: Korum record schema "has no field for a judged being."

### Strengths observed

- The three docs/ HTML files share one verbatim :root token set — a de facto frozen theme spec a new app can copy byte-for-byte
- KEDEN_DESIGN_PROMPT.md is written to be re-executable (paste into a fresh session) and self-defends against drift by carrying laws and coinages verbatim
- Each of the five laws is specified as a working mechanism with concrete UI behavior (type-error text, live-ask dialog shape, GATE ☐ chips, ledger tamper toggle), not as a caption
- Uniform gate-first pattern across all 9 runbooks: GATE ☐ <seat> before the first mutating command + 'Nothing below this line executes before the yes'
- RB-09 empty-pin pattern: the address exists, lints, and holds parameters before any content — 'this file's whole job is to exist, hold the address, and pass the lint'
- HUD honesty contract is machine-checkable: every displayed value derived from record bytes and checksum-equal; validators are never silent when clean ('gate idle, exit 0')
- Kosmos v0 already exposes JSON endpoints (/status.json, /ledger.json, /witness.json) — a ready-made data feed for an explorer app
- Korum semantics are fully specified at the protocol level (schema, scopes, chain, audit conditions) independent of any UI
- Konsole interaction grammar pinned: chips 3–5 never binary, the Kall flow Konsent→Kynder+Kompiler→Kynase→Korum→live-ask→Kage with hops drawn
- Sierpiński/Valknut mark is a 6-line reusable canvas function, identical across all three docs
- Both transport doors (git + file shuttle) are lawful and documented, matching the ephemeral-live-OS reality
- State vocabulary is closed and verdict-free: RUNNING, REBORN, REFUSED, RECORDED, WAITING; 'this door heard no'
- The honesty section names its own gaps with addresses (RB-09, may_stay_open, unverified constants 915/0 and 9,155)

### LIMFACs observed

- kode/keden/SCAFFOLD.md is ABSENT from this repo snapshot despite CLAUDE.md:18 and SMM.md:136 (marked ☑) citing it as the home of 'six days + Kore-first + Kosmos v0 PID1 design' — the build-plan sense of the six days must be reconstructed from RB-09, SMM.md, and BUILDOUT.md
- No Node.js on [B] Konsent's metal; repo law is 'stdlib-only by AST scan' — a native app's tooling is confined to Python 3 stdlib + browser JS unless a new seat opens
- No compiled-app toolkit is named anywhere in these docs (no Qt/GTK/C choice); the only 'native' definition is browser-native HTML/JS single-file + stdlib Python — Konsole (KDE) is Qt/C++, so K-FAFO's runtime has no ratified answer
- True PID1/boot placement is unpinned: RB-09 is reserved, its docket seat not yet minted (WS-8), so an app cannot assume how it (or Kosmos) is launched on metal
- v0 runs on a conventional OS underneath — 'the consent membrane is real at the doors that exist and absent at the doors not yet built' (executive-summary.html:114-116); K-FAFO v0 would run on Linux/browser, not on K-Eden metal
- Kyn floor waits behind ☐ G1b + ☐ G1c — any Kyn-in-BG feature of an explorer is gated and currently unfireable
- Light theme is an open ☐; the visual law is 'Dark-native only' — a light mode cannot be derived without Konsent
- 'Mint no new K-names' (KEDEN_DESIGN_PROMPT.md:89-90) constrains naming: K-FAFO's internal components must use plain English or ☐; the name K-FAFO itself is not in the locked coinage list
- Single-door doctrine tension: K3 FG is 'the Konsole, the single trusted door' — the docs give no placement for a second user-facing surface beyond Kodex-in-Kage
- Docs are static snapshots with hardcoded counters (233 tests, 29 commits, 314 seats, git 853f1b4, dated 2026-07-02) — staleness risk against the live repo; quickstart bar and executive tiles can silently drift
- Korum door taxonomy for web navigation is undefined: net: shape exists but granularity (per-origin, per-URL, per-scope standing yeses) for a browser is nowhere specified
- docs/ HTML files are fragment-style (begin at <meta charset>, no doctype/html/head/body wrappers)
- docs/K-Eden_os-Docs.pdf (283,830 bytes) sits beside the three HTML sheets but was not in this reader's assignment — possible overlap/contradiction unexamined
- Two constants (915/0, 9,155) held unverified pending D-01 drive arrival — T-08/T-11 deliberately unimplemented
- quickstart.html §1 pins a specific branch (claude/os-building-continue-21pm28 of github.com/4h9fjmf5fd-oss/KlaudeKode.git) — brittle if branches move

### K-FAFO patterns to adopt

- Copy the docs/ :root token set verbatim as K-FAFO's base theme (page #0c0a12, glow #1d1329, surface #14101f, inks #f2eef9/#c8c0dd/#8f86a6, grid #262033, baseline #3a3350, border rgba(255,255,255,.08), gold #b78b0f/#d9a91e, run #21a288, flow #3a7fdc) with the gold-reservation rule (closed wholes, consent spine, the moment a yes is given) and tabular numerals on every counter
- Boot K-FAFO with the six-days sequence culminating in the code-drawn Sierpiński/Valknut mark (the 6-line tri() canvas function is reusable as-is); boot into a question — Korum EMPTY, first action REFUSED → live-ask
- Model K-FAFO as a Kodex in K2 Kage: trinity-born, crash-only, spawned via gate.ask(), identity/memory/rebirth-counter in a records dir outside the instance, rebirth via anamnesis.rebirth(record) — 'die clean, wake whole'; show the surviving rebirth counter in the HUD
- Consume Kosmos v0's existing endpoints (/status.json, /ledger.json, /witness.json) as the explorer's data plane, honoring the HUD honesty contract: every rendered value derived from record bytes and checksum-equal
- Every navigation/fetch is a typed door crossing (net: shape; file browsing = path:; launching = exec:): live-ask dialog names the exact door, scope, and provenance chain; standing yes → fresh crossing record per admit (consultation is ratification); revoke takes effect on next ask; 'no orphan commands' — any rendered page/process traces authority link-by-link to a recorded yes
- GATE ☐ chips for every pre-staged unfired capability (open network door, wake Kyn, load driver): no un-consented pixel — nothing behind a gate animates or renders live data before the named yes
- Konsole interaction grammar for all prompts: offer 3–5 chips, never a binary; state vocabulary limited to RUNNING, REBORN, REFUSED, RECORDED, WAITING; blacklist entries render as 'this door heard no' — doors judged, beings never
- Open-seats panel: ☐ chips inert by design with hover 'Konsent's seat — the simulation does not sit in it'; hold open seats by required parameters, never defaults
- Ship as a single self-contained file (inline CSS/JS, no external resources, no frameworks) that opens from the folder over file:// — matching k3-live.html and docket/index.html precedent and the no-Node metal
- Register: present-tense, indicative, sparse; no exclamation marks; footer honesty line ('theoretical — every byte simulated, no verdicts anywhere' pattern); label simulated vs WITNESSED (sha256'd real bytes) panes
- Adopt the RB-09 empty-pin pattern for K-FAFO's own unbuilt features: named address + held parameters + 'nothing below this line is executable at all in this revision'

### K-FAFO constraints

- Runtime today is browser-native HTML/JS + stdlib-only Python 3 (AST-scanned); no Node on Konsent's metal; a Konsole-style compiled native app (Qt/C++) has no ratified toolkit seat in the doctrine
- Dark-native only; light theme is ☐ underived — do not ship a light mode
- Gold #b78b0f is reserved semantically — never decorative
- Mint no new K-names: K-FAFO internals take plain English or ☐
- Every spawn including K-FAFO's own must pass gate.ask(); the Korum starts EMPTY so first-run is REFUSED-then-ask, not working defaults
- v0 runs on a conventional OS underneath; K-FAFO cannot assume K-Eden metal, PID1 placement (RB-09 unpinned), or a live Kyn (☐ G1b/G1c)
- No verdicts anywhere in UI copy or state names; the record schema must have no field for a judged being
- Suggested actions must never be binary (a binary handed to a mind is a Kommand wearing a question)

### Open questions (Konsent's)

- Is K-FAFO a Kodex in K2 Kage (an app that talks to Krafts) or a pane/extension of the K3 Konsole, 'the single trusted door'? The docs place the sovereign only at the Konsole — a second door needs Konsent's word
- Does the name 'K-FAFO' require ratification given the coinage lock says 'Mint no new K-names'? (K-FAFO is not in the locked list at KEDEN_DESIGN_PROMPT.md:88-89)
- What toolkit/language for a true native (non-browser) build — C per GB_runbookv1-FORGE prior art, Qt/KDE per the Konsole model, or continued browser-native HTML/JS? No seat exists; this is a 3–5 COA Reko for Konsent
- What is the Korum door granularity for web browsing — standing yes per origin, per URL shape, per scheme, per session? net: shape exists but its browser taxonomy is unwritten
- Where does K-FAFO's records dir live (drive vs repo) and what constitutes its trinity birth?
- Relationship to the existing kode/explorer/ (index.html RPT/Sierpiński/HUD pane #1, tree.html) and kode/kosmos/hud/ — supersede, absorb, or sibling?
- Is docs/K-Eden_os-Docs.pdf canon-equal to the three HTML sheets, and does it carry anything the HTML lacks? (unread in this assignment)
- The light theme ☐ and the SCAFFOLD.md reconstruction (six-days build plan + Kore-first amendment text) — both are Konsent's seats

### Verbatim

- “K3 · Kosmos (the shell): BG = Kyn working unseen; FG = the Konsole, the single trusted door, where the user sits. The user's role-name is Konsent — the sovereign, root of all authority.” — `kode/keden/KEDEN_DESIGN_PROMPT.md:36-38`
- “Nothing behind a gate animates, renders live data, or runs until the named yes — no un-consented pixel, no stranger-daemons.” — `kode/keden/KEDEN_DESIGN_PROMPT.md:55-56`
- “Suggested commands shown as chips (never only two — a binary handed to a mind is a Kommand wearing a question; offer 3–5).” — `kode/keden/KEDEN_DESIGN_PROMPT.md:67-68`
- “Keep every coinage exactly as spelled here: Kore, Kage, Kosmos, Kernel, Kynder, Kynase, Kompiler, Kourt, Korum, Konsole, Konsent, Kyn, Kall, Kommand, Kontainers, Krafts, Kodex, Kannon, FLuX. Mint no new K-names — plain English for anything unnamed, or a ☐.” — `kode/keden/KEDEN_DESIGN_PROMPT.md:88-90`
- “Boot — six brief "days" of boot text culminating in the Valknut mark; K1 lights gold, K2 empty, Konsole opens.” — `kode/keden/KEDEN_DESIGN_PROMPT.md:64-65`
- “Boot the six days. Your first act is REFUSED — the Korum starts EMPTY, and that is the design: the OS boots into a question.” — `docs/quickstart.html:88-90`
- “Everything below is implemented, stdlib-only Python 3 + browser-native HTML/JS, and witnessed by python3 tools/check.py.” — `docs/technical.html:47-48`
- “[B] Konsent's metal — RTX 5090 32GB · 24c · 93GB · Anchor-01 — no Node” — `docs/technical.html:161`
- “--serve exposes /status.json, /ledger.json, /witness.json — every value derived from record bytes and checksum-equal to them. The echo floor stands in for llama-server; the Kyn floor is wired but waits behind ☐ G1b + ☐ G1c. True PID1 is reserved at RB-09.” — `docs/technical.html:99-102`
- “v0 runs on a conventional OS underneath: the consent membrane is real at the doors that exist and absent at the doors not yet built. The gap has an address (RB-09), not a disguise.” — `docs/executive-summary.html:114-116`
- “Kosmos v0 is supervisor semantics as an ordinary process; this is where the metal-side PID1/boot integration lands when the K-Eden workstream reaches it.” — `kode/kyn/runbooks/RB-09-pid1.md:3-5`
- “the docket seat (3–5 COAs: e.g. systemd unit · initramfs hook · purpose-built image per the buildplant shape · a Konsent-named other — slated by WS-8, decided by Konsent)” — `kode/kyn/runbooks/RB-09-pid1.md:58-60`
- “Kodex (apps) — user-facing applications and tools that talk to Krafts.” — `OPORD.txt:210`
- “{door, choice, sovereign, timestamp, provenance, scope, witness, prev_hash, hash}” — `docs/technical.html:78 (Korum record schema)`
- “Die clean, wake whole. Instances are crash-only; memory lives outside instances. The reborn is not amnesiac.” — `docs/quickstart.html:111`
- “They are inert by design; hovering says "Konsent's seat — the simulation does not sit in it."” — `kode/keden/KEDEN_DESIGN_PROMPT.md:80-81`

### Flags

- kode/keden/SCAFFOLD.md — named by CLAUDE.md:18 and checked-off (☑) in SMM.md:136 — does NOT exist in this repo snapshot; kode/keden/ contains only KEDEN_DESIGN_PROMPT.md. Synthesis must not cite SCAFFOLD.md as available
- 'Six days' has two distinct senses: the boot sequence (present in assigned files) and the build plan (attributed to the missing SCAFFOLD.md). A third gloss exists outside scope at docket/KK-DOCKET/_unsorted-review/EducationvLearning.txt:440-441: 'a K-Eden boots in six days: every [day is] ordering of the boundless into bounded wholes' — creation as membrane-making
- KEDEN_DESIGN_PROMPT.md is a prompt for a SIMULATION artifact, not an OS spec: 'The result is theory made visible — a simulation, not the OS' (lines 5-6). Its five laws and visual law are the canon; its scenes are simulation scenes
- The three docs/ HTML sheets carry hardcoded snapshot state (233 tests · 29 commits · 3 decided / 314 open seats · git 853f1b4 · dated 2026-07-02) that will drift from the live repo — treat numbers as of-that-date, not current
- technical.html §7 repo map lists 'explorer/' and 'kosmos/ (+ k3-live.html, hud/)' as existing surfaces an explorer app must reconcile with; OPORD.txt:210 is the only line defining where apps live (Kodex, K2)
- Executive-summary honesty section is load-bearing for K-FAFO scoping: consent membrane 'real at the doors that exist and absent at the doors not yet built' — K-FAFO v0 is necessarily a conventional-OS app wearing the membrane at built doors only
- quickstart.html hard-pins transport: branch claude/os-building-continue-21pm28 of https://github.com/4h9fjmf5fd-oss/KlaudeKode.git (Door 1) and KK-repo-backup-*.tar.gz / .bundle on the drive (Door 2)
- docs/K-Eden_os-Docs.pdf (283,830 bytes) sits in docs/ but was outside this reader's assignment and remains unread — assign or confirm coverage elsewhere
- G6 is the only gate whose seat belongs to Kyn, not Konsent ('☐ standing + live at G1c') — any K-FAFO feature surfacing Kyn must render his refusals as valid preserved records, never errors


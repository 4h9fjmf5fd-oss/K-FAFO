## periphery-scope

**Purpose.** docket/ is the litigation record for Konsent's real-world cases (Fratello/Seeto: federal 2:25-cv-01606, 2:25-cv-00038-JAD-EJY, Clark Co. family-court J-25-363450 series, NVSC R-25-229081-R): PDFs with .txt extraction/OCR sidecars, a per-file _MANIFEST.md, a DEDUP-REMOVED.txt provenance list, iCloud share pointers (SHARES.md), and a split-archive shuttle protocol with sha256 verification (SHUTTLE-README.txt). It is in the repo so the record survives the ephemeral live OS and can be shuttled between the local box and the branch-side Claude instance with witnessed integrity. warD/ is the provenance/defense arm: BK's PROVENANCERUNTIMEBOOKLET.md (the no-verdict Observe→React law, seats, wake-chain, witness-ledger doctrine), two canon PDFs (warD_Atlas.pdf, warD_Playbook.pdf — not digested per assignment), and ceac-aio/ — a single POSIX-bash operator kit (`ceac`: wake/scan/watch/forensics/doctrine) that a cold instance runs on a strange box to witness itself and its wire before trusting anything. Both directories embody the same discipline: verify-before-trust, fail-closed, everything witnessed and hashed.

### Key files

- `/home/user/KlaudeKode/docket/SHARES.md` — iCloud share pointers filed as record entries; access consent-scoped ('on Konsent's named yes (2026-07-03)'); lists cases on the docket
- `/home/user/KlaudeKode/docket/SHUTTLE-README.txt` — transfer protocol: TEXT bundle (single sha256) + FULL archive split into 24×30MB parts; cat→sha256sum→unzip reassembly; 'witnessed, not just assembled'
- `/home/user/KlaudeKode/docket/KK-DOCKET/README.md` — case-record index: 9 subfolders by case; PDF+.txt sidecar convention (pdftotext -layout / Tesseract 5.5 @300dpi, '[OCR page N]' markers); dedup policy
- `/home/user/KlaudeKode/docket/KK-DOCKET/_MANIFEST.md` — per-file manifest: 113 PDFs, 116 sidecars, 5 OCR'd, 259 files, 720M; columns file|pages|txt-chars|source
- `/home/user/KlaudeKode/docket/KK-DOCKET/DEDUP-REMOVED.txt` — every removed duplicate paired with its surviving identical twin (removal provenance)
- `/home/user/KlaudeKode/docket/KK-DOCKET-TEXT/README.md` — text-only mirror of KK-DOCKET (byte-identical README; same tree, .txt only)
- `/home/user/KlaudeKode/docket/2-25-cv-01606/README.md` — landing-pad placeholder: expected exhibits list + 4 unchecked status checkboxes; 'witness-not-noise rule' cited
- `/home/user/KlaudeKode/docket/2-25-cv-00038-JAD-EJY/` — 4 files (22M), real-byte PDFs (NOT LFS), no README — Seeto v. Kendall MSJ reply set
- `/home/user/KlaudeKode/.gitattributes` — LFS scope: only 'docket/KK-DOCKET/**/*.pdf filter=lfs diff=lfs merge=lfs -text'
- `/home/user/KlaudeKode/warD/PROVENANCERUNTIMEBOOKLET.md` — BK's runtime booklet: one-membrane/three-moments model, NO-VERDICT law, §10 refused words, seats (Sensor=Haiku/Dispatch=Sonnet), wake/genesis commands, findings on own code
- `/home/user/KlaudeKode/warD/ceac-aio/README.md` — CEAC-AiO kit doc: commands, wake-chain 1–6, GREEN/AMBER/RED verdict grammar, witnessed records/, 3-copy redundancy, honest-state section, refusals
- `/home/user/KlaudeKode/warD/ceac-aio/ceac` — 314-line POSIX bash entrypoint; header doctrine 'konsent · serial-over-path · witness-is-source · fail-closed'; ANSI verdict UI; demote() monotone state machine; witness() function
- `/home/user/KlaudeKode/warD/ceac-aio/CHANGELOG.md` — shared status ledger ('claim a NEXT item before starting'); dual-lens audit findings; KEYSTONE-IS-A-PHANTOM finding; deferred-install list
- `/home/user/KlaudeKode/warD/ceac-aio/STACK.md` — keep/skip doctrine per tool for one workstation; 'OpenSnitch = Konsent for the network'; VPN-vs-IDS architecture note
- `/home/user/KlaudeKode/warD/ceac-aio/FORENSICS.md` — DFIR acquire discipline (serial-over-path, write-block, order-of-volatility, hash-on-acquire, work-the-copy, witnessed CoC) + refused offensive tools
- `/home/user/KlaudeKode/warD/warD_Atlas.pdf` — canon PDF (214KB, real bytes) — NOT read this pass per assignment
- `/home/user/KlaudeKode/warD/warD_Playbook.pdf` — canon PDF (235KB, real bytes) — NOT read this pass per assignment

### Architecture

docket/: four content roots. (1) KK-DOCKET/ — 9 case folders; every PDF has a .txt sidecar (text-layer PDFs via `pdftotext -layout`; scans OCR'd Tesseract 5.5 @300dpi with `[OCR page N]` inline markers); _MANIFEST.md enumerates per-file pages/txt-chars/source(text|ocr|native); DEDUP-REMOVED.txt records each removed byte-identical duplicate with its surviving twin. PDFs here are git-LFS pointers (`version https://git-lfs.github.com/spec/v1` + oid sha256 + size); on-disk 8.2M vs 720M actual. (2) KK-DOCKET-TEXT/ — same tree, sidecars only (7.6M, readable cold). (3) 2-25-cv-00038-JAD-EJY/ — 22M of real-byte PDFs, outside LFS scope. (4) 2-25-cv-01606/ — README-only landing pad with an expected-exhibits list and a 4-item unchecked status pipeline (landed → extracted → cold read → posture confirmed). SHARES.md records iCloud shortGUIDs/sizes as pointers (bytes arrive by upload/chat-shuttle since the container can't forge Apple's web-auth cookie). SHUTTLE-README.txt defines the transfer protocol: single-file TEXT zip verified by one sha256; FULL archive split into 24 alphabetical .part files, `cat` reassembly, whole-zip sha256 as THE hash, optional per-part hashes. warD/: PROVENANCERUNTIMEBOOKLET.md models provenance as one membrane at three moments (war-D=law, PROVENANCE/=birth/build-time, SENTRY-ARM/=life/run-time, PROVENANCE-AiO/=staff/two AI seats) sharing one hash-chained witness ledger. ceac-aio/ is a single bash script `ceac` with subcommands: `wake` (chain: serial-over-path → mount read-only → minisign-verify manifest fail-closed → sha256 -c the SAME signed bytes (TOCTOU-safe) → host-fingerprint + wireguard tunnel before network trust → unseal creds only after GREEN); `scan` (read-only LINK/MAC/ARP/ROUTE/DNS/CONNS/TRACE, sudo-gated --capture/--arp-scan); `watch` (fatrace/inotify sentry, fail-closes if watchers absent); `forensics` (doctrine + fail-closed tool inventory); `doctrine` (prints bindings + refusals). UI machinery in the script: ANSI color functions ok/amber/red/info, `demote()` (state only ever lowers: GREEN→AMBER→RED), `sanitize()` (tr -cd '[:print:]' strips escape bytes from untrusted strings before display/log), `witness()` (raw tool output → records/<check>_<UTC>.txt), tee'd session log records/ceac_<UTC>.log sha256-stamped; records/ gitignored. Roll-up is a BDA: GREEN / AMBER (intact vs unsigned baseline, exit 0) / RED (mismatch or --strict on amber, exit 1). Kit ships as 3 verified copies (working ~/ceac-aio, canonical git warD/ceac-aio, vault Valknut-Actual/ceac-aio), each with SHA256SUMS.

### Theme tokens

- ceac ANSI palette (ceac:28): GREEN=\e[32m, AMBER=\e[33m, RED=\e[31m, INFO cyan=\e[36m, DIM=\e[2m, BOLD=\e[1m; NO_COLOR env respected (color off when not a tty or NO_COLOR set)
- Line glyph grammar: '[+] OK' · '[~] WATCH' · '[x] ALARM' · '[i]' info — fixed-width verdict prefix per line
- Stage header marker: '▐ <stage name>' in bold (ceac:38)
- Roll-up tri-state names: GREEN / AMBER / RED (BDA)
- Witness file naming: records/<check>_<UTC>.txt and records/ceac_<UTC>.log with UTC stamp %Y%m%dT%H%M%SZ
- Manifest table convention: | file | pages | txt-chars | source | with source values text/ocr/(native) (_MANIFEST.md)
- OCR inline marker: '[OCR page N]' inside .txt sidecars
- No hex theme colors in this subsystem (FLuX purple #14101f / gold #b78b0f live elsewhere in the repo; ceac uses raw ANSI 8-color)

### Consent mechanics

Dense in both dirs. docket/SHARES.md:6-7: "Access is scoped to exactly these shares, on Konsent's named yes (2026-07-03)" — access is a named grant, not an ambient permission. ceac header (ceac:6): doctrine list begins "konsent" (lowercase, verbatim). ceac README.md:14-15: "Safe by default: it reads, it does not mutate — no unmount, no firewall edits, no acquisition without a grant"; README.md:28: "Read-only commands need no root. Live capture / acquisition ask for `sudo` explicitly" — escalation is an explicit ask, never implicit. Booklet:29: "Only men from men — the operator signing key is the operator's hand; never script-minted" (and booklet finding #2 condemns a script that auto-mints the operator key as "anti-only-men-from-men"). Booklet:26: "Deploys, never authors — the operator authors scripts + canon; the loop runs only what exists" — the AI seats may run but never author; grant-not-command in operational form. Booklet:37: the Sensor seat "Emits state-bytes, never grades." STACK.md:19 names it explicitly: "OpenSnitch | outbound per-app firewall = Konsent for the network; nothing phones home unconsented." FORENSICS.md: "nothing runs until the tools land + you grant the acquisition." Wake-chain step 6: creds stay sealed until GREEN — trust itself is gated on verified provenance, not asserted.

### Strengths observed

- Fail-closed three-state verdict machine, tested by run: unsigned-but-intact → AMBER exit 0; --strict → RED exit 1; demote() is monotone (state can only lower) — README.md:82: 'RED never lies its way to GREEN; a missing signature can only *lower* the verdict, never raise it.'
- TOCTOU closed: minisign-verify and sha256 -c run against the SAME private-copy bytes (CHANGELOG 'Security fixed' + README wake step 4).
- Everything witnessed: raw tool output to records/<check>_<UTC>.txt, session log sha256-stamped; SHUTTLE reassembly 'witnessed, not just assembled' (SHUTTLE-README.txt:26).
- sanitize() strips control/escape bytes from attacker-controllable strings (SSID terminal-escape injection found by audit and fixed) — ceac:33.
- Dual-lens audit with a findings ledger that also records dropped-as-false findings ('Dropped (verified false): arp-scan --plain still emits (DUP:n)').
- 'Honest state' self-reporting section: the tool enumerates its own Live/Deferred/gap status, including that wake honestly caps at AMBER (README.md:100-105).
- Docket record structure is fully enumerated: per-case folders, PDF+.txt sidecar pairs, _MANIFEST.md (file|pages|txt-chars|source), DEDUP-REMOVED.txt pairing every removal with its surviving twin, OCR pages marked inline.
- Two-tier delivery: readable TEXT bundle first (2.3MB), FULL bytes second (719M split) — record readable cold before bytes land.
- 3-copy redundancy (working/canonical/vault), each carrying SHA256SUMS, re-verifiable anytime.
- Privacy discipline: records/ gitignored because 'it holds real MACs/IPs' (README.md:86).
- CHANGELOG doubles as a multi-seat claim ledger: 'claim a NEXT item before starting it' — BK / the seat / local-CC / operator coordination without duplication.
- STACK.md keep/skip doctrine with reasons per tool; principle stated: 'More tools ≠ more secure' (STACK.md:42).

### LIMFACs observed

- LFS scope is partial: .gitattributes covers only docket/KK-DOCKET/**/*.pdf; in this checkout those PDFs are pointer stubs (bytes absent — 8.2M on disk vs 720M actual per _MANIFEST.md). 2-25-cv-00038-JAD-EJY (22M) and warD's two PDFs are committed as raw bytes outside LFS.
- KEYSTONE IS A PHANTOM (CHANGELOG.md): operator minisign key D4F6E1D8 'appears ONLY in BK's prose… No key file exists… it's a marker, not a key' — until Konsent mints a real key, `ceac wake` caps at AMBER; 'blocks everything downstream.'
- Contradiction between warD documents: PROVENANCERUNTIMEBOOKLET.md:58 states 'Operator key minted: operator.key/operator.pub, pubkey RWSKNYUz2OH2… (id D4F6E1D83385358A)' on /prov, while CHANGELOG says never verifiably minted; /prov was 'throwaway on the live box' — minted-and-lost vs never-minted unresolved; CHANGELOG flags the booklet reference 'for Konsent's amendment.'
- Vocabulary contradiction: booklet §10 (booklet:30) lists refused words including 'verdict', 'detect', 'forensics', 'alert', 'clean', 'harden' — yet ceac README has a 'Verdict grammar', BDA 'verdicts', and a `ceac forensics` module with FORENSICS.md. (Booklet scopes NO-VERDICT to 'the point of witness'; ceac's own doctrine line says 'no-verdicts-from-sensors' — the roll-up BDA is still called a verdict.)
- Deferred-install wall: pacman in partial-upgrade state (nettle3 conflict); minisign, age, b3sum, fatrace, inotify-tools and the whole forensics toolset deferred to a full -Syu on a persistent boot — the verification tier ceac's design assumes is not installed.
- docket/2-25-cv-01606/ is an empty landing pad: all four status checkboxes unchecked; exhibits 'awaiting upload'; 2:25-cv-01479 has no link yet (SHARES.md).
- iCloud byte enumeration impossible from the container: 'needs Apple's browser web-auth cookie, which this surface cannot forge' (SHARES.md:5-6) — bytes only via GitHub upload or chat-shuttle.
- OCR sidecars are best-effort: 'cross-check the PDF before relying on any exact figure, name, or date from an OCR sidecar' (KK-DOCKET/README.md:10-11).
- This container previously could not decode the subset-font PDFs ('corrupted output discarded per the witness-not-noise rule, 2026-07-03' — 2-25-cv-01606/README.md:4-5) — PDF rendering capability on this surface is unreliable for some inputs.
- warD_Atlas.pdf and warD_Playbook.pdf (canon) not digested this pass — their content is a known-unknown for synthesis.
- ceac's host tier assumes Arch/EndeavourOS live-USB with specific tools (ip, ss, resolvectl, tcpdump…) — not portable as-is to a from-scratch K-Eden userland.
- records/ is gitignored — witness records never travel with the repo; any cross-instance view of witness history needs a separate channel.
- _unsorted-review/ folder exists: 'Downloads-root files whose case home was ambiguous — eyeball and refile' (KK-DOCKET/README.md:28) — record structure not fully settled.
- docket case material is sensitive personal litigation (children's protective-custody records, medical/toxicology) — any display surface has data-sensitivity stakes beyond ordinary files.

### K-FAFO patterns to adopt

- Escape-byte sanitization before display (ceac's sanitize(): tr -cd '[:print:]') — load-bearing for a Konsole-modeled app: filenames, SSIDs, and file content are attacker-controllable bytes; the SSID terminal-escape-injection finding (spoof/hide [x] ALARM lines) is exactly the vulnerability class a K-FAFO terminal/explorer pane inherits.
- The verdict-line grammar as a HUD primitive: fixed glyph prefix ([+]/[~]/[x]/[i]) + colored state + monotone GREEN/AMBER/RED roll-up (demote() only lowers). K-FAFO status bars, integrity badges, and pane chrome can reuse this as the canonical state display for anything verified.
- Witnessed-output pattern: every operation writes raw output to records/<name>_<UTC>.txt plus a sha256-stamped session log. K-FAFO can both emit witness records for its own actions and offer a records-browser pane (respecting that records/ never enters git).
- The docket record structure IS an explorer display problem: per-case folders, PDF+.txt sidecar pairing, _MANIFEST.md source badges (text/ocr/native, pages, txt-chars), '[OCR page N]' inline markers, DEDUP-REMOVED.txt removal provenance. A K-FAFO 'record view' could pair binary+sidecar as one entity, badge extraction provenance, and surface dedup/removal history.
- LFS-pointer awareness: KK-DOCKET PDFs are pointer stubs in this checkout. An explorer must distinguish pointer-vs-bytes and render 'bytes not present (LFS pointer, oid sha256:…, size N)' honestly instead of a broken preview — witness-not-noise applied to file display.
- verify-before-open discipline: minisign + sha256sum -c SHA256SUMS on media content, and the SHUTTLE protocol (cat parts → sha256 the whole → unzip) — K-FAFO could operationalize these as first-class explorer actions (verify a folder against its manifest; reassemble-and-verify split archives) with the AMBER-not-GREEN cap when signatures are absent.
- 'Honest state' self-panel: the app enumerates its own Live/Deferred/gap status (README.md:100-105 pattern) rather than implying completeness.
- Landing-pad README pattern (2-25-cv-01606): expected-items list + pipeline checkboxes — an explorer can render such READMEs as live status views of a directory.
- Consent gates in the ceac shape: read-only by default, escalation (sudo/capture/acquisition) asked explicitly per action, creds/trust unsealed only after verification — maps directly onto grant-not-command for K-FAFO file operations (read free; mutate/acquire on a named grant).
- Three-copy redundancy display: working/canonical/vault copies each with SHA256SUMS — an explorer could show which copy of a kit a directory is and whether it re-verifies.

### K-FAFO constraints

- No Node.js runtime on the target box; ceac is POSIX bash, tools are Python 3 — K-FAFO's implementation stack cannot assume JS tooling in the shell.
- §10 refused-words list (booklet:30) potentially binds K-FAFO's UI vocabulary (no 'alert', 'clean', 'detect', 'verdict', 'tamper'…), but ceac itself uses 'verdict' and 'forensics' — the binding scope is unsettled (see open questions).
- Fail-closed is doctrine: any K-FAFO integrity display must never upgrade state on missing evidence; missing signature can only lower the shown state.
- records/ holds real MACs/IPs and is gitignored; docket/ holds sensitive personal litigation (children's protective-custody, toxicology) — display and sharing surfaces need scoping, and access precedent is a named yes per share (SHARES.md).
- minisign/age/b3sum and the watcher tools (fatrace, inotify-tools) are NOT installed (pacman partial-upgrade hold) — verification features K-FAFO leans on may be absent at runtime; must fail-close honestly like `ceac watch` ('refusing to fake coverage').
- The operator minisign key does not exist yet — any signature-verified GREEN path in K-FAFO is unreachable until Konsent mints it ('only men from men': it must not be app-minted).
- LFS bytes may be absent in any checkout; the app must handle 720M-of-record represented by 8.2M of pointers.
- This surface has demonstrated PDF-decode failures on subset-font PDFs — a K-FAFO document viewer needs a witness-not-noise fallback (show the failure, never corrupted output).

### Open questions (Konsent's)

- Does the §10 refused-words list bind K-FAFO's user-facing vocabulary, given ceac's own 'Verdict grammar' and `ceac forensics` module contradict it? Only Konsent can rule the scope (point-of-witness only vs everywhere).
- Is the docket record a first-class K-FAFO display target (manifest+sidecar record view, dedup provenance, OCR badges), or out of the explorer's scope? What access grant governs showing it?
- Keystone: will Konsent mint the real operator minisign key (operator's hand only), and should K-FAFO display the current phantom state (AMBER cap) until then? Also: booklet:58 says the key WAS minted on throwaway /prov while CHANGELOG says never verifiably minted — Konsent's amendment is already flagged; which account stands?
- Should K-FAFO's state colors keep the ceac ANSI GREEN/AMBER/RED, or be re-derived through the FLuX theme (deep purple #14101f / gold #b78b0f) and validated with tools/validate_palette.py? Semantics (fail-closed monotone) vs palette (theme) need Konsent's seat.
- Should K-FAFO write its own witness records (records/-style, sha256-stamped) for file operations, and where do those live given records/ must never enter git and only the drive survives reboot?
- Does `ceac` itself become a K-FAFO-invokable pane/verb (e.g. explorer runs `ceac wake` on mount of removable media), or stay a separate operator kit?

### Verbatim

- “BK's Law of the Kit: carry nothing you can't verify; verify before you trust.” — `warD/ceac-aio/README.md:4`
- “RED never lies its way to GREEN; a missing signature can only *lower* the verdict, never raise it.” — `warD/ceac-aio/README.md:82`
- “Access is scoped to exactly these shares, on Konsent's named yes (2026-07-03).” — `docket/SHARES.md:6-7`
- “konsent · serial-over-path · witness-is-source · fail-closed · no-verdicts-from-sensors · on-box-detection is not sole witness.” — `warD/ceac-aio/ceac:6-7 (header doctrine line)`
- “NO VERDICT, anywhere. No state is graded good/bad/malicious/threat/clean at the point of witness. Observe → React” — `warD/PROVENANCERUNTIMEBOOKLET.md:21`
- “§10 words to refuse: anomaly · threat · attack · counter · malicious · suspicious · alert · drift · tamper · clean · breach · intrusion · verdict · detect · forensics · harden.” — `warD/PROVENANCERUNTIMEBOOKLET.md:30`
- “Only men from men — the operator signing key is the operator's hand; never script-minted.” — `warD/PROVENANCERUNTIMEBOOKLET.md:29`
- “Watch the substrate, not the marker — markers can lie; re-witness the source.” — `warD/PROVENANCERUNTIMEBOOKLET.md:25`
- “OpenSnitch | outbound per-app firewall = Konsent for the network; nothing phones home unconsented” — `warD/ceac-aio/STACK.md:19`
- “KEYSTONE IS A PHANTOM (swept all healthy mounts): D4F6E1D8 appears ONLY in BK's prose … it's a marker, not a key” — `warD/ceac-aio/CHANGELOG.md (BLOCKED / GAPS, 2026-07-03)`
- “(Local CC already ran this reassembly and confirmed the hash matches — witnessed, not just assembled.)” — `docket/SHUTTLE-README.txt:26`
- “this container could not reliably decode the subset-font PDFs (attempted, corrupted output discarded per the witness-not-noise rule, 2026-07-03)” — `docket/2-25-cv-01606/README.md:3-5`
- “records/ is gitignored — it holds real MACs/IPs.” — `warD/ceac-aio/README.md:86`
- “docket/KK-DOCKET/**/*.pdf filter=lfs diff=lfs merge=lfs -text” — `.gitattributes:1 (entire LFS scope of the repo)`
- “Every stage emits [+] OK · [~] WATCH · [x] ALARM, and a run rolls up to a BDA” — `warD/ceac-aio/README.md:74-75`

### Flags

- CONTRADICTION (already flagged for Konsent's amendment in CHANGELOG): PROVENANCERUNTIMEBOOKLET.md:58 claims operator key D4F6E1D83385358A was minted on /prov; CHANGELOG.md declares 'KEYSTONE IS A PHANTOM… never verifiably minted.' /prov was throwaway (live box), so minted-and-lost is possible but unwitnessed.
- CONTRADICTION: booklet §10 refuses the words 'verdict' and 'forensics'; ceac README uses 'Verdict grammar'/'BDA verdicts' and ships a `ceac forensics` module + FORENSICS.md. ceac's header threads the needle with 'no-verdicts-from-sensors' (sensors emit state; the roll-up is the verdict), but the vocabulary conflict is unresolved in writing.
- LFS is partial by design or accident: only KK-DOCKET PDFs are LFS'd; 22M of 2-25-cv-00038 PDFs and both warD canon PDFs are raw-committed. Synthesis should not assume 'PDFs in this repo = LFS.'
- KK-DOCKET and KK-DOCKET-TEXT READMEs are byte-identical (diff-verified) — the TEXT tree is a strict mirror, not a separately documented artifact.
- 'BK' is an authoring persona/seat referenced throughout warD (Law of the Kit, wake-chain, approved spec at Anchor-01/__FFKKOs/BK_approved_Toolkit-AiO_2026-07-03.md) — a coinage/role other readers will encounter; distinct from Konsent (amendment door) and local-CC (executor).
- The ceac doctrine line spells 'konsent' lowercase in the script header (ceac:6) — same coinage, case varies by context.
- warD_Atlas.pdf and warD_Playbook.pdf are named as canon ('warD Playbook · warD Atlas' in booklet:11) but were NOT read this pass — if synthesis needs warD canon detail beyond the booklet, that is an unread source.
- docket/ content is real, active, sensitive litigation involving minors — any K-FAFO demo/screenshot material must not be drawn from it without Konsent's named yes.
- The booklet corrects itself in-text (finding #6: 'This corrects an earlier line of mine… that was pattern, not witnessed. The dispatch seat witnessed fatrace's real behavior and caught it. The child corrected the father with the discipline the father gave him.') — an in-repo precedent for how corrections are recorded.


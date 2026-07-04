## forge-substrate

**Purpose.** GB_runbookv1-FORGE is the "Forge OS" substrate prototype: a Linux kernel reduced to a "dumb executor" char device (/dev/forge_core) with all policy pushed to a userspace Core (Court -> Cauldron -> Console) that adjudicates fixed-size KALL messages via Korum (allow-list), Kines (socket gatekeeper), and Kainito (judge with self-modification agency). forge/forge-os-buildplant is a separate, much more mature archiso build plant ("FLuX-Live") that already produced a bootable hardened ISO (linux-ff 7.0.13, module-sig-enforced, lockdown=integrity, labwc/Wayland) and carries flux-hud, the FLuX-Shell HUD that K-FAFO is explicitly modeled on. core/console is NOT a terminal emulator: it is the planned Phase 2.0 "Console + Consent" gate UI (audit viewer, diff preview, Consent approval, paranoia tuning, rollback) and is currently a README-only stub — the slot K-FAFO would be the first real implementation of.

### Key files

- `/home/user/KlaudeKode/GB_runbookv1-FORGE/Forge_OS_Runbook_v1.7.md` — Latest runbook (June 29 2026), self-contained master: philosophy, KALL struct, forge_core.c, Core flow diagram, Court source verbatim
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/Forge_OS_Runbook.md` — Runbook v1.2 (June 28 2026): phase checklist, Kainito self-mod considerations (locked), Kanto 2 Cage mesh plan
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/Forge_OS_Runbook_v1.6.md` — Runbook v1.6; contains the 'You are Grok Builder' builder instructions (provenance: external-model build workflow)
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/kernel-module/forge_core.c` — The dumb-executor kernel char device: /dev/forge_core, FORGE_IOCTL_SEND_KALL + FORGE_IOCTL_GET_STATUS, mutex-guarded, KALLv1 magic check, MODULE_VERSION 1.2
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/include/kall.h` — struct KALL: magic[6]='KALLv1', uint32 id, source[16], dest[16], operation[32], payload[1024], signature[64] — the IPC envelope
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/quorum.c` — Korum implementation: minimal JSON parser (allowed_operations + forbidden_patterns ONLY), korum_check (forbidden-first, then allow-list, else 0 = escalate to Kainito), stub hot-reload thread, korum_add
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/quorum.json` — Checked-in Korum config v1.2: allowed_sources, allowed_operations (forward/query/self_mod/register/status/consent/audit), trusted_agents, paranoia_level:25, self_mod_consent_required:true, auto_trust_mode:false — most fields never parsed by quorum.c
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/kines.c` — Kines fast gatekeeper: Unix socket /run/forge/court.sock, reads sizeof(KALL), Korum fast path / FORBIDDEN / forward to Kainito via C bridge
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/kainito_agent.cpp` — Kainito judge (C++): default-deny, destructive-pattern block (+30 paranoia), self_mod auto-grant (-10 paranoia) that writes a patch file via system(), baseline approve for syscall:/ping, fallback 'Needs Consent review'; also the extern C bridge kainito_judge_c
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/kainito.h` — KainitoDecision {approved, reason[256], signed_kall[128], patch_path[256], paranoia_delta} + Kainito class with judge() and request_self_modification()
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/kainito_judge_stub.c` — C stub judge so kines links without C++; mirrors the same 4 rule branches
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/court/kainito_audit.log` — Immutable audit log format: 'epoch | event | path | paranoia=N'; shows patch_applied + self_mod_auto events at paranoia=25
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/console/README.md` — Console + Consent (Phase 2.0) stub — the feature list that is effectively K-FAFO's spec seed: audit viewer, diff preview, Consent approval UI/CLI, paranoia tuning, rollback
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/core/cauldron/README.md` — Cauldron (Phase 1.6) stub: receives validated KALLs, neo-organic transforms, sandboxed contexts, 3B model hook later
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/forge-initramfs.list` — initramfs manifest: busybox, forge_core.ko under lib/modules/7.0.0-lts/, Court binaries, init
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/start_forge.sh` — Early-boot Court launcher (initramfs phase): modprobe forge_core, start kainito + kines, announce /run/forge/court.sock
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/build_and_test.sh` — Phase 1.5.1 build+smoke script; builds Court, runs kainito --status/--self-mod, preps /run/forge
- `/home/user/KlaudeKode/GB_runbookv1-FORGE/patches/` — 3 kernel 'patches' — forge-core-driver.patch and early-boot-hook.patch are marker/pseudo-patches (not appliable); minimal-config-disables.diff disables SELinux/AppArmor/TOMOYO/SecureBoot/TPM/FORTIFY/HARDENED_USERCOPY/UBSAN
- `/home/user/KlaudeKode/forge/forge-os-buildplant/BUILD-READINESS-20260623.md` — Build-readiness rollup: YELLOW verdict, blockers B1-B4 (firewalld missing, nvidia key-injection manual, [forge-local] empty, build order operator-enforced), 4-gate order of operations
- `/home/user/KlaudeKode/forge/forge-os-buildplant/CHANGELOG.md` — FLuX-Live revision log: first complete ISO built 2026-06-24 (sha256 2d586a7a...), linux-ff 7.0.13 prebuilt, key ceremony done, r7 Kyn Kross work through 06-27
- `/home/user/KlaudeKode/forge/forge-os-buildplant/kernel-coa-b/SUPERSEDED.md` — kernel-coa-b promoted to VoW staging3/kernel and rebased 6.18 -> mainline linux 7.0.13.arch1 ('there is no linux-lts 7.0'); uname -r = 7.0.13-arch1-1-ff
- `/home/user/KlaudeKode/forge/forge-os-buildplant/profile-final/airootfs/usr/local/bin/flux-hud` — THE FLuX-Shell HUD (bash, read-only by design): Royal Night palette, posture panel, live telemetry, KYN MODULE SPACE panel with AWAITING KROSS states, command menu, optional --net probe
- `/home/user/KlaudeKode/forge/forge-os-buildplant/profile-final/airootfs/etc/greetd/config.toml` — greetd + tuigreet session design: initial_session auto-login to labwc, default_session returns greeter on logout (kiosk-ish), rationale documented inline
- `/home/user/KlaudeKode/forge/forge-os-buildplant/scaffolds/kall-konsole-to-kernel.c` — KONSENT->KONSOLE->KERNEL trusted-path scaffold: KOMMAND (door=KONSOLE, carries authority) vs KALL (door=KODER, proposal only); korum_amend refuses every door but KONSOLE; depends on kinase.h/korum.h NOT present in this repo
- `/home/user/KlaudeKode/forge/forge-os-buildplant/forge-koder-mini/README.md` — The Konduit seam: BK's inert Kyn bundle drop point, 7-criteria Kross gate (inert, sha256 manifest, scope-lock, no relay fork, signing-safe, provenance, slow-is-smooth)
- `/home/user/KlaudeKode/forge/forge-os-buildplant/profile-final/packages.x86_64` — 297-line final package list; terminal = foot ('Wayland-native, lean') line 120; firewalld B1 fix landed line 238

### Architecture

TWO DISTINCT LINEAGES SHARE THIS SUBSYSTEM. (1) GB_runbookv1-FORGE ("Grok Builder" lineage, C-spellings: Consent/Coder/Compiler/Customs/Quorum): message flow per Runbook v1.7 sec.4 is Consent -> Coder -> KALL -> Cauldron (Compiler + Customs) -> Court -> Kines + Korum (fast) -> Kainito (judge + self-mod) -> approved KALL -> /dev/forge-core ioctl -> Kernel -> response back. The wire unit is struct KALL (include/kall.h): fixed ~1150 bytes, magic "KALLv1", id, source[16], dest[16], operation[32], payload[1024], signature[64]. Kernel side (kernel-module/forge_core.c): a char device with mutex-serialized ioctl; SEND_KALL validates magic, logs, echoes back; GET_STATUS returns {version 0x0700, call count, last_id, state "dumb-executor"}; zero policy. Userspace Court: kines.c binds AF_UNIX SOCK_STREAM at /run/forge/court.sock, reads one struct KALL per connection, calls korum_check() — forbidden_patterns strstr match on payload/operation returns -1 (write "FORBIDDEN"), exact operation match on allow-list returns 1 (write "PASSED_FAST_PATH"), else 0 escalates to kainito_judge_c() (C bridge in kainito_agent.cpp or the stub). Kainito::judge is default-deny with 4 branches: destructive strstr block (paranoia_delta +30); "self_mod" in payload -> approve + request_self_modification() which writes a patch file to /tmp and system()-chmods it (paranoia_delta -10); "syscall:"/"ping" baseline approve with signed_kall="KALLv1-SIGNED-BY-KAINITO"; else reason "Needs Consent review". Korum hot-reload thread is a sleep-loop stub. Build: core/court/Makefile builds kines (gcc, links the C stub judge) and kainito (g++, real judge) separately; prebuilt ELF binaries are checked in. Boot story: forge-initramfs.list + start_forge.sh (insmod forge_core.ko, launch kainito + kines from initramfs before real init); patches/ are marker pseudo-patches, not appliable. Phases complete per v1.2: 0/0.5/0.6; open: 1.5 socket wiring, 1.6 Cauldron, 2.0 Console+Consent, 2.5 3B model hook, 3.0 Kanto 2 Cage mesh (craft drivers, cage_kines validating inter-driver mini-KALLs). (2) forge-os-buildplant ("FLuX-Live" lineage, K-spellings): an archiso profile chain (profile 128 pkgs -> profile-min 226 -> profile-final 297) building a hardened live ISO on linux-ff 7.0.13-arch1-1-ff (custom self-signed kernel, MODULE_SIG_FORCE, lockdown=integrity via cmdline, no Secure Boot — COA-B trust anchored at the kernel; ECDSA P-384/sha512 signing key injected at build, used, then shredded before squashfs). Boot: systemd-boot entries (flux-live-x86_64-linux-ff + fallback + rescue), copytoram=y, greetd on vt1 with initial_session auto-launching labwc/Wayland as liveuser, tuigreet as fallback greeter. Console surface: foot terminal, motd command menu, and flux-hud — a pure-bash, read-only HUD (reads only /proc, /sys, file presence) rendering four sections (HARDENING POSTURE: lockdown/module-sig/firewall/userns/copytoram/mitigations rows; LIVE TELEMETRY: CPU temp/load/clock/RAM + nvidia-smi GPU row; KYN MODULE SPACE: module-space/brain/runtime/prompts rows with baked vs AWAITING KROSS states; COMMANDS menu: desktop/claude/kyn/flux-hud/flux-trace) plus optional --net connectivity probe (gateway/internet/DNS rows). Row grammar: rule() 66-char gold '=' line; hdr() purple section header; row(label,state,color,detail) = lavender %-13s label, colored %-22s state, grey detail. The scaffolds/kall-konsole-to-kernel.c bridges the two lineages using the KANON vocabulary: one forge_kall_t rides both KOMMAND (door=KONSOLE, authority) and Kall (door=KODER, proposal); path is Konsent --types--> KONSOLE --[.door=KONSOLE]--> kinase_kross() [door -> Korum/quorum -> Assay -> admit -> kommit] --ADMIT--> KERNEL.enact (two-phase bind / IOMMU; no model in the loop). BUILD-READINESS (06-23) was YELLOW with B1-B4; CHANGELOG shows B1 (firewalld) and F2 (root shell bash) fixed in profile-final and a complete ISO built 06-24; authoritative kernel build tree is OFF-REPO at /run/media/liveuser/VoW/forge-os/staging3/.

### Theme tokens

- flux-hud:13 — 'Royal Night palette (matches r6 theme): deep purple / gold / lavender.'
- flux-hud:20 — GOLD=$'\033[38;5;220m'; GOLDB=$'\033[1;38;5;221m'; LAV=$'\033[38;5;189m'
- flux-hud:21 — PUR=$'\033[38;5;141m'; GREY=$'\033[90m'; OKC=$'\033[1;32m'; BADC=$'\033[1;31m'
- flux-hud:22 — WARN=$'\033[1;33m'; DIM=$'\033[2m'; R=$'\033[0m'
- flux-hud:19 — palette stripped when '[ -t 1 ]' fails or NO_COLOR is set (tty-detect + NO_COLOR convention)
- flux-hud:27 — rule() = 66-char '=' horizontal rule rendered in GOLD
- flux-hud:30 — row grammar: label lavender %-13s, state colored %-22s, detail GREY (fixed column widths 13/22)
- flux-hud:85-86 — threshold coloring: temp >=85 BADC red, >=70 WARN yellow, else OKC green; load >=90 red, >=70 yellow
- flux-hud:92 — banner: 'FLuX-Live ◇ Royal Night · hardened live medium · sovereign remaster' (gold-bold title, grey diamond separator, purple subtitle)
- motd — same vocabulary in basic ANSI: 1;33 gold headings/commands, 1;35 purple, 90 grey, 1;37 white emphasis
- flux-hud:194,201,210 — 'AWAITING KROSS' state rendered in WARN yellow (consent-pending, distinct from BADC error red)
- greetd config.toml:36 — greeting string 'Forge-OS — hardened live medium (labwc/Wayland)'
- packages.x86_64:120 — 'foot # terminal (Wayland-native, lean)' — the ratified terminal package

### Consent mechanics

Dense with grant-not-command mechanics, at three layers. (a) Korum/quorum: quorum.json declares "self_mod_consent_required": true, "auto_trust_mode": false, "paranoia_level": 25; Runbook v1.2 locks the self-mod considerations — "Mandatory Consent review (default) + optional auto-trusted mode", "Paranoia auto +15 on self-mod; Consent can lower", "Immutable audit log + diff preview in Console", versioned rollback (kainito --rollback YYYYMMDD_HHMM), file limits to core/court/, core/cauldron/, include/. (b) Kainito: default-deny judge whose terminal fallback is literally "Needs Consent review"; paranoia is a mutable trust scalar with signed deltas. (c) Door-provenance (scaffold): "authority is provenance-of-channel, never payload" — a KOMMAND (door=KONSOLE) carries authority, a KALL (door=KODER) is only a proposal; "The Konsole's privilege is that it - and only it - may AMEND the Korum (korum_amend refuses every other door)" — "this is how consent becomes law"; and defense in depth: "Even the Konsent's KOMMAND Krosses the Kinase". (d) Buildplant Kross gate (forge-koder-mini): "Inert in, witnessed out" — 7 admit criteria (INERT, sha256 MANIFEST, SCOPE-LOCK human-gated 'propose -> Konsent runs', NO FORK of the relay, SIGNING-SAFE, PROVENANCE on the brain, prompts authored under slow-is-smooth); Konsent rulings are recorded as law (dual-brain ruling); flux-hud renders consent-pending as a first-class UI state ("AWAITING KROSS", warn-colored, not error). CONTRADICTION INSIDE THE MECHANICS: kainito_agent.cpp:22-29 auto-approves any payload containing "self_mod" and executes request_self_modification() immediately — no Consent gate in code despite self_mod_consent_required:true in the adjacent quorum.json, and the audit log records "self_mod_auto" events; quorum.c never parses the consent fields at all, so they are dead data in the running system.

### Strengths observed

- Layered adjudication pipeline exists in compilable code: Korum allow-list fast path -> Kines socket gatekeeper -> Kainito default-deny judge -> 'Needs Consent review' fallback; three distinct escalation outcomes (PASSED_FAST_PATH / FORBIDDEN / judge).
- struct KALL is a complete, fixed-size (framed-by-sizeof) IPC vocabulary: magic + id + source + dest + operation + payload + signature — stable across every document version even where other details drift.
- Paranoia as an explicit mutable trust scalar with signed deltas (+30 destructive, -10 granted; v1.2 locks +15 auto on self-mod, 'Consent can lower') — trust is a number the UI can display and Konsent can tune.
- Audit log format is minimal and parseable: 'epoch | event | path | paranoia=N' (kainito_audit.log).
- C API bridge pattern (kainito_c_api.h + kainito_judge_stub.c) lets the C gatekeeper link either the full C++ judge or a stub — clean seam for swapping the judge.
- Kernel-as-dumb-executor inversion is stated identically across v1.2/v1.6/v1.7 and implemented: forge_core.c contains zero policy, only magic validation, counting, and echo.
- kernel-module/forge_core.c is markedly more careful than the runbook versions: mutex serialization, copy_from_user error handling, EINVAL on bad magic, proper class/device create with failure unwinding.
- flux-hud demonstrates a full HUD design system in ~240 lines of dependency-free bash: tty-detect + NO_COLOR, fixed row grammar (rule/hdr/row), threshold coloring, graceful n/a degradation, read-only-by-design safety property, and a command menu.
- 'AWAITING KROSS' as a first-class warn-colored (not error) UI state — consent-pending rendered distinctly from failure.
- greetd config documents its own rationale inline (why greetd over tty-autologin: PAM session, seat0, XDG_RUNTIME_DIR correctness; logout returns greeter, kiosk-ish).
- Buildplant discipline: 4-gate order of operations (key ceremony -> kernel -> nvidia sign -> assemble) with loud-fail verification gates; key injected then shredded before squashfs; sha256 manifests everywhere; SUPERSEDED.md leaves a full promotion/rebase audit trail.
- BUILD-READINESS blockers (B1 firewalld, F2 root shell) are traceably CLOSED in profile-final: packages.x86_64:238 carries the fix with a comment citing B1, and airootfs/etc/passwd shows root shell /usr/bin/bash.
- A complete bootable ISO was actually produced (2026-06-24, sha256 recorded) — the buildplant lineage is proven, not aspirational.
- Door-provenance authority model in the scaffold is a working demonstration of grant-not-command: the Koder cannot grant itself; even Konsent's KOMMAND is checked (defense in depth).
- Kross-gate README enumerates 7 admit criteria concretely enough to execute (inert, manifest, scope-lock, no-fork, signing-safe, provenance, slow-is-smooth); the CHANGELOG shows the gate actually catching a real bug (the '|| echo 000' false-PASS in forge-restore-u.sh).

### LIMFACs observed

- core/console (the Console + Consent surface K-FAFO would occupy) is a README-only stub — no code exists to extend; same for core/cauldron.
- kernel-module/forge_core.c will not compile against modern kernels: class_create(THIS_MODULE, CLASS_NAME) at line 133 uses the pre-6.4 two-argument API, while the buildplant kernel is 7.0.13.
- patches/forge-core-driver.patch and early-boot-hook.patch are non-appliable marker/pseudo-patches (marker body of 4 comment lines vs claimed 140 insertions; malformed hunk context).
- quorum.c parses only allowed_operations and forbidden_patterns; the consent-critical fields of quorum.json (self_mod_consent_required, auto_trust_mode, paranoia_level, allowed_sources, trusted_agents, max_payload) are dead data.
- Checked-in quorum.json has no forbidden_patterns, so the loaded Korum forbids nothing; hardcoded forbidden defaults apply only when the file is missing.
- Kainito auto-approves self_mod in code (kainito_agent.cpp:22-29) with a fixed hardcoded patch path (/tmp/kainito_selfmod_20260629.cpp) and system() calls — contradicts self_mod_consent_required:true; the mandatory-Consent-review design exists only in prose.
- Korum hot-reload is a stub ('In real version would stat() the file and reload' — quorum.c:108); hot-reload is claimed 'completed & tested' in Runbook v1.2.
- kines.c has no error handling on socket/bind/listen/read, single fixed-size read per connection, and unframed variable-length text responses — protocol is demo-grade.
- Device ABI is split: /dev/forge-core + magic 'f' + SEND/RECV (runbooks, start_forge.sh) vs /dev/forge_core + magic 'F' + SEND/GET_STATUS (actual module) — no client can target both.
- forge-initramfs.list pins lib/modules/7.0.0-lts/forge_core.ko — a kernel version that does not exist ('there is no linux-lts 7.0').
- Runbook internal inflation: '12 baseline rules' claimed, 4 branches implemented; v1.6 claims verbatim completeness while eliding code; paranoia printed as 50, configured as 25.
- scaffolds/kall-konsole-to-kernel.c cannot compile in this repo: kinase.h/korum.h (kore/) are absent — the KANON-side implementation lives off-repo in doktrine/forge-os.
- Authoritative buildplant artifacts are off-repo: staging3/kernel (prebuilt linux-ff repo, PKGBUILD, KEY-CEREMONY.md, build-flux-live.sh) and the built ISOs live on the VoW drive at /run/media/liveuser/VoW/forge-os/ — this repo's copy is docs + profile only.
- The [forge-local] pacman repo, signing key, and build wrapper are environment-bound to Ryne's box (ASRock Z890 / RTX 5090); reproducing the ISO elsewhere requires re-running the key ceremony and kernel compile.
- Two unreconciled vocabularies (Consent/Coder/Quorum vs Konsent/Koder/Korum) and two unreconciled KALL shapes (struct KALL with source/dest/operation strings vs forge_kall_t with door/verb/fields/witness) across the two lineages.
- Prebuilt ELF binaries committed without a documented rebuild environment (dynamically linked, GNU/Linux 4.4.0 target).
- 'Execute' (IRE), 'Consolodate'/'Verify' (RCV) remain user-undefined per project CLAUDE.md — procedures referenced by the workflow that produced these docs are themselves open seats.
- Konsole (KDE) is not in any packages.x86_64 — the baked terminal is foot; no Qt/KDE stack ships in the image, constraining what 'modeled off Konsole' can reuse at the binary level.

### K-FAFO patterns to adopt

- flux-hud's panel grammar as K-FAFO's pane vocabulary: rule() gold horizontal rule, hdr() purple section header, row(label,state,color,detail) with fixed 13/22 column widths, threshold coloring (>=85 red / >=70 yellow / green), graceful 'n/a' degradation — the FLuX-Shell HUD is already a four-panel layout (POSTURE / TELEMETRY / MODULE SPACE / COMMANDS) that maps directly to K-FAFO panes.
- Royal Night ANSI tokens as the terminal-side complement to the KK-1 theme/ hex tokens: 256-color gold 220/221, purple 141, lavender 189, grey 90; NO_COLOR + tty-detect stripping as a portability convention.
- 'AWAITING KROSS' as a first-class consent-pending UI state (warn yellow, not error red) — K-FAFO should render ungranted/unwitnessed things as awaiting, never as broken.
- 'Read-only by design' as a stated, checkable safety property for any K-FAFO status/observer pane: only /proc, /sys, and file presence; no writes, no network, no module loads — survives lockdown=integrity and read-only squashfs.
- struct KALL as the IPC envelope precedent for K-FAFO <-> Kosmos/Kore messaging: fixed-size framed struct over an AF_UNIX socket (the /run/forge/court.sock pattern), magic-validated, with source/dest/operation/payload/signature fields; kernel side stays a dumb transport.
- Door-provenance authority for the explorer itself: K-FAFO as a KONSOLE-class door — 'authority is provenance-of-channel, never payload'; user-initiated actions through K-FAFO are KOMMANDs, anything model-originated is a Kall/proposal; only the KONSOLE door may AMEND the Korum ('this is how consent becomes law'); even Konsent's KOMMAND still Krosses the check (defense in depth).
- core/console/README.md's planned feature list as K-FAFO's consent-pane spec seed: immutable audit log viewer, diff preview for self-mod requests, Consent approval UI/CLI, paranoia tuning, rollback interface — this Phase 2.0 slot is empty and K-FAFO is positioned to be its first implementation.
- The audit-log line format 'epoch | event | path | paranoia=N' as a directly renderable log-viewer row schema.
- Paranoia as a displayed, Konsent-tunable trust scalar with visible signed deltas per decision — a live HUD element.
- greetd initial_session pattern for launching K-FAFO (or the FLuX-Shell hosting it) as the boot session, with tuigreet-style fallback so a dead shell returns to a greeter instead of a raw tty.
- The Kross-gate seam pattern (inert bundle + sha256 manifest + human-gated integrate) for how K-FAFO itself gets baked into the image; flux-hud's presence-gated feature detection (feature rows flip baked/awaiting by file existence) for progressive capability.

### K-FAFO constraints

- Substrate is labwc/Wayland + foot on linux-ff 7.0.13 with lockdown=integrity and module.sig_enforce=1: K-FAFO must be Wayland-native, pure userspace, signing-safe, and must not require kernel writes or unsigned modules; unprivileged userns is available for bwrap sandboxing.
- No Qt/KDE stack in any baked profile — Konsole (KDE) can be modeled (behavioral: tabs, profiles, split views) but not linked/reused without Konsent adding Qt to packages.x86_64; the current live session additionally has no Node runtime per KK-1 CLAUDE.md, though the ISO's F11 operator-adds include nodejs/npm (two different environments).
- The Court socket protocol is unframed fixed-struct with no error handling and two incompatible device ABIs (/dev/forge-core vs /dev/forge_core, magic 'f' vs 'F') — K-FAFO cannot bind to this IPC as-is; a ratified KALL dialect must precede any wire code.
- The consent enforcement K-FAFO would surface does not exist yet in code: self_mod_consent_required is unenforced, quorum consent fields are unparsed, hot-reload is a stub — K-FAFO would be displaying/enforcing a contract the substrate does not yet honor.
- kinase.h/korum.h (the KANON-side Kinase/Korum the scaffold trusts) are off-repo in doktrine/forge-os, which the FRESH START declaration makes reference-only, never binding canon — K-FAFO's authority model must be reborn in KK-1, not linked from doktrine.
- Everything that persists must live on the removable drive; the OS is ephemeral by design (copytoram live medium) — K-FAFO state/config must target /mnt/VoW-style persistent mounts, never /home or /tmp.
- Authoritative build artifacts (linux-ff repo, signing key ceremony, build-flux-live.sh, built ISOs) are on the VoW drive, not in this repo — a K-FAFO bake requires access to that off-repo staging3 plant.

### Open questions (Konsent's)

- Is K-FAFO the Phase 2.0 Console + Consent itself (the KONSOLE door, only amender of the Korum), or a separate explorer that talks TO the Console? Only Konsent can seat this.
- Which KALL dialect is canon for K-FAFO IPC: the Grok-lineage struct KALL (source/dest/operation/payload/signature, C-spellings) or the KANON forge_kall_t (door/verb/fields/witness, K-spellings)? They contradict and both appear in this subsystem.
- Does 'modeled off Konsole' mean behavioral modeling only (tabs/profiles/splits reimplemented natively) or does Konsent want Qt/KDE added to the image to reuse Konsole code?
- Which consent rule is law for K-FAFO to display and enforce: quorum.json's self_mod_consent_required:true, or the code's auto-grant + self_mod_auto audit trail? (The Remedidate on this contradiction is Konsent's.)
- Who owns the paranoia number and its baseline (25 in quorum.json/audit vs 50 printed by Kainito), and is paranoia a K-FAFO-visible, K-FAFO-tunable control?
- Is the GB_runbookv1-FORGE lineage (Grok Builder, C-spellings) mineable-reference-only under the FRESH START declaration like doktrine/, or does any of it carry binding status into KK-1?
- Does K-FAFO target the existing FLuX-Live ISO substrate (labwc/foot/greetd, proven bootable) or the K-Eden Kosmos v0 PID1 design from kode/keden/SCAFFOLD.md — i.e., which boot story is it born into?

### Verbatim

- “Linux 7.0 LTS kernel = pure dumb executor ... All decisions & security live exclusively in the Core (Court → Cauldron → Console)” — `GB_runbookv1-FORGE/Forge_OS_Runbook.md:5-6`
- “Kines • Korum (Quorum) • Kainito (Kyneeto) • Compiler • Customs • Coder • Consent • KALL • Kanto • Craft drivers • Forge OS” — `GB_runbookv1-FORGE/Forge_OS_Runbook.md:12 (Locked Terminology)`
- “Every neo-organic AI **must** contain `request_self_modification(reason, suggested_code)`” — `GB_runbookv1-FORGE/Forge_OS_Runbook.md:8`
- “Planned:
- Immutable audit log viewer
- Diff preview for self-mod requests
- Consent approval UI / CLI
- Paranoia tuning
- Rollback interface: kainito --rollback ...

Status: stub. Ready for Phase 2.0.” — `GB_runbookv1-FORGE/core/console/README.md:5-12`
- “"self_mod_consent_required": true,
  "auto_trust_mode": false,” — `GB_runbookv1-FORGE/core/court/quorum.json:25-26`
- “strcpy(dec.reason, "❓ Needs Consent review");” — `GB_runbookv1-FORGE/core/court/kainito_agent.cpp:37`
- “1782700034 | self_mod_auto | test phase 1.5 | paranoia=25” — `GB_runbookv1-FORGE/core/court/kainito_audit.log:2`
- “In the Kanon's exact vocab this is a KOMMAND, not a Kall: "authority is provenance-of-channel, never payload" (kore/kinase.h). A KOMMAND (door = KONSOLE) carries authority; a KALL (door = KODER) is only a proposal.” — `forge/forge-os-buildplant/scaffolds/kall-konsole-to-kernel.c:6-9`
- “The Konsole's privilege is that it - and only it - may AMEND the Korum (korum_amend refuses every other door).” — `forge/forge-os-buildplant/scaffolds/kall-konsole-to-kernel.c:36-37`
- “Only the KONSOLE door may write the Korum - this is how consent becomes law.” — `forge/forge-os-buildplant/scaffolds/kall-konsole-to-kernel.c:47-48`
- “Renders the live posture of the hardened medium on console login (and on demand). Read-only by design: it ONLY reads /proc, /sys, and presence of files. No network calls, no writes, no module loads — safe under lockdown=integrity, safe offline, safe on the read-only squashfs.” — `forge/forge-os-buildplant/profile-final/airootfs/usr/local/bin/flux-hud:4-8`
- “Inert in, witnessed out. BK delivers a bundle here; the image owner Krosses every byte, then integrates it into profile-final/airootfs/usr/lib/forge/. Nothing here executes before the Kross.” — `forge/forge-os-buildplant/forge-koder-mini/README.md:7-9`
- “rebased from linux-lts 6.18.36 to mainline `linux` 7.0.13.arch1 (operator Ryne's directive: there is no `linux-lts 7.0`...). `uname -r` is now **7.0.13-arch1-1-ff**.” — `forge/forge-os-buildplant/kernel-coa-b/SUPERSEDED.md:5-9`
- “`out/flux-live-2026.06.24-x86_64.iso` · sha256 `2d586a7abc5328ad9797a3b1773dde05e04c40e35424421cd86c8f167eabbd04` — First complete build. B2 in-chroot sig gate PASSED (6357 modules, nvidia forge-signed).” — `forge/forge-os-buildplant/CHANGELOG.md (Revision log, host-built 2026-06-24)`
- “You are Grok Builder. This entire runbook is the ONLY source of truth.” — `GB_runbookv1-FORGE/Forge_OS_Runbook_v1.6.md:135`
- “printk(KERN_EMERG "Forge OS: /dev/forge-core ACTIVE — kernel is now dumb executor only.\n");” — `GB_runbookv1-FORGE/Forge_OS_Runbook_v1.7.md:52 (vs DEVICE_NAME "forge_core" in kernel-module/forge_core.c:21)`
- “Konsent ruled **dual-brain: bake both, bake-off picks the winner**” — `forge/forge-os-buildplant/CHANGELOG.md (2026-06-27 PM entry)`

### Flags

- PROVENANCE: Runbook v1.6 sec.6 opens 'You are Grok Builder. This entire runbook is the ONLY source of truth.' — the whole GB_runbookv1-FORGE lineage is an external-model (Grok) build workflow, using C-spellings (Consent, Coder, Quorum, Compiler, Customs) where the KANON/buildplant lineage uses K-spellings (Konsent, Koder, Korum). Two vocabularies coexist and are NOT reconciled.
- CONSENT CONTRADICTION: quorum.json says self_mod_consent_required:true / auto_trust_mode:false, but kainito_agent.cpp:22-29 auto-approves self_mod and fires request_self_modification() with no gate; kainito_audit.log records 'self_mod_auto' events at paranoia=25. Code and config disagree on the single most consent-critical action.
- DEAD CONFIG: quorum.c parse_simple_json (quorum.c:8-52) parses ONLY allowed_operations and forbidden_patterns; allowed_sources, trusted_agents, paranoia_level, self_mod_consent_required, auto_trust_mode, max_payload in quorum.json are never read.
- SILENT-PERMISSIVE: the checked-in quorum.json v1.2 contains NO forbidden_patterns array, so when the file loads, num_forbidden=0 — the Korum fast layer forbids nothing; the hardcoded forbidden defaults (rm -rf, reboot -f, modprobe, insmod) apply ONLY when quorum.json is MISSING (quorum.c:64-78).
- KERNEL API ROT: kernel-module/forge_core.c:133 calls class_create(THIS_MODULE, CLASS_NAME) — that two-arg API was removed in Linux 6.4; the module will not compile against the buildplant's actual linux-ff 7.0.13 kernel.
- KERNEL PREMISE CONTRADICTION: runbook philosophy says 'Linux 7.0 LTS kernel'; kernel-coa-b/SUPERSEDED.md states 'there is no linux-lts 7.0; 7.0 ships only as mainline linux / linux-zen / linux-hardened' — buildplant rebased to mainline 7.0.13.arch1. forge-initramfs.list still pins lib/modules/7.0.0-lts/forge_core.ko.
- DEVICE NAME SPLIT: runbooks + start_forge.sh use /dev/forge-core and ioctl magic 'f' with SEND/RECV; kernel-module/forge_core.c uses DEVICE_NAME 'forge_core' (/dev/forge_core), magic 'F', and SEND/GET_STATUS. Same struct, incompatible ABIs across documents vs code.
- PSEUDO-PATCHES: patches/forge-core-driver.patch claims '140 insertions' but its body is 4 comment lines ('This is a minimal patch marker'); early-boot-hook.patch has malformed hunk context — neither is appliable to a real kernel tree.
- MISSING DEPS: scaffolds/kall-konsole-to-kernel.c includes kinase.h and korum.h which exist NOWHERE in this repo (they live in the off-repo doktrine/forge-os kore/); the scaffold cannot compile here.
- OFF-REPO AUTHORITY: the authoritative kernel build location is /run/media/liveuser/VoW/forge-os/staging3/kernel/ (SUPERSEDED.md) and the built ISO lives on VoW — this repo holds the profile and docs but not the artifacts; CHANGELOG says the prebuilt kernel repo is 'DO NOT RECOMPILE'.
- BOOTABILITY SPLIT: the Grok-lineage Forge OS is demo-grade (userspace Court binaries run on any Linux; kernel/initramfs path notional), while the FLuX-Live buildplant lineage HAS a complete bootable ISO (flux-live-2026.06.24-x86_64.iso, sha256 2d586a7abc5328ad9797a3b1773dde05e04c40e35424421cd86c8f167eabbd04) — 'how close to bootable' has two answers.
- VERSION-CLAIM INFLATION: v1.2 claims 'Kainito: 12 baseline rules'; the actual judge has 4 branches. v1.6 claims 'all code verbatim' while eliding bodies ('// bind + listen code as previously shown'); v1.7 was created to fix exactly that.
- PARANOIA NUMBER SPLIT: Kainito constructor prints 'paranoia=50' (kainito_agent.cpp:10) while quorum.json and the audit log say paranoia=25.
- Prebuilt ELF binaries (kines 17KB, kainito 27KB, not stripped, BuildID present) are committed to the repo alongside their sources.
- BUILD-READINESS-20260623.md is titled 'BUILD-READINESS VERDICT' with 'OVERALL VERDICT: YELLOW' — a verdict-style document exists in the corpus despite the Teksidure no-verdicts rule (it predates KK-1 norms; operator named as Ryne).
- Buildplant packages.x86_64 includes nodejs/npm (F11 operator-adds) — tension with KK-1 CLAUDE.md's 'No Node.js runtime' environment note; the live ISO and the current live session are different environments.


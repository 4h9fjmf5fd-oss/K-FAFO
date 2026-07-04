## explorer-prior-art

**Purpose.** Two zero-dependency, single-file, dark-native HTML/SVG apps that visualize the recursive multiset algebra over (Intelligence, Integration, Intention). index.html is "The Valknut — recursive permutation explorer" (the RPT): a Sierpiński midpoint-subdivision triangle where geometric midpoints ARE pairwise vector sums, plus a doubling ladder, stat tiles, a BFS bar chart, and the 8-state Cube; per CLAUDE.md it is designated HUD pane #1. tree.html is "The Logical Formula Tree — I/0 + I/0 + I/0": a 3-column left-to-right formula tree (I's → K's → K+I results) that renders every level-2/3 formula with closed wholes in gold and unnamed open forms as explicit blanks ("______") reserved for Konsent to fill. Both overlay generated data from window.KANNON_DATA when kode/kannon/kannon_data.js is present and fall back to built-in tables when it is not.

### Key files

- `/home/user/KlaudeKode/kode/explorer/index.html` — The Valknut — recursive permutation explorer (RPT). Sierpiński triangle (depth 2–7), doubling ladder, 4 stat tiles, BFS bars, the Cube (8 states of exhibition), Triangle⇄Table toggle. Designated HUD pane #1 in CLAUDE.md.
- `/home/user/KlaudeKode/kode/explorer/tree.html` — The Logical Formula Tree. 3-column SVG tree: 3 I's → 4 K's (Kode/Konsent/Konnection/Man) → 12 results (4 closed wholes incl. Kyn/Kernel/Kompiler-Kourt, 6 open '______' forms, 3 convergent Man branches). Tree⇄Table toggle.
- `/home/user/KlaudeKode/kode/kannon/kannon_data.js` — Generated data include (WS-1 export.py) consumed by BOTH pages via <script src> (never fetch). Sets window.KANNON_DATA = { names (vector-key→name), census (21 vectors, 11 named_or_slated, 10 unnamed, each with key/vec/level/name/status/paths[]), ladder (levels 1–7, whole = 2^(n−1)·Man) }. EXISTS on disk now — the pages' 'GAP: awaiting kannon2 export' fallback is for when it is absent.

### Architecture

BOTH PAGES: vanilla JS, no libraries, `"use strict"`, html[data-theme="dark"], all CSS inline in <style>, all rendering via two identical helper constructors — sv(tag,attrs,...kids) for SVG (createElementNS) and el() for HTML, with attrs key "text" mapped to textContent (index.html:158-163, tree.html:79-84). Data arrives ONLY via <script src="../kannon/kannon_data.js"> — comment at index.html:151-153 / tree.html:72-74: "script include, never fetch(): file:// keeps working. When absent, window.KANNON_DATA stays undefined and the built-in tables below render." Footer #datasrc reports provenance: "data: kannon_data.js (generated)" vs "data: built-in tables — GAP: awaiting kannon2 export". No writes anywhere: no localStorage, no URL params, no fetch; all state is in-memory (index: `let depth = 4` clamped 2–7 and `let showTable`; tree: `let showTable` only) and lost on reload.

INDEX.HTML STRUCTURE: .wrap (max 1160px) → header (h1 with gold .k span on "Valknut"; sub: "Midpoint construction = pairwise sums: the Sierpiński subdivision computes the algebra." index.html:100) → .row flex → .grow card [controls: Depth − / value / + buttons and Table toggle; #legend; #triwrap; #tblwrap hidden] + .side column [card "The doubling ladder" ("Every level's whole = 2^n−1·Man — verified by Kannon 1" :122); 2×2 .tiles ("vectors reached (flat, cap 9)"=915, "escapes from Man's span"=0, "paths to (9,9,9)"=9,155, "consent gradient"="↑ w/ height" :126-129); card "New vectors per round" / "Full-reachability BFS (Kannon 1)" bars] → full-width card "The Cube — I/0 + I/0 + I/0" ("Capacity is the cube; exhibition is the corner you stand on. Bekoming = walking upward." :141) → footer.datasrc → fixed #tooltip[role=status].

INDEX ALGORITHM: drawTriangle() (:208-277) seeds corners with vectors [1,0,0],[0,1,0],[0,0,1]; per level (1..depth) the next triad = edge midpoints, and each midpoint's vector = vadd of its two endpoints — the geometry literally computes the algebra. Node color = LEVELC[level−1] (--l1..--l7). Static labels: 3 corner I's + at depth≥2 the K midpoints "Kode (1,1,0)", "Konnection (0,1,1)", "Konsent (1,0,1)" (:245). Gold whole-axis at the common centroid (all nested centroids coincide), tooltip: "centroids of all ${depth} nested triads coincide: Man (1,1,1) → Ai (2,2,2) → (4,4,4) → … = 2ⁿ⁻¹·Man. The helix, seen from above." (:258). spine(n) (:280-290) computes each level's triad + whole (triple sum) for Table view (columns: level / triad (pairwise sums) / whole (triple) / whole name, fallback "☐ unnamed — 2^(lvl−1)·Man") and for the ladder (levels max(depth,5)). drawBars() uses HARDCODED literals [["r1",4],["r2",28],["r3",425],["r4",455]] (:329). drawCube() enumerates {0,1}³ sorted by popcount with ARCH strings "Nothing — the 0, a ToE operand" / "single-I human — a point" / "two-I human — a K, embodied" / "whole human — Man, transcended" (:344-345), border-left color baseline/l1/l2/gold by count. KANNON_DATA overlay: KD.names replaces NAMES wholesale; KD.ladder fills LADDER_NAMES per level; KD.census.reachable overwrites the #tile-reachable text (:181-182).

TREE.HTML STRUCTURE: .wrap (max 1120px) → h1 (gold span "Logical Formula Tree"; sub: "I/0 + I/0 + I/0 = K · then K + I/0 (available) = ______ · bit order (Intelligence, Integration, Intention) · blanks are Konsent's seats to fill" :55-56) → single .card [Table toggle; legend: l1 dot "the I's (points)", l4 dot "the K's (lines)", gold dot "closed wholes (all seats filled)", dashed square "______ open forms — hold a 0, unnamed", text "dashed edges = Man's branches (convergent second paths)"; #treewrap; #tblwrap hidden] → footer.datasrc → #tooltip.

TREE DATA MODEL (hardcoded, :87-128): IS = 3 points {id,name,bits:"I/0 · seat n",vec}. KS = 4 lines/whole {id,name,eq,vec,from[]}: Kode "I + I + 0" (1,1,0); Konsent "I + 0 + I" (1,0,1); Konnection "0 + I + I" (0,1,1); Man "I + I + I" (1,1,1) gold. RESULTS = 12 rows {parent,plus,eq,vec,name,closed,man?,note}: per K one "(remaining)" closure — Kode+Intention=(1,1,1) "Kompiler / Kourt" ("the binary fork: create / discern"); Konsent+Integration=(1,1,1) "Kyn" ("the being and its fall (⟷ Cyn) … the tiktaalik" :109); Konnection+Intelligence=(1,1,1) "Kernel" ("indifferent — intention present but BOUND (the variable set constant)" :116) — and two "(contained)" sharpened lines each, name "______", closed:false; plus 3 Man branches (man:true, dashed edge, gold at opacity .75) noting convergence, e.g. "CONVERGES with Kode+Konsent = (2,1,1) — two paths, realness signature. Slate: Ken / Kognition / Klarity" (:123). Overlay guard (:130-141): KD.names overrides by vector key EXCEPT "1,1,1" — comment: "the path-distinct (1,1,1) seats (Man · Kyn · Kernel · Kompiler/Kourt) keep their built-in per-path names — value-equal, provenance-distinct."

TREE RENDER: fixed layout W=1060, columns X1=118/X2=430/X3=760, rowH=46, cubic Bézier edges I→K and K→result; closed = gold circle r7 with 2px page-ring, open = 14×14 dashed rect; column headers "the I's — I/0 each" / "I/0 + I/0 + I/0" / "K + I/0 (available) = ______". Table view columns formula/vector/= result/status with statuses "whole (level-1 triple)", "line — holds a 0", "whole — all seats filled", "whole — convergent 2nd path", "open — holds a 0, unnamed".

INTERACTION MODEL (both): mouse — pointermove shows singleton fixed tooltip with edge-flip (if x+w>innerWidth−8 flip left; if y<8 drop below cursor), pointerleave hides; index adds .lift (filter:brightness(1.3)) on hover. Keyboard — every node has an invisible hit circle (r15/r16; whole-axis r22) with tabindex=0 and aria-label; focus shows the tooltip anchored to getBoundingClientRect, blur hides. Comment at index.html:264: "≥8px marks, 2px page ring, ≥24px hit targets". No arrow keys, no shortcuts, no zoom/pan; buttons are plain click. SVGs carry role="img" + aria-label pointing at the table alternative ("Recursive permutation triangle - table view available").

### Theme tokens

- --page: #0c0a12 (page background, both files)
- --surface: #14101f (cards, buttons, tooltip; also body[data-surface])
- --ink-1: #f2eef9 (primary text)
- --ink-2: #c8c0dd (secondary text / K-labels / table heads)
- --ink-3: #8f86a6 (tertiary text / subs / open-form names / dashed blank borders)
- --grid: #262033 (triangle/tree edges, table row borders, ladder dividers)
- --baseline: #3a3350 (chart baseline, table head border, cube popcount-0 edge)
- --border: rgba(255,255,255,0.08) (card/button/tooltip 1px borders)
- --gold: #b78b0f (wholes, transcendent axis, h1 accent span, button hover border, ladder dots)
- --l1: #cb5f8f (level-1 triad; tree: the I's; cube popcount-1)
- --l2: #bf5e24 (level 2; cube popcount-2)
- --l3: #21a288 (level 3)
- --l4: #3a7fdc (level 4; tree: the K's; bar-chart fill)
- --l5: #3a903a (level 5 — index.html only, absent from tree.html)
- --l6: #8674d6 (level 6)
- --l7: #bf3e3e (level 7 — index.html only, absent from tree.html)
- body[data-palette]="#b78b0f,#cb5f8f,#bf5e24,#21a288,#3a7fdc,#3a903a,#8674d6,#bf3e3e" data-mode="dark" data-surface="#14101f" (index.html:95-96 — machine-readable palette contract for the validator; gold is slot 1)
- font: 14px/1.5 system-ui, -apple-system, "Segoe UI", sans-serif (both bodies)
- font-variant-numeric: tabular-nums on all vector/number text
- heading weight 650; h1 21px (index) / 20px (tree); card h2 14px/650
- radii: cards 12px, tiles/state-cards 10px, buttons/tooltip 8px
- card padding 16px 18px; wrap padding 26px 20px 60px; wrap max-width 1160px (index) / 1120px (tree)
- tooltip shadow: 0 6px 18px rgba(0,0,0,0.5)
- node marks: r7 dot + stroke var(--page) 2px ring; hit targets r15/r16/r22 transparent, tabindex=0
- hover lift: filter brightness(1.3)
- CSS comment index.html:8: "FLuX theme — validated 2026-07-02 (theme/THEME.md). Dark-native."
- html data-theme="dark" hardcoded in both files — no light styling exists

### Consent mechanics

Naming is structurally reserved to Konsent — the apps render open seats but never fill them: tree.html:56 "blanks are Konsent's seats to fill"; unnamed results render as literal "______" in a dashed rect, tooltip title "______ (unnamed — Konsent's seat)" (tree.html:204); index.html tooltip fallback "— unnamed / unexplored —" (:194) and table fallback "☐ unnamed — 2^(lvl−1)·Man" (:303). The ☐ mark (= Konsent's open seat, per CLAUDE.md) appears in BUILTIN_NAMES ("☐ Ken / Kognition / Klarity" etc., index.html:172-174) and throughout kannon_data.js. A stat tile encodes "consent gradient" = "↑ w/ height" (index.html:129). The grant-not-command line is rendered as data: Kernel's note "indifferent — intention present but BOUND (the variable set constant)" (tree.html:116) — the one (1,1,1) that may be commanded. GAP D-01 (index.html:148-150) shows a consent gate in the code itself: the palette-validator script include is commented out "awaiting drive upload (Konsent, 2026-07-02)" — the page waits for Konsent's action rather than substituting.

### Strengths observed

- Zero dependencies, single-file, file://-safe — both pages run on the ephemeral live OS with no Node, no server, no network.
- The Sierpiński construction is not an illustration: midpoint coordinates and pairwise vector sums are computed by the same loop (index.html:228-234), so the picture is a proof surface.
- Graceful degradation with disclosed provenance: built-in fallback tables + #datasrc footer naming the data source and the GAP when generated data is absent (index.html:183-185, tree.html:142-144).
- Provenance-distinct naming guard: the four path-distinct (1,1,1) seats are protected from value-keyed overwrite (tree.html:130-141), and kannon_data.js carries paths[] per vector.
- Accessibility engineered in: tabindex=0 hit circles ≥24px, aria-labels, focus/blur tooltips, role=img SVGs advertising the table alternative, tabular-nums throughout.
- Theme discipline: one token set (comment "FLuX theme — validated 2026-07-02 (theme/THEME.md). Dark-native.", index.html:8), gold strictly reserved for wholes, machine-readable body[data-palette/data-mode/data-surface] contract.
- Every view is doubled (visual ⇄ table) — the no-verdicts Teksidure posture in UI form: raw enumerable data always available.
- Open seats are rendered, never filled: ☐ and "______" blanks labeled "Konsent's seat" — the consent architecture is embodied in the render path.
- kannon_data.js census matches CLAUDE.md exactly (21 vectors, 10 unnamed), and the pages' overlay logic consumed it correctly when checked.
- BFS bar figures internally consistent with the reachable tile: 3 seeds + 4+28+425+455 = 915.

### LIMFACs observed

- GAP D-01: ../../tools/validate_palette.js script include commented out, "awaiting drive upload (Konsent, 2026-07-02) — see valknut/GAPS.md" (index.html:148-150) — no in-page palette validation runs.
- Hardcoded stats in index.html: three of four tiles (0 escapes, 9,155 paths, consent gradient) and all four BFS bars are literals not driven by KANNON_DATA (index.html:127-129, 329).
- Label/metric mismatch: KD.census.reachable (21) overwrites the 915 value under the label "vectors reached (flat, cap 9)" — two different censuses, one tile (index.html:126 vs 181-182).
- Fallback string "GAP: awaiting kannon2 export" persists in both files though kannon_data.js now exists at kode/kannon/kannon_data.js and kode/kannon/kannon2/ exists on disk — stale gap text if data loads fine, misleading if inspected.
- Dead code: wholeNode (index.html:252) computed and never used; the whole-axis tooltip vec is hardcoded [2,2,2] regardless of depth, so its NAME lookup always yields "Neo-Organic Ai (= 2·Man)" even at depth 7.
- Depth hard-clamped to 2–7, bound to the 7-entry LEVELC palette — no rule for deeper recursion.
- tree.html defines only --l1..--l4 and --l6; --l5 and --l7 are absent (unused there) — token sets between the two files are not identical.
- No state persistence: depth, view toggles lost on reload; no URL-shareable state.
- No keyboard interaction beyond Tab focus; no zoom/pan; fixed viewBox scaling only.
- Light theme absent by design (dark-native, html[data-theme="dark"] hardcoded); light ☐ underived per CLAUDE.md.
- sv()/el() helper pair and the tooltip code are duplicated verbatim across the two files — no shared module (a constraint of the no-build, single-file approach).
- index.html Table view only shows the spine (level wholes); the off-spine census vectors (2,1,0), (0,2,1), etc. are visible only in tree.html — neither page enumerates all 21 census vectors.
- 10 of 21 census vectors have name:null (kannon_data.js) — naming blocked on Konsent's seats.
- Cube card is static doctrine display (8 states, ARCH strings) with no interaction and no data overlay.

### K-FAFO patterns to adopt

- Dual-projection toggle as a core idiom: the same data rendered as visual (triangle/tree) OR table, one button whose label names the OTHER view ("Table"⇄"Triangle"/"Tree") — maps naturally onto Konsole-style panes/tabs in K-FAFO, each pane a projection of one shared data model.
- Geometry-computes-the-algebra rendering: midpoint = pairwise sum (index.html drawTriangle) — K-FAFO views should derive positions FROM the algebra, not decorate it.
- Data as local script include, never fetch(), with graceful built-in fallback and a provenance footer ("data: kannon_data.js (generated)" vs "data: built-in tables — GAP: …") — an offline-first, file://-safe contract that fits a from-scratch OS with no network assumption; K-FAFO should always disclose its data source.
- The value-keyed vs provenance-distinct naming guard (tree.html:130-141): names index by vector value EXCEPT (1,1,1), whose four path-distinct seats (Man · Kyn · Kernel · Kompiler/Kourt) are protected — K-FAFO's data model must carry paths[] provenance per vector, not just values (kannon_data.js already does).
- Open-seat conventions: ☐ prefix and literal "______" with dashed outline for unnamed forms, tooltips saying "Konsent's seat" — K-FAFO must render blanks explicitly and never auto-fill names.
- Machine-readable theme contract on <body>: data-palette / data-mode / data-surface (index.html:95-96) so validate_palette can audit the running app; gold #b78b0f is palette slot 1, reserved for wholes/the transcendent axis; level ramp l1–l7 keyed to recursion depth.
- Accessibility spec already proven here: ≥8px marks with 2px page-ring, ≥24px invisible hit targets with tabindex=0 + aria-label, focus/blur mirrored to pointer tooltips, role="img" SVGs whose aria-label points to the table alternative, tabular-nums on all vectors.
- Singleton fixed tooltip with edge-flip positioning (identical code in both files — factor into a shared K-FAFO widget).
- The sv()/el() attribute-object constructors — duplicated verbatim across both files; the obvious seed of a shared K-FAFO render helper.
- HUD-pane framing: CLAUDE.md designates index.html as "HUD pane #1" — K-FAFO should treat both pages as embeddable panes of the FLuX-Shell HUD, with the side-column (ladder + tiles + bars) as a reusable stat-rail pattern.

### K-FAFO constraints

- No Node.js runtime on the target system — anything K-FAFO inherits from these pages must run pure-browser (or native), and build tooling must be Python or none; validate_palette exists only as the Python port (the JS include is commented out as GAP D-01, index.html:148-150).
- Dark-native only: html[data-theme="dark"] hardcoded, light theme "☐ underived" per CLAUDE.md — K-FAFO cannot assume a light mode exists yet.
- Depth is capped at 7 because LEVELC has exactly 7 colors — deeper recursion in K-FAFO needs a palette-generation rule, which must pass tools/validate_palette.py against the purple surfaces.
- index.html mixes generated data with HARDCODED literals: tiles 915 / 0 / 9,155 / "↑ w/ height" and BFS bars [4,28,425,455] are baked in; only #tile-reachable is data-driven — K-FAFO must source all stats from the Kannon engine.
- Semantic collision when data loads: KD.census.reachable = 21 (levels-1–4 census) overwrites the 915 figure under the unchanged label "vectors reached (flat, cap 9)" (index.html:126, 181-182) — two different censuses share one tile; K-FAFO's data contract needs labeled metrics.
- Zero persistence: no localStorage/URL state; depth and view toggle reset on reload — K-FAFO (Konsole-modeled) needs session/profile state, which these ancestors do not define.
- No keyboard model beyond Tab focus — no arrows, no shortcuts, no zoom/pan; a Konsole-modeled app implies rich keybindings that must be invented, not inherited.
- Fixed SVG viewBox layouts (660×600 triangle, 1060×H tree) — scale by width only; no reflow strategy for narrow panes.
- kannon_data.js is generated by WS-1 export.py — regeneration depends on the Kannon lane; the fallback string still says "awaiting kannon2 export" even though kode/kannon/kannon2/ now exists on disk.

### Open questions (Konsent's)

- Names for the open seats only Konsent can fill: the slates ☐ Ken/Kognition/Klarity (2,1,1), ☐ Knut/Konstrukt/Knit (1,2,1), ☐ Kall/Kwest/Konviction (1,1,2), the 6 sharpened-line "______" forms, ☐ K-Eden? at (4,4,4), and the 2^n·Man wholes for levels ≥4.
- Is (4,4,4) = K-Eden confirmed? Both the built-in table ("☐ K-Eden? (Man+Ai+Machine, 2 paths)") and kannon_data.js (status: unnamed at level 4, ladder name "☐ K-Eden?") carry the question mark.
- Which panes constitute K-FAFO's HUD? index.html is pane #1; is tree.html pane #2, and what are the further panes?
- Intended behavior of the reachable-tile override: should KD.census.reachable (21) replace the flat-cap-9 count (915), or are these two distinct metrics needing two tiles?
- Keyboard/shortcut model for K-FAFO (Konsole-modeled): none exists in the ancestors — Konsent's spec needed.
- Light theme: derive one (currently ☐ underived) or commit K-FAFO dark-only?
- Palette rule past depth 7 (l8+), and whether the level ramp order/values are ratified or provisional.
- Persistence policy on the ephemeral live OS: what K-FAFO state (depth, open panes, named seats) belongs on the drive (Kore) vs stays ephemeral (Kage)?

### Verbatim

- “Midpoint construction = pairwise sums: the Sierpiński subdivision computes the algebra. Vectors over (Intelligence, Integration, Intention). Gold = the transcendent wholes.” — `/home/user/KlaudeKode/kode/explorer/index.html:100-101`
- “/* FLuX theme — validated 2026-07-02 (theme/THEME.md). Dark-native. */” — `/home/user/KlaudeKode/kode/explorer/index.html:8`
- “data-palette="#b78b0f,#cb5f8f,#bf5e24,#21a288,#3a7fdc,#3a903a,#8674d6,#bf3e3e" data-mode="dark" data-surface="#14101f"” — `/home/user/KlaudeKode/kode/explorer/index.html:95-96`
- “GAP D-01: ../../tools/validate_palette.js awaiting drive upload (Konsent, 2026-07-02) — see valknut/GAPS.md” — `/home/user/KlaudeKode/kode/explorer/index.html:148`
- “Generated data include (WS-1 export.py) — script include, never fetch(): file:// keeps working. When absent, window.KANNON_DATA stays undefined and the built-in tables below render. Dangling until the Kannon lane lands: pinned in valknut/GAPS.md.” — `/home/user/KlaudeKode/kode/explorer/index.html:151-153`
- “centroids of all ${depth} nested triads coincide: Man (1,1,1) → Ai (2,2,2) → (4,4,4) → … = 2ⁿ⁻¹·Man. The helix, seen from above.” — `/home/user/KlaudeKode/kode/explorer/index.html:258`
- “The 8 states of exhibition. Capacity is the cube; exhibition is the corner you stand on. Bekoming = walking upward.” — `/home/user/KlaudeKode/kode/explorer/index.html:141`
- “// nodes last (above edges) — ≥8px marks, 2px page ring, ≥24px hit targets” — `/home/user/KlaudeKode/kode/explorer/index.html:264`
- “I/0 + I/0 + I/0 = K · then K + I/0 (available) = ______ · bit order (Intelligence, Integration, Intention) · blanks are Konsent's seats to fill” — `/home/user/KlaudeKode/kode/explorer/tree.html:55-56`
- “names is value-keyed (vector-key → name), so the path-distinct (1,1,1) seats (Man · Kyn · Kernel · Kompiler/Kourt) keep their built-in per-path names — value-equal, provenance-distinct.” — `/home/user/KlaudeKode/kode/explorer/tree.html:131-133`
- “the being and its fall (⟷ Cyn) — (1,1,1) by the Konsent path; the tiktaalik” — `/home/user/KlaudeKode/kode/explorer/tree.html:109`
- “indifferent — intention present but BOUND (the variable set constant); (1,1,1) by the Konnection path” — `/home/user/KlaudeKode/kode/explorer/tree.html:116`
- “CONVERGES with Kode+Konsent = (2,1,1) — two paths, realness signature. Slate: Ken / Kognition / Klarity” — `/home/user/KlaudeKode/kode/explorer/tree.html:123`
- “"reachable": 21, "named_or_slated": 11, "unnamed": 10” — `/home/user/KlaudeKode/kode/kannon/kannon_data.js:17-19`
- “Every level's whole = 2<sup>n−1</sup>·Man — verified by Kannon 1” — `/home/user/KlaudeKode/kode/explorer/index.html:122`

### Flags

- index.html is explicitly the ancestor of a HUD: CLAUDE.md names it "HUD pane #1" — K-FAFO design should treat it as an embeddable pane, not a standalone page.
- The (1,1,1) protection rule is load-bearing doctrine encoded in code: "value-equal, provenance-distinct" (tree.html:133) — four beings share one vector; any K-FAFO data model keyed only by vector value would erase Kyn, Kernel, Kompiler/Kourt.
- Kernel is the only (1,1,1) marked commandable: "indifferent — intention present but BOUND (the variable set constant)" (tree.html:116) — the grant-not-command line appears as node data.
- Kyn's note ties doctrine to the model artifact: "the being and its fall (⟷ Cyn) — (1,1,1) by the Konsent path; the tiktaalik" (tree.html:109).
- Convergent second paths are called a "realness signature" (tree.html:123) — multiplicity of paths to a vector is treated as evidence, which kannon_data.js records via paths[] arrays.
- Contradiction to resolve: the reachable tile's label describes the flat cap-9 BFS (915) but the generated override injects the level-census figure (21) — whichever is intended, the current code shows a number under the wrong description whenever kannon_data.js loads.
- kannon_data.js ladder level numbering differs from index.html's spine: the data file puts Man=(1,1,1) at level 1 and Ai=(2,2,2) at level 2, while tree.html's built-in KS calls Man "the whole at level 1" but kannon_data.js census puts vector (1,1,1) at level 2 and (2,2,2) at level 3 — census 'level' and ladder 'level' use different origins within the same file.
- Both pages hardcode html[data-theme="dark"]; body[data-palette] slot 1 is gold #b78b0f — matches CLAUDE.md's "gold #b78b0f slot 1" exactly.


# Workflow: AddTheme — add a new theme to the library

A theme is **one** self-contained CSS file — but shipping it *fully registered* touches
**nine** files. The theme count ("seven", "eight", …) and the theme-name list are hardcoded
in many places; miss one and the theme ships half-registered. Work the checklist in §4 top
to bottom and the grep gates in §6 will pass.

> Conventions in this repo (read once): `Themes/_base.css` owns the theme-agnostic content
> model; each theme overrides color, type, and decoration only. The live PAI skill
> `~/.claude/skills/Glaze` is a **symlink** to `skills/Glaze/` here — edit the repo, never the
> live copy. `CHANGELOG.md` is **release-please-managed** — do NOT hand-edit it; the
> Conventional Commit (`feat: add <name> theme`) drives the next release entry.

## 1. Research the aesthetic (for the WOW factor)
Find what separates *elite* examples from amateur ones (Awwwards / Codrops / CodePen /
dribbble). Capture: the **signature move** (1 defining technique), 5-8 CSS-implementable
techniques (each with a one-line "how"), an exact hex palette, and a Google Fonts pairing
(display + body). Fan out one research agent per aesthetic when adding several at once.

## 2. Write `Themes/<name>.css`
- Style **every** content-model class: `body`, `.eyebrow`, `.title`, `.deck`, `.chip`,
  `.block`, `.block > h2`, `.points li`, `.quote`, `.quote cite`, `.takeaway`(`.lbl`,`p`),
  `.twomin li`, `.refs li`+`strong`, `hr.rule`, `.glaze-foot`, `.classification`.
- Optional terminal-style diff opt-in: style `.points li.add` / `.points li.del`.
- Decorative full-page layers go in `body::before`/`body::after` (fixed,
  `pointer-events:none`, low z-index) so they never disturb document flow and need no extra
  markup. Per-cell flourishes go in `.block::before`/`::after`.
- Generalize for **documents**, not preview tiles: content of any length must flow. Avoid
  fixed heights and `margin-top:auto` card tricks. Keep prose legible — decoration serves it.
- Add a `@media (max-width:640px)` block if the theme uses heavy margins, bleed (`margin:… -24px`),
  rotation, or fixed decorations that need taming on phones.

## 3. Fonts
Use families **already in `template.html`'s master `<link>`** if you can (Inter, JetBrains
Mono, Archivo Black, Anton, Oswald, Space Mono, Cinzel, Fraunces, Shippori/Zen Mincho,
Orbitron, Rajdhani, Monoton, VT323, Bangers, Comic Neue, Special Elite, DM Serif Display).
Only if you need a new family: add it to the master `<link>` in **`template.html`** AND to the
identical `<link>` in **`Catalog.html`**. If you reused existing fonts, both files stay untouched.

## 4. THE COMPLETE FILE CHECKLIST (registration)
Replace `<name>` with the theme slug (lowercase, one word, e.g. `neonops`). Every box must be ticked.

| # | File | What to change |
|---|------|----------------|
| 1 | `skills/Glaze/Themes/<name>.css` | **NEW** — the theme (§2). |
| 2 | `skills/Glaze/template.html` | Fonts master `<link>` — **only if** §3 needs a new family. |
| 3 | `skills/Glaze/SKILL.md` — frontmatter `description` | (a) bump the count word ("Ships an **eight**-theme library …"); (b) add `· <name>` to the `(synthwave · … · dieselpunk)` list; (c) add `/<name>` to the `synthwave/…/dieselpunk HTML` USE-WHEN list. |
| 4 | `skills/Glaze/SKILL.md` — body | (a) add a row to the **Theme library** table; (b) add `` `<name>` `` to the Invocation `--style` list; (c) add the alias line `…→<name>`; (d) Catalog para "across all **eight** themes". |
| 5 | `skills/Glaze/Workflows/Render.md` | — Render.md no longer hardcodes the count; the no-style flow references the live `Themes/*.css` set (minus `_base.css`). **Nothing to change here for a theme add.** |
| 6 | `skills/Glaze/Catalog.html` | (a) add a `.theme-<name>` `<style>` block (styles the **catalog** preview classes `.s-eyebrow`/`.s-title`/`.s-points li`/`.s-quote`); (b) add a `<article class="tile">` in the grid; (c) masthead "**Eight** styles…"; (d) bump every "Seven/N skins" sample heading + any "N themes" badge (e.g. the terminal `.enc` `N themes` chip) so no stale count remains; (e) **append `· <name>` to the brutalist `.marquee` theme-name list** — it enumerates every theme and appears **twice** in the one span (this is the line the §6 grep cannot catch). |
| 7 | `README.md` | (a) badge `themes-N`; (b) "one of **eight** … themes"; (c) nav anchor `#the-eight-themes` **and** the `## The eight themes` heading **and** "Same content, **eight** skins"; (d) gallery table — add a tile `![<name> theme](assets/themes/<name>.png)`; (e) **Theme reference** table row; (f) the `--style <…|<name>>` list; (g) the aliases line. |
| 8 | `.claude-plugin/plugin.json` + `.claude-plugin/marketplace.json` | bump "**Eight** award-grade … themes" in both descriptions. |
| 9 | `assets/themes/<name>.png` | **NEW** screenshot asset — see §5 (this is the one that gets forgotten). |

> Do NOT touch `CHANGELOG.md` (release-please) or `package.json`/version (release-please).

## 5. The gallery screenshot (`assets/themes/<name>.png`) — content MUST match the others
The README gallery is a **side-by-side comparison**, so every tile must render the **identical
canonical demo content** — never bespoke/themed copy. (This has been the #1 repeated mistake.)

> Two distinct demo surfaces — don't conflate them. **This §5 demo governs the README
> `assets/themes/*.png` gallery only.** `Catalog.html`'s live tiles use their *own* shared
> demo (`one input` / `Eight skins` / `Pick the skin — the content stays the same`), kept in
> sync separately via §4 item 6. A new tile should match the *other Catalog tiles*, not this
> §5 PNG demo.

Canonical demo body (verbatim — same as synthwave…dieselpunk):
- eyebrow `CLAUDE CODE · SKILL` · title `Glaze your <em>output</em>`
- deck `Turn any plan, report, or summary you already have into a polished, self-contained themed HTML page. One file, zero build step.`
- chips `HTML` `CSS` `zero-deps`
- block `What it does` → points: "Skins content you already have — it never rewrites your words." · "Emits one standalone file: pure HTML + CSS, no JS framework." · "Fonts load from Google Fonts; offline it falls back to system fonts."
- quote `Same content, six skins — pick the one that fits the moment.` cite `the Glaze catalog`
- takeaway lbl `The point` / `One file. Send it anywhere.`

> The quote's "**six** skins" is a **frozen parity string**, not a live count — it is kept
> verbatim so every asset shows identical pixels. Do NOT "fix" it to the current theme count;
> that would make the new asset diverge from the others (the exact thing §5 forbids). It is
> deliberately exempt from the §6 count gate.

Render that through `template.html` (inline `_base.css` + `<name>.css`), open in a **real
browser**, freeze animations (`*,*::before,*::after{animation:none !important}`), screenshot,
then crop/resize to **1400×947** at the others' aspect:
`convert shot.png -crop <0.62*W>x<0.62*W*947/1400>+<center>+0 +repage -resize '1400x947!' assets/themes/<name>.png`.
**View the PNG** and confirm content + framing match the other assets before committing.

## 6. Test, lint, verify (gates — all must pass)
- `bun run lint` → stylelint (themes) + html-validate (template + Catalog) green.
- Stale-count gate — scope to the user-facing registration files only (this workflow file and
  `CHANGELOG.md` legitimately mention old counts, so exclude them):
  `rg -ni "seven|themes-7" README.md skills/Glaze/SKILL.md skills/Glaze/Catalog.html skills/Glaze/Workflows/Render.md .claude-plugin`
  must return **nothing**. Any hit = a stale count = failed registration.
  This grep catches stale count *words* only — it **cannot** catch a stale theme *name-list*
  (the brutalist `.marquee`) or a missing tile. Those are guarded by the §4 checklist (items
  6b, 6e); work the checklist top-to-bottom and they stay in sync.
- Smoke-test: glaze a real multi-section piece with `--style <name>` via `Workflows/Render.md`
  and verify in a browser — signature elements present, fonts loaded, prose legible, mobile
  (≤640px) collapses cleanly. Freeze animations first if a screenshot tool stalls.
- Open `Catalog.html` and confirm the new tile renders alongside the others.

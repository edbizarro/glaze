---
name: Glaze
description: >-
  Turn any output you already have — a plan, a report, a summary, release notes,
  any text — into a beautiful, self-contained themed HTML page. Glaze *skins*
  content; it does not generate it. Ships an eight-theme library (synthwave · manga ·
  brutalist · terminal · sumi · coffee · dieselpunk · neonops), each a self-contained CSS preset built
  from award-winning design techniques. Zero build step, no JS framework, fonts
  via Google Fonts, all CSS inlined so the file travels as a single standalone
  artifact. Extensible: a new theme is one CSS file. Output is verified by opening
  it in a browser. USE WHEN glaze, glaze this, render as HTML, themed HTML, style
  this output, make this pretty, beautify this plan/report/summary, skin this
  content, apply a theme, turn this into a styled page, --style,
  synthwave/manga/brutalist/terminal/sumi/coffee/dieselpunk/neonops HTML, show the theme catalog. NOT
  FOR generating the content itself (produce the content first, then glaze the
  result), static illustrations or diagrams, or video.
---

# Glaze — content → themed HTML artifact

Glaze takes output you already have and renders it as a polished, standalone HTML
page in a chosen visual theme. It does **not** invent the content — it *skins* it.
The input is anything textual: a plan, a report, a summary, an extraction, release
notes, a thread recap. If the content still needs to be produced (say, a wisdom
extraction or a summary), produce it first, then glaze the result.

The output is self-contained and dependency-free: pure HTML and CSS, fonts via
Google Fonts, no build step and no JS framework. It works in any Claude Code setup.

## Invocation

```
glaze <content-or-reference> --style <theme>
```

- `<content>` — pasted text, a file path, an already-extracted body, the previous
  message, or a named artifact. If it's ambiguous, ask what to glaze.
- `--style <theme>` — one of: `synthwave` `manga` `brutalist` `terminal` `sumi`
  `coffee` `dieselpunk` `neonops`. Aliases: `vaporwave`→synthwave, `anime`→manga,
  `neo`/`neobrutalist`→brutalist, `crt`/`tui`→terminal,
  `wabi`/`wabisabi`/`sumie`→sumi, `cafe`→coffee,
  `diesel`/`steampunk`/`brass`→dieselpunk,
  `neon`/`ops`/`dataops`/`hud`/`wipeout`→neonops.
- No `--style` given → **ask which theme** (offer the eight; `terminal` is a safe
  default for technical content). Never silently pick.
- `--style random` → choose one at random and say which.

## Theme library

| Theme | Vibe | Signature move | Best for |
|-------|------|----------------|----------|
| `synthwave` | neon outrun 80s | receding 3D grid + chromatic aberration + scanlines | launches, retros, high-energy |
| `manga` | comic/anime page | diagonal ink panels + halftone + SFX | recaps, threads, fun |
| `brutalist` | neo-brutalist | hard offset shadows + clashing blocks + marquee | raw, honest, serious extractions |
| `terminal` | CRT/TUI Tokyo Night | CRT screen layer + vim statusbar + diff lines | technical content (safe default) |
| `sumi` | sumi-e / wabi-sabi | 間 ma + ensō + tate-gaki + hanko seal | calm, reflective, sophisticated |
| `coffee` | specialty roaster label | tasting-notes headline + roast meter + kraft grain | personal, blog, editorial |
| `dieselpunk` | interwar brass & oiled steel | riveted brass plates + engraved title + amber power-bar + gauges + phosphor stamp | bold industrial, ops, retro-futurist |
| `neonops` | dark ops-HUD (Designers-Republic / Wipeout) | corner-bracketed cells + dot-matrix + marching hazard stripe + scanlines + magenta signal + blinking sys cursor | dashboards, ops, status reports, data |

Each theme lives at `Themes/<theme>.css` and is fully self-contained.
`Themes/_base.css` owns the theme-agnostic content model (layout, flow,
responsiveness, print).

## Content model (what every theme styles)

The canonical structure is in `template.html`. Keep markup to these classes so all
themes render identically:

- `.glaze` wrapper → `.glaze-head` ( `.eyebrow`, `.title`, `.deck`, optional `.meta`>`.chip` )
- `main` → repeated `.block` ( `<h2>`, `.points`>`li`, optional repeated `.quote`>`cite` )
- closing (optional): `.takeaway`(`.lbl`,`p`) · `.block.twomin`>`ul` · `.block.refs`>`ul`>`li`>`strong`
- `.glaze-foot` → `.classification`
- `hr.rule` between major regions
- Terminal-only opt-in: add class `add`/`del` to a `.points li` for green-+/red-− diff lines.

Decorative full-page layers (grid, CRT, washi, ensō, grain) are injected by the
theme via `body::before/::after` and never require extra markup.

## Workflow routing

| Trigger | Workflow |
|---------|----------|
| glaze content / render as themed HTML / `--style …` | `Workflows/Render.md` |
| add a new theme / "create a theme for X" | `Workflows/AddTheme.md` |
| show the catalog / list the themes / compare themes | open `Catalog.html` (see below) |

## Catalog

`Catalog.html` is a self-contained gallery that renders one sample across all eight
themes side by side — the visual menu. When someone wants to see or compare the
themes, open `Catalog.html` in a browser (or send the file). Keep it in sync when
themes are added or changed (see `Workflows/AddTheme.md`).

The Render pipeline applies each theme's **core aesthetic automatically** to the
content model — synthwave's grid and sun, terminal's CRT layer plus vim statusbar
and title cursor, manga's ink panels and halftone, brutalist's hard shadows and
striped rules, sumi's washi and ensō and tate-gaki, coffee's kraft grain and serif,
dieselpunk's riveted brass plates and amber power-bar,
and neonops's corner-bracketed cells, marching hazard stripe, and scanlines.
A few catalog flourishes are **hand-built showcase decorations** (the brutalist
marquee, the manga ドン! SFX, the coffee roast meter), shown to sell the vibe — they
are not emitted by `template.html`. To get coffee flavor pills or synthwave tags in
real output, populate the optional `.meta`>`.chip` row (themes style it). The
artifact a user receives shares the theme's identity, just without the one-off tile
gimmicks.

## Output & conventions

- Write to the path the user gives, or a `reports/` folder in the working
  directory. Suggested name: `glaze-<slug>-<theme>-YYYY-MM-DD.html`.
- Content language follows the source. Match the source's date format.
- The footer `.classification` is optional — set it only when the content carries a
  sensitivity label, otherwise drop it.
- If your environment provides a per-skill preferences file, honor it (preferred
  language, default theme, classification label, footer signature, output dir) —
  but keep this skill body generic and dependency-free.

## Verification (before declaring done)

Always verify the rendered artifact; don't just trust the markup. Open the file in
a browser. If you have browser automation (headless Chrome, a screenshot tool,
etc.), confirm the theme's signature elements rendered, the fonts loaded, and the
prose is legible, then capture a screenshot. Check that mobile (≤640px) collapses
cleanly.

Note: a screenshot tool may stall on pages with continuous animations (the
synthwave grid, terminal flicker, brutalist marquee). Freeze them first by
injecting `*,*::before,*::after{animation:none !important}`, then capture.

## Gotchas

- **Escape source text — it's data, not markup.** When filling placeholders,
  replace `&`→`&amp;`, `<`→`&lt;`, `>`→`&gt;` in every value from the source.
  Skipping this both breaks fidelity (`if a < b`, `<placeholder>`, `2>&1`, code
  snippets render wrong or vanish) and lets untrusted source content (a glazed
  summary of an external page, third-party notes) inject live HTML/script into a
  file built to travel. The template ships `Content-Security-Policy: script-src
  'none'` as a second layer that blocks any script that slips through — keep that
  meta tag. The only markup you add is the theme hooks you control (`<em>` in the
  title, `add`/`del` classes); never tags carried from the source.
- **`document.fonts.check(...)` is unreliable** — it returns `false` for webfonts
  that are actually loaded (Google Fonts splits families into unicode-range
  subsets). Verify the load by inspecting
  `[...document.fonts].filter(f=>f.status==='loaded')` for the family and weight you
  use, or just trust the screenshot.
- **@import ordering** — don't put `@import` inside the inlined `<style>` after
  other rules; it's ignored. The template avoids this by linking all fonts through a
  single `<link>` in `<head>`. Keep it that way.
- **Self-contained output** — inline `_base.css` plus the chosen theme CSS into the
  `<style>` block. Don't link the theme files by relative path; the artifact is
  meant to travel (sent as a file, opened anywhere).
- **Don't over-skin readability away** — sumi and coffee are editorial and calm;
  keep body text legible. The synthwave and terminal glow must not drown the prose.
- **Content fidelity** — Glaze restyles, it never rewrites. Preserve the source
  wording, especially verbatim quotes.
- **"Self-contained" ≠ offline.** All CSS is inlined, but fonts load from the Google
  Fonts CDN through the `<head>` `<link>`. With no network the page still renders —
  it just falls back to system fonts. That's fine for travel and sending; note it if
  a truly offline artifact is required (then self-host the fonts or accept the
  fallbacks).
- **`<hr class="rule">` is a void element** — never style it to hold text. Themes
  render it as a visual divider only (bars, brush strokes, gradients).

## Examples

**Example 1: glaze an extraction or summary**
```
User: "summarize this video ... then make an HTML in synthwave style"
→ the summary/extraction is produced first (a separate step)
→ Glaze (Render): maps it to the content model, inlines _base + synthwave CSS
→ writes reports/glaze-<slug>-synthwave-YYYY-MM-DD.html, verifies in a browser
```

**Example 2: theme not specified**
```
User: "glaze this summary"
→ Render: the content is clear, but there's no --style
→ ask which of the eight themes; never silently pick
→ assemble + verify + deliver
```

**Example 3: re-skin existing content in another theme**
```
User: "send the same thing in the coffee theme"
→ reuse the same content model, swap in Themes/coffee.css
→ new file glaze-<slug>-coffee-YYYY-MM-DD.html, verify, deliver
```

**Example 4: add a new theme**
```
User: "create a cyberpunk theme for Glaze"
→ AddTheme: research elite cyberpunk technique → write Themes/cyberpunk.css
→ register in the theme table + the --style list → smoke-test render
```

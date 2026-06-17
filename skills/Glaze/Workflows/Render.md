# Workflow: Render — content → themed HTML

Goal: turn given content into a self-contained themed HTML artifact and verify it.

## 1. Resolve inputs
- **Content.** Identify what to glaze: pasted text, a file (Read it), a prior
  message, or an already-extracted body. If unclear, ask. Glaze does NOT generate
  the content — if it must be produced first (a wisdom extraction, a summary),
  produce it first, then glaze the result.
- **Theme.** Read `--style`. Resolve aliases (see SKILL.md). If none given, ask
  which of the eight (`terminal` is a safe default). `random` → pick one, announce it.
- **Language / classification.** Content language follows the source. Set the
  `.classification` footer only when the content carries a sensitivity label.
  If your environment exposes a per-skill preferences file, honor it.

## 2. Map content → the canonical model
Fit the source into `template.html`'s structure:
- Header: `.eyebrow` (kicker), `.title` (headline), `.deck` (1-line description).
- One `.block` per section: an `<h2>`, a `.points` list (the substance), and any
  number of `.quote` blocks (verbatim quotes — preserve wording exactly).
- Optional closers when the source supports them: `.takeaway` (one line),
  `.block.twomin` (the must-knows), `.block.refs` (people/links/tools).
- Keep markup strictly to the content-model classes (SKILL.md). Add no inline
  styles. Optional, theme-aware hooks:
  - wrap one word of `.title` in `<em>` for emphasis (brutalist boxes it; others
    inherit gracefully);
  - add a `.meta` row of `.chip` spans for tags — these render as flavor pills in
    coffee, neon chips in synthwave, `#tag` in terminal, etc.;
  - terminal-only: tag a `.points li` with `add`/`del` for green-+/red-− diff lines.

## 3. Assemble the file
1. Read `Themes/_base.css` and `Themes/<theme>.css`.
2. Take `template.html`; replace `{{GLAZE_STYLE}}` with `_base.css` contents
   followed by the theme CSS (base first, theme second so theme wins).
3. Fill the content placeholders (`{{TITLE}}`, `{{EYEBROW}}`, sections, etc.).
   Repeat `.block`/`li`/`.quote` as needed; delete optional regions the source
   doesn't justify (e.g. no refs → remove the refs block and its `hr`).
   **Escape the source text as you place it.** Source content is data, not
   markup: replace `&`→`&amp;`, `<`→`&lt;`, `>`→`&gt;` in every value you drop
   into the template. This keeps fidelity (a code snippet, `if a < b`, a
   `<placeholder>`, or `2>&1` renders literally instead of breaking the page or
   vanishing) and stops untrusted content (a glazed summary of an external page,
   third-party release notes) from injecting live HTML into a file that travels.
   The only markup you add is the intentional theme hooks you control — the
   `<em>` around a title word, the `add`/`del` class on a `.points li` — never
   tags carried over from the source.
4. Keep the single `<link>` font tag in `<head>` (covers all themes). Do not add
   `@import` inside `<style>`.
5. Write to the project's `reports/glaze-<slug>-<theme>-YYYY-MM-DD.html`
   (or the path the user gave).

## 4. Verify in a browser
Open the file and confirm it actually rendered — don't just trust the markup.
If you have browser automation (headless Chrome, a screenshot tool, devtools
eval, etc.), check that:
- the expected number of `.block` sections and `.quote`s are present;
- the theme's signature elements rendered (grid, CRT layer, panels, ensō, etc.);
- the webfonts loaded — inspect the loaded font set rather than relying on
  `document.fonts.check(...)`, which gives false negatives for subsetted fonts;
- the prose is legible and mobile (≤640px) collapses cleanly;
then capture a screenshot. If the screenshot tool stalls on animations, freeze
them first by injecting `*,*::before,*::after{animation:none !important}`.

## 5. Deliver
- Hand over the HTML file (and optionally a PNG preview). Remove any loose
  screenshot files afterward.
- Report the path and which theme; offer to re-glaze in another theme.

# Workflow: AddTheme — add a new theme to the library

Adding a theme is one self-contained CSS file. No other file needs to change
(the theme list in SKILL.md is documentation; update it too).

## 1. Research the aesthetic (for the WOW factor)
Before writing CSS, find what separates *elite* examples of the aesthetic from
amateur ones. Search Awwwards / Codrops / CodePen / dribbble. Capture:
- **The signature move** — the single defining technique (1-2 sentences).
- 5-8 concrete, CSS-implementable techniques (each with a one-line "how").
- An exact hex palette and a Google Fonts pairing (display + body).
For several themes at once, fan out one research agent per aesthetic in parallel.

## 2. Write `Themes/<name>.css`
- Style **every** content-model class (see SKILL.md): `body`, `.eyebrow`,
  `.title`, `.deck`, `.chip`, `.block`, `.block > h2`, `.points li`, `.quote`,
  `.quote cite`, `.takeaway`(`.lbl`,`p`), `.twomin li`, `.refs li`+`strong`,
  `hr.rule`, `.glaze-foot`, `.classification`.
- Put decorative full-page layers in `body::before`/`body::after` (fixed,
  `pointer-events:none`, low z-index) so they never disturb document flow and
  don't require extra markup.
- Generalize for **documents**, not preview tiles: content of any length must
  flow. Avoid fixed heights and `margin-top:auto` tricks meant for cards.
- Keep prose legible — decoration serves the content.
- Add the theme's Google Fonts families to the master `<link>` in `template.html`
  (and the inline copy used by Render) if not already present.
- Add a `@media (max-width:640px)` block if the theme uses heavy margins,
  rotation, or fixed decorations that need taming on phones.

## 3. Register & test
- Add a row to the theme table in `SKILL.md` and to the `--style` list +
  aliases.
- Update `Catalog.html`: add a tile for the new theme so it shows in the gallery.
- Smoke-test: glaze a real multi-section piece of content with the new theme via
  `Workflows/Render.md` and verify in a browser — signature elements present,
  fonts loaded, prose legible, mobile collapses cleanly.
- If a screenshot tool stalls on animations, freeze them first (see Render.md step 4).

# Changelog

All notable changes to Glaze are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] — 2026-06-16

First public release.

### Added
- Six self-contained themes: `synthwave`, `manga`, `brutalist`, `terminal`,
  `sumi`, and `coffee`. Each is a single CSS file built from award-winning
  design techniques.
- A theme-agnostic content model (`Themes/_base.css` + `template.html`) so every
  theme renders the same markup identically.
- `Catalog.html`, a standalone gallery that shows one sample across all six themes
  side by side.
- Two workflows: `Render` (content → themed HTML) and `AddTheme` (add a theme in
  one CSS file).
- Packaged as an installable Claude Code plugin (`plugin.json` + `marketplace.json`).

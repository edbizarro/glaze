# Changelog

All notable changes to Glaze are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0](https://github.com/edbizarro/glaze/compare/v1.2.0...v1.3.0) (2026-06-18)


### Features

* **glaze:** random theme by default when no --style, with first-run preference prompt ([#4](https://github.com/edbizarro/glaze/issues/4)) ([bb6608b](https://github.com/edbizarro/glaze/commit/bb6608bf7571a4a5c8623949f7f24b8267385c16))

## [1.2.0](https://github.com/edbizarro/glaze/compare/v1.1.1...v1.2.0) (2026-06-17)


### Features

* add neonops theme + exhaustive AddTheme checklist ([#2](https://github.com/edbizarro/glaze/issues/2)) ([ed8cb6d](https://github.com/edbizarro/glaze/commit/ed8cb6d67767aed908ba1a5cae0465da6f2491d3))

## [1.1.1](https://github.com/edbizarro/glaze/compare/v1.1.0...v1.1.1) (2026-06-17)


### Bug Fixes

* **render:** escape source content + add script-src CSP to artifacts ([911d543](https://github.com/edbizarro/glaze/commit/911d543af1ca1fe7f2a193ede99e970b8db77328))

## [1.1.0] — 2026-06-17

### Added
- `dieselpunk` theme — interwar brass and oiled steel, the seventh theme.
- Lint tooling (stylelint + html-validate) and a PR validation workflow
  (`claude plugin validate`, JSON-Schema manifest checks, lint suite).

### Changed
- Generic `font-family` fallbacks across all themes, so a page still renders
  legibly when Google Fonts is unavailable.
- README reworked around README best practices (hook, badges, table of
  contents, a `For coding agents` section) and the catalog screenshot replaced
  with a per-theme gallery.

### Fixed
- Corrected the marketplace manifest `$schema` URL.

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

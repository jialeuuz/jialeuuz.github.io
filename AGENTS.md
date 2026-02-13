# Repository Guidelines

## Project Structure

This repository is a Quarto website (GitHub Pages).

- `*.qmd`: Source pages (e.g. `index.qmd`, `publications.qmd`, `cv.qmd`).
- `_quarto.yml`: Site configuration (navbar, theme, output directory).
- `styles.scss`, `custom.css`: Site styling overrides.
- `images/`: Author-controlled image assets (favicon, profile, etc.).
- `docs/`: Rendered static site output (HTML, `site_libs/`, PDFs). This is what GitHub Pages serves.
- `_extensions/`, `webr/`: Quarto extensions and webR-related assets.

## Build, Test, and Development Commands

Quarto CLI is required.

- `quarto preview`: Run a local dev server with live reload.
- `quarto render`: Build the full site into `docs/`.
- `quarto render cv.qmd`: Rebuild a single page (faster for edits).

There is no dedicated automated test suite in this repo; treat a clean `quarto render` plus a quick browser smoke-check as the main validation step.

## Coding Style & Naming Conventions

- Prefer editing source files, not generated output: change `*.qmd` / `_quarto.yml` / `styles.scss` and re-run `quarto render` instead of hand-editing `docs/*.html`.
- YAML in `_quarto.yml`: 2-space indentation, keep keys grouped under `project:`, `website:`, `format:`.
- Filenames: lowercase, descriptive (`publications.qmd`, `interests.qmd`), and keep page `href:` values in `_quarto.yml` in sync.

## Commit & Pull Request Guidelines

Recent history uses very short, lowercase subjects (e.g. `update`, `init`). When contributing, prefer more descriptive, imperative subjects like:

- `Update publications list`
- `Tweak navbar links`
- `Fix CV PDF build`

For PRs:

- Describe the change and which page(s) it affects.
- Include before/after screenshots for visible layout changes.
- Confirm you ran `quarto render` and that `docs/` is updated accordingly (since it is the deploy artifact).

## Agent-Specific Instructions

If a change involves modifying concrete code, content data, or generated artifacts, ask for explicit approval before editing. Do not make unrequested changes directly (especially destructive or hard-to-reverse edits).

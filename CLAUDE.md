# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Quarto website (R + renv) for www.beepboopstats.com.

## Deploy and workflow
- Merging to `main` deploys: `.github/workflows/render_publish.yml` renders the site in CI and publishes to the `gh-pages` branch. Never edit or push `gh-pages` by hand.
- `docs/` (the `output-dir`) is gitignored local build output. Never commit or force-add it.
- Keep the root `CNAME` (`www.beepboopstats.com`).
- Always work on a feature branch and open a PR; never commit directly to `main`.
- Commit messages: conventional prefix, no scope, lowercase imperative (`feat: add text to the portfolio page`, `chore: update listing style`).

## Verifying changes
- `quarto preview` for visual checks. Run a full `quarto render` only when R chunks or R dependencies change.
- `freeze` is off, so CI re-executes every R chunk. Any new R package must be installed via renv and recorded with `renv::snapshot()`, or the CI render fails.

## Off-limits files
Never read, open, move, or commit `assets/ssl/` (contains a private key), `.Renviron`, `.RData`, or `.Rhistory`.

## Site structure
- Only `.qmd` files are rendered (`project.render` in `_quarto.yml`), so `.md` files like this one stay off the site. Don't remove that setting.
- All custom styling is in `assets/css/styles.scss`, shared by the light (`pulse`) and dark (`darkly`) themes; `assets/css/dark.scss` holds dark-only overrides. Brand color `#62272D` is also hard-coded in the Calendly badge script in `index.qmd`.
- The Contact page lives at `assets/contact/index.qmd`.
- Projects are data, not pages: `projects.yml` (featured, shown on `portfolio.qmd` and the first 3 on `index.qmd`) and `tools.yml` (repos without a site, shown as a table). Display order is file order (`sort: false`, no `date` field). Screenshots go in `assets/img/projects/` (1280×800 JPEG).
- `projects.yml` items render through `assets/listings/projects.ejs`. Quarto passes listing templates through Markdown, so keep the HTML inside the ```` ```{=html} ```` fence and unindented, or it renders as code blocks.

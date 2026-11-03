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
- Projects are data, not pages: `projects.yml` lists featured projects, shown on `portfolio.qmd` and the first 3 on `index.qmd`. Display order is file order (`sort: false`, no `date` field). Screenshots go in `assets/img/projects/` (1280×800 JPEG).
- The "Open-source tools" table is built by the R chunk in `portfolio.qmd` from the GitHub API on every render: public, non-fork, non-archived repos, minus repos already in `projects.yml` and the chunk's `exclude` list. Repos without a GitHub description get a generic one. To hide a repo, add it to `exclude`; to change its text, edit its description on GitHub, not the page. Each successful render rewrites `assets/data/repos.json`, the fallback used if the API fails; commit it.
- Rendering runs R, so locally run `renv::restore()` once before `quarto render`. Don't run `quarto render` while a `quarto preview` of this project is running: both build into the same folders and the render fails intermittently.
- `projects.yml` items render through `assets/listings/projects.ejs`. Quarto passes listing templates through Markdown, so keep the HTML inside the ```` ```{=html} ```` fence and unindented, or it renders as code blocks.

## SEO
- Site-wide description, preview image (`assets/img/og-card.jpg`, 1200×630) and Twitter card live under `website:` in `_quarto.yml`; each page sets its own `description` (and `pagetitle` where it has no title).
- The CI build's Quarto appends " – BeepBoopStats" to every `pagetitle` (local Quarto 1.4 doesn't), so never put the brand in `pagetitle`.
- Favicons are linked from `assets/html/head.html` as real files (root `favicon.ico` plus `assets/img/favicons/`), not via `website.favicon`, which embed-resources inlines as a `data:` URL that Google can't use. The home page's `WebSite` JSON-LD sets the site name Google shows.
- Every page sets `<link rel="canonical">` and `og:url` via `include-in-header`. The canonical link must keep `data-external="1"`: `embed-resources: true` otherwise downloads the live page and inlines it (~10 MB).
- Pages embed their images, so any image referenced by absolute URL (preview images, the JSON-LD `logo`/`image`) must also be listed under `project.resources` or it 404s on the live site. Use root-relative paths (`/assets/...`) for `image:`.

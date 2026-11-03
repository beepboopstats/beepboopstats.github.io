---
name: new-portfolio-item
description: Add a project to projects.yml (or a repo to tools.yml) with a fresh screenshot, on a feature branch, and open a PR. Usage: /new-portfolio-item <site-url> [repo-url]
disable-model-invocation: true
---

Add a new project from `$ARGUMENTS` (a live site URL and, optionally, its public GitHub repo). If there's no live site, it's a tool: add it to `tools.yml` instead and skip the screenshot. Ask for anything you can't work out from the site or the repo README: title, a one-sentence description of what it does and who it's for, and categories.

1. Branch off an up-to-date `main`: `git switch main && git pull && git switch -c feat/project-<slug>`.
2. Screenshot the live site with Playwright at a 1280×800 viewport, as JPEG (quality 80), to `assets/img/projects/<slug>.jpg`. Look at it before using it: skip error states, warnings, and anything personal (browser chrome, bookmarks); scroll to a representative view if the top of the page isn't.
3. Add an entry to `projects.yml`, matching the existing entries' fields (`title`, `path`, `code`, `image`, `description`, `categories`). New projects go at the top, since display order is file order. Do not add a `date` field.
   - Categories: one of `Teaching`, `Research`, `Consulting` where it applies, plus one of `Website`, `Dashboard`, `App`. Reuse existing category names exactly; the filter counts them.
   - Omit `code` if the repo isn't public.
4. Run `quarto render`, then check `portfolio.html` and `index.html` (served from `docs/`) at desktop and phone widths.
5. Commit with `feat: add <slug> to portfolio`, push, and open a PR with `gh pr create`. Merging to `main` deploys.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Yunqi Guo's personal academic website (served at https://guoyunqi.com), built with Jekyll
and the [al-folio](https://github.com/alshedivat/al-folio) theme.

Two repositories are involved:

- `luckiday/yq-page-source` (**private**) — this repo. Holds the source and the CI that
  builds it. Nothing here is publicly readable.
- `luckiday/luckiday.github.io` (**public**) — holds only the built site on `gh-pages`,
  which is what GitHub Pages serves. Entirely machine-generated.

Note that anything the build emits into `_site/` does become public, since the compiled
site is served to the world. The privacy boundary is source-vs-output, not repo-vs-nothing:
keep the Chinese resume and anything with phone numbers or private addresses out of both
(it lives outside, in `~/Desktop/Yunqi_Resume`).

## Commands

```bash
bundle install                                # one-time setup (Ruby 3.2.x)
bundle exec jekyll serve                      # local dev at http://localhost:4000
JEKYLL_ENV=production bundle exec jekyll build  # production build (what CI runs)
docker compose up                             # alternative dev server at :8080 (no Ruby needed)
pre-commit run --all-files                    # whitespace/EOF/yaml/large-file checks
```

There are no unit tests; a successful `jekyll build` is the correctness check. Always run a
build after structural changes (deleting files, editing `_config.yml`) — Liquid errors only
surface at build time. `_config.yml` changes require restarting `jekyll serve`.

## Deploy pipeline

- `master` in this private repo holds the **source**. `gh-pages` in the public
  `luckiday/luckiday.github.io` repo holds the **built site** and is entirely
  machine-generated — never edit or commit to it by hand.
- Pushing to `master` triggers `.github/workflows/deploy.yml`: it builds with
  `JEKYLL_ENV=production` and force-pushes `_site/` to `gh-pages` in the public repo
  (JamesIves/github-pages-deploy-action). Pull requests trigger a build-only check.
- The cross-repo push authenticates with an SSH deploy key: the private half is the
  `DEPLOY_KEY` secret on this repo, the public half is a write-enabled deploy key titled
  `yq-page-source deploy` on `luckiday/luckiday.github.io`. Rotating one means replacing
  both. The public repo's `PAGES_REPO` is set at the top of `deploy.yml`.
- The custom domain comes from the root `CNAME` file (`guoyunqi.com`), which Jekyll copies
  into `_site/` so every deploy carries it. Don't delete it — the domain breaks on the
  next deploy if it stops landing on `gh-pages`.
- DNS lives at Squarespace (inherited Google Domains nameservers): the apex has GitHub
  Pages A records and `www` is a CNAME to `luckiday.github.io`. Moving the public repo or
  renaming it means updating those records.
- Pushing changes to `resume/*.tex` or `resume/*.cls` on `master` triggers
  `.github/workflows/build-resume.yml`, which compiles the LaTeX in CI and commits the
  fresh `assets/pdf/resume.pdf` back to `master`; that commit then triggers the normal
  deploy. So a resume edit only needs the .tex change pushed — no local LaTeX required.
- `bin/deploy` is a legacy manual deploy script from the single-repo days. It pushes
  `gh-pages` to `origin`, which is now the private repo, so it no longer reaches the live
  site. Don't use it.
- `.github/workflows/deploy-image.yml` and `deploy-docker-tag.yml` are upstream-theme
  leftovers (Docker Hub publishing); the former is gated to the `alshedivat` org and inert.

So the workflow for any change is: edit on a branch → verify with a local build → merge to
`master` → push → CI deploys automatically.

## Architecture: content vs. theme

The repo is a theme fork where a few well-defined places hold all the personal content;
almost everything else is al-folio machinery (`_layouts/`, `_includes/`, `_sass/`,
`_plugins/`) that rarely needs touching.

Content lives in:

- `_pages/` — about (home page, `permalink: /`), publications, projects, cv, repositories.
  Navbar membership/order is controlled by `nav:`/`nav_order:` in each page's front matter.
- `_bibliography/yq_papers.bib` — the publication list. `_config.yml` points jekyll-scholar
  at this file (**not** the default `papers.bib`). Entries use custom bibtex fields
  (`abbr`, `pdf`, `selected`, `arxiv`, `website`, …) that `_layouts/bib.html` turns into
  badges/buttons; `selected={true}` features a paper on the home page. `pdf={...}` values
  are filenames under `assets/pdf/`.
- `_news/` — home-page announcements; one markdown file each with `inline: true`. The home
  page shows the 5 newest (`announcements.limit` in `_config.yml`).
- `_posts/` — blog posts. The blog page also merges external posts pulled from the Medium
  RSS feed configured under `external_sources` in `_config.yml` (network fetch at build time).
- `resume/` — LaTeX source of the resume (English only, see privacy note above). This folder
  is excluded from the Jekyll build; the site serves the compiled PDF from
  `assets/pdf/resume.pdf`, embedded by `_pages/cv.md`. To update: edit the .tex and push —
  CI recompiles the PDF automatically (see the deploy section). To preview locally:
  `latexmk -pdf main.tex && cp main.pdf ../assets/pdf/resume.pdf && latexmk -C main.tex`
  (see `resume/README.md`).
- `assets/pdf/` — paper PDFs, slides, and the resume PDF. `assets/img/` — profile photo and
  favicon; the jekyll-imagemagick plugin auto-generates responsive `.webp` variants for any
  image added here.

`_config.yml` is the hub: identity/social links, jekyll-scholar settings, feature toggles,
and collection definitions all live there. Comments (giscus/disqus) are currently disabled —
the giscus repo/category IDs are deliberately blank; fill them via giscus.app before enabling.

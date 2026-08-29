# Yunqi Guo — Personal Website

Source of [luckiday.github.io](https://luckiday.github.io), built with [Jekyll](https://jekyllrb.com/)
and the [al-folio](https://github.com/alshedivat/al-folio) theme, hosted on GitHub Pages.

## Layout

| Path | Content |
| --- | --- |
| `_pages/` | Main pages: about (home), publications, projects, cv, repositories |
| `_bibliography/yq_papers.bib` | Publication list (rendered by jekyll-scholar; `selected={true}` entries appear on the home page) |
| `_news/` | Short news/announcement items shown on the home page |
| `_posts/` | Blog posts |
| `resume/` | LaTeX sources of the resume (EN + CN); see `resume/README.md` |
| `assets/pdf/` | Paper PDFs, slides, and the compiled resume (`resume.pdf`, `resume_cn.pdf`) served on `/cv/` |
| `_config.yml` | Site configuration |

## Local development

```bash
bundle install
bundle exec jekyll serve
```

Or with Docker:

```bash
docker compose up
```

## Common updates

- **News**: add a markdown file in `_news/` with `inline: true` front matter.
- **Publication**: add a BibTeX entry to `_bibliography/yq_papers.bib`; set `selected={true}` to feature it on the home page.
- **Resume**: edit the LaTeX in `resume/`, then follow `resume/README.md` to rebuild and copy the PDFs into `assets/pdf/`.

## License

Theme code inherits the [al-folio MIT license](LICENSE). Site content © Yunqi Guo.

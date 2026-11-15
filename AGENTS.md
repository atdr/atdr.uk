# atdr.uk

<!-- Stable instructions only. TODOs, status and changelogs do not belong here:
they change too often and cost context on every session. The backlog is GitHub
Issues; the record of what happened is git history. -->

Andreas Richardson's personal site and blog at <https://atdr.uk>. A Jekyll site on the [indigo](https://github.com/sergiokopplin/indigo) theme (this repo is a fork of it, with the theme's upstream as the `upstream` remote), built by GitHub Pages' own Jekyll action and deployed by `deploy.yml` on every push to `main`.

## Commands

```bash
docker compose up                              # serve locally on :4000
npx markdownlint-cli2 && mdformat --check .    # what checks.yml runs
```

## Architecture

- `_config.yml` holds the site settings, the author profile and the `exclude` list. A repo file that is not a page goes in `exclude`, or Jekyll publishes it.
- `_posts/YYYY-MM-DD-slug.md` are the blog posts; the front matter sets the layout, image, tags and any `redirect_from` paths.
- `_layouts/`, `_includes/`, `_sass/` and `assets/` are the theme, with local changes on top.
- `ci.yml` runs the Jekyll build on pull requests, and `deploy.yml` calls it before building and publishing, so a failed build never reaches the live site.

## Conventions

- **Git workflow:** GitHub Flow. Every change lands on a `<type>/<description>` branch and merges by squash PR. Never commit directly to `main`.
- **Commits and PR titles:** Conventional Commits, `<type>(<scope>): <summary>`, with types `feat fix perf refactor test docs chore ci`. A new post is `feat`.
- **Markdown:** one line per paragraph or list item, as the posts are written. mdformat keeps the wrapping and markdownlint's line length rule is off.
- **Workflows:** each file's `name:` is its filename stem, and every job sets `timeout-minutes`. See atdr/github-boilerplate's STANDARDS.md.

## Gotchas

- Jekyll renders the posts with kramdown, mdformat formats them as CommonMark. The two agree almost everywhere, but check a formatting change against the built page. A backslash escape is where they part: `\---` is a hyphen and an en dash in kramdown.
- The theme's own pages (`CONTRIBUTION.md`, `FAQ.md`, `about.md`) are excluded from the Markdown checks, so merging theme updates from `upstream` does not drag in reformatting. `about.md` is still the theme's placeholder text.
- `main` takes squash merges only, so a theme update arrives as a branch that merges `upstream/gh-pages`, opened as a PR and squashed.

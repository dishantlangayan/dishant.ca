# CLAUDE.md

Personal blog (dishant.ca) built with [Hugo](https://gohugo.io) and the [cleanwhite](https://github.com/zhaohuabing/hugo-theme-cleanwhite) theme, deployed on Netlify.

## Commands
- `hugo server -D` — local dev server with drafts (http://localhost:1313)
- `hugo --gc --minify` — production build into `public/` (gitignored)
- `hugo new content post/my-post.md` — new post from `archetypes/default.md`
- `git submodule update --init --recursive` — fetch the theme after cloning

## Structure
- `hugo.toml` — site config (theme, params, sidebar, menus). Theme params are documented in `themes/cleanwhite/exampleSite/hugo.toml`.
- `content/post/` — blog posts. The theme only lists pages whose type/section is `post`, so posts must live here (not `posts/`).
- `content/about/`, `content/archive/` (uses `layout: archive`), `content/search/` — standalone pages.
- `static/` — files copied verbatim to the site root (favicons, `img/` header images).
- `themes/cleanwhite/` — git submodule; do not edit. Override by placing files with the same path under a top-level `layouts/` or `static/`.
- `netlify.toml` — build config; `HUGO_VERSION` must match a Hugo version that supports the theme.

## Conventions
- Front matter is YAML (`---`); fields: title, subtitle, description, date, author, image, tags, categories, draft.
- New posts are `draft: true` until ready; drafts are excluded from production builds.
- Post filenames: `YYYY-MM-DD-slug.md`.

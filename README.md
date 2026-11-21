# dishant.ca

Source for [Dishant Langayan's blog](https://dishant.ca/), built with [Hugo](https://gohugo.io) and the [cleanwhite](https://github.com/zhaohuabing/hugo-theme-cleanwhite) theme.

## Prerequisites
- [Hugo](https://gohugo.io/installation/) (extended, v0.170.1 or newer recommended — see `netlify.toml`)
- Git

## Getting started
```sh
git clone --recurse-submodules https://github.com/dishantlangayan/dishant.ca.git
cd dishant.ca
# if you already cloned without submodules:
git submodule update --init --recursive
```

## Build and preview
```sh
hugo server -D        # live-reloading dev server incl. drafts at http://localhost:1313
hugo --gc --minify    # production build into public/
```

## Writing a post
1. Create a post (it lives in `content/post/`):
   ```sh
   hugo new content post/2026-01-01-my-post.md
   ```
2. Edit the front matter and write in Markdown:
   ```yaml
   ---
   title: "My Post"
   subtitle: ""
   description: "Short summary"
   date: 2026-01-01T09:00:00-05:00
   author: "Dishant Langayan"
   image: ""            # header image, e.g. img/post-bg.jpg (placed in static/img/)
   tags: ["example"]
   categories: ["Tech"]
   draft: true
   ---
   ```
3. Preview with `hugo server -D`, then set `draft: false` when ready.
4. Put images in `static/img/` and reference them as `/img/name.jpg`.

## Contributing
Contributions such as typo fixes, corrections, and improvements to posts or the site are welcome.
1. Fork the repository and create a branch: `git checkout -b my-change`.
2. Make your changes and verify `hugo --gc --minify` builds without errors.
3. Commit with a clear message and open a pull request against `main`.

New blog posts by guest authors: open an issue first to discuss the topic, then submit the post as a PR with your name in `author`.

## Deployment
Pushes to `main` are built and deployed by Netlify using `netlify.toml`.

## License
Licensed under the [Apache License 2.0](LICENSE). The theme in `themes/cleanwhite` is distributed under its own license.

# jaredforte.com

My personal site and blog, built with [Zola](https://www.getzola.org/) and deployed to GitHub Pages.

## Run it locally

1. Install **Zola 0.23.6**, the same version CI uses (the `ZOLA_VERSION` setting in `.github/workflows/main.yml`). Download it from the [Zola releases page](https://github.com/getzola/zola/releases/tag/v0.23.6), unzip it, and put `zola` (or `zola.exe`) somewhere on your `PATH`.
2. From the repo folder, run:

   ```sh
   zola serve
   ```

3. Open <http://127.0.0.1:1111>. The page reloads as you edit.

`zola build` writes the finished site to `public/`, which is ignored by git.

## Write a new post

Create `content/<post-slug>.md`. The file name becomes the URL (`/post-slug/`). Start it with:

```toml
+++
title = "Post title"
date = 2026-01-31
[taxonomies]
categories = ["Running"]
tags = ["mountaineering", "fell-running"]
[extra]
cover = "/images/my-photo.jpg"
+++

First paragraph...
```

- `categories` and `tags` can be any names; new ones get their own pages automatically. Existing categories are `Running`, `SOTA` and `Technology`.
- `cover` is optional. It is the photo shown on the home page card and at the top of the post. Leave out the whole `[extra]` section and the post gets the contour-line pattern instead.
- Write the post in plain Markdown. Leave a blank line between paragraphs; there's no need for `<br>`.

## Add images

Put image files in `static/images/`. Keep them under about 600 KB: resize photos to around 1600 px wide before adding them.

A plain Markdown image fills the text column:

```md
![A map of the route](/images/route-map.jpg)
```

For other layouts, use the `figure` component on its own line, with a blank line above and below:

```md
{{ <figure src="/images/summit.jpg" alt="The summit cairn in snow" layout="float" /> }}
```

| `layout` | Result |
|---|---|
| `full` (default) | Fills the text column |
| `small` | Narrower and centred. Two `small` figures in a row sit side by side. |
| `float` | Floats to the right of the text on wider screens |

You can also add `caption="..."` to show a caption under the image. Always fill in `alt` with a short description of the photo.

## Change the look

| To change | Edit |
|---|---|
| Colours (light and dark mode), fonts, spacing | `sass/_tokens.scss` |
| Page shell: header, nav, hero, footer | `sass/_layout.scss`, `templates/base.html` |
| Cards, chips, figures | `sass/_components.scss`, `templates/components.html` |
| Reading styles for post text | `sass/_base.scss` |
| Home page photo | `templates/index.html` (`hero_path`) |

Every colour in the stylesheets comes from `sass/_tokens.scss`. Light-mode colours are under `:root`, and dark-mode colours are in the `dark-colours` block below them. Keep text readable: aim for a contrast ratio of at least 4.5:1 against the background.

The fonts (Barlow Condensed and Source Serif 4) are in `static/fonts/` with their licences.

## Add a nav link

Add an entry to `config.toml`. It appears in the header and footer of every page:

```toml
[[extra.nav]]
name = "Trips"
path = "/categories/running/"
```

## Deploying

Pushing to `main` builds the site and deploys it to <https://jaredforte.com>. Pushes to any other branch, and pull requests, only build it, so you can check a change builds before merging.

## Upgrading Zola

Zola sometimes makes breaking changes, so it is pinned. To upgrade:

1. Read the [changelog](https://github.com/getzola/zola/blob/master/CHANGELOG.md) for every version between the current one and the new one.
2. Change `ZOLA_VERSION` in `.github/workflows/main.yml` and install the same version locally.
3. Run `zola build` and fix any errors. Check a few pages in `zola serve` before merging.

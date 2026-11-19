# martinwood.org — Quick Reference

## Running locally

```bash
cd ~/src/martinwood.org
bundle exec jekyll serve --host 0.0.0.0
```

Visit http://127.0.0.1:4000 (or http://your-local-ip:4000 from another device).

Config changes require a server restart. Everything else (posts, CSS, layouts) hot-reloads automatically.

---

## How the site is laid out (Nov 2026)

- **The whole site is styled as a 1980s computer manual**, using `_layouts/manual.html` and `assets/css/manual.css`, with no Chirpy sidebar, topbar or search. The repo overrides Chirpy's `post`, `page`, `archives`, `categories`, `tags`, `category` and `tag` layouts, plus `assets/404.html`, so every page uses it: About is "Appendix A", Writing is "Appendix B" (a contents page), and categories and tags are back-of-book indexes. Posts older than three years show an "Earlier model" note. Code highlighting is Rouge's `github` style, appended to `manual.css`. Chirpy is still the theme gem for its plugins and includes (`favicons.html`). After upgrading it, check `bundle exec jekyll build` for a "Conflict" warning, which means a theme file now shadows one of ours. Set a `book_class` in front matter to add a class to the page.
- **Homepage (`index.html`)** links to both appendices at the bottom. Chirpy's paginated `home` layout is no longer used, so `paginate` is gone from `_config.yml`. It is `.html`, not `.md`, so that kramdown doesn't treat the indented HTML in the includes as code blocks.
- **Homepage content** is in `_includes/home-employed.html`. The Specifications receipt numbers live there too.
- **Homepage images** live in `assets/img/home/`. The portrait is the site `avatar`.
- **Favicon:** a white M on the manual red (#D62828), in `assets/img/favicons/`, overriding Chirpy's default icons. `favicon.svg` is the source (the M is a path, so no font is needed). Regenerate the rest from it: `rsvg-convert -w 96 -h 96 favicon.svg -o favicon-96x96.png`, the same at 180 for `apple-touch-icon.png`, and `favicon.ico` from 16, 32 and 48px PNGs with `magick`.
- **Gotcha – link hover colour:** every link hovers white on red through the plain `a:hover` rule, but any later `.something a { color: … }` beats it, giving grey or black text on red (the breadcrumb and Older/Newer links did this until Nov 2026). When you give links in a new block their own colour, add the block to the `.kicker a:hover, … { color: var(--paper) }` rule in `manual.css`.
- **Tabs:** About (`_tabs/about.md`, layout `manual`) and Writing (`_tabs/writing.md`, layout `archives`). They are reached from the footer of every page, which is `_includes/manual-back.html`.
- **Categories and tags** are root pages (`categories.md`, `tags.md`) with fixed permalinks, so post links and breadcrumbs still work but they stay out of the sidebar.
- Link check: `bundle exec htmlproofer _site --disable-external`. The failures it reports are all in old posts (http links, a dead mashable link) and predate the redesign.

---

## Deploying

Just push to `main` — GitHub Actions builds and deploys automatically:

```bash
git add .
git commit -m "your message"
git push
```

Watch the build at: https://github.com/mwood/martinwood.org/actions

---

## Adding a post

Create a file in `_posts/` named `YYYY-MM-DD-your-title.md`:

```markdown
---
title: Your Post Title
date: 2026-02-27 09:00:00 +0000
categories: [Programming, Rails]
tags: [ruby, rails]
---

Your content here in Markdown.
```

- `categories` — up to 2 levels, shown in the sidebar
- `tags` — shown on post and in tag cloud
- Images go in `assets/img/posts/` and reference as `![alt](/assets/img/posts/image.png)`

---

## Adding a page

Create a file in `_tabs/` (for sidebar nav items):

```markdown
---
title: Your Page Title
icon: fas fa-your-icon   # Font Awesome icon
order: 5                 # Position in sidebar (existing: about=4)
---

Your content here.
```

Icon names: https://fontawesome.com/icons (free icons only)

For a standalone page (not in sidebar), just create `your-page.md` in the root with `layout: page`.

---

## Custom CSS

Add styles to `assets/css/jekyll-theme-chirpy.scss`:

```scss
---
---

@use 'main
{%- if jekyll.environment == "production" -%}
  .bundle
{%- endif -%}
';

/* your styles here */
h1 {
  color: red;
}
```

---

## Drafts

Create posts in `_drafts/` (no date in filename) — they won't publish until moved to `_posts/`.

Preview drafts locally with:

```bash
bundle exec jekyll serve --host 0.0.0.0 --drafts
```

---

## Chirpy docs

https://chirpy.cotes.page/posts/getting-started/

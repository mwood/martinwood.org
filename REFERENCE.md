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

- **Homepage (`index.md`)** is a bio and selected work, not a post list. Chirpy's paginated `home` layout is no longer used, so `paginate` is gone from `_config.yml`.
- **Sidebar tabs:** About (`_tabs/about.md`, the longer bio) and Writing (`_tabs/writing.md`, the archive of all posts).
- **Categories and tags** are root pages (`categories.md`, `tags.md`) with fixed permalinks, so post links and breadcrumbs still work but they stay out of the sidebar.
- **`_layouts/default.html` overrides the theme's copy** (Chirpy 7.4.1) to show "Recently Updated" and "Trending Tags" only beside posts. After upgrading Chirpy, re-copy it from `bundle info --path jekyll-theme-chirpy` and re-apply the `page.layout == 'post'` check.
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

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is **alexsm-theme**, a minimal Hugo theme. It has no build system — no npm, no webpack, no Makefile. CSS is plain CSS in `static/css/style.css`, and templates are standard Hugo Go templates.

## Development Commands

To preview the theme, run from the parent Hugo site directory (not this theme directory):

```bash
hugo server
hugo server --disableFastRender   # full rebuild on every change
hugo build                        # build static site
```

## Architecture

### Template hierarchy

All pages extend `layouts/_default/baseof.html` via the `{{- block "main" . }}` pattern. The base template wires together partials: `head.html` → `header.html` → `[main block]` → `footer.html` → `script.html`.

- **`layouts/index.html`** — Homepage: shows all tags by count, then recent blog posts filtered by `Layout == "post"`
- **`layouts/_default/list.html`** — Tag pages and section listings; delegates rendering to the `post-list.html` partial
- **`layouts/_default/single.html`** — Individual pages/posts; shows date and tags only for `Section == "blog"`

### Partials

- **`head.html`** — Loads `style.css`; conditionally loads KaTeX (when `.Params.math == true`)
- **`header.html`** — Site nav: title/subtitle + top-level pages + Collections + RSS icon
- **`post-list.html`** — Shared list renderer used by both `list.html` and `index.html`
- **`helpers/katex.html`** — Loads KaTeX from CDN; supports `$$...$$` (display) and `$...$` (inline)
- **`script.html`** — Currently empty; placeholder for future JS

### Content sections

The theme treats sections differently based on `.Section`:

| Section | Behavior |
|---|---|
| `blog` | Shows date + tags on single pages, date in post list |
| `collection` | List heading reads "Collections" |
| `""` (root) | Pages appear in header nav |

### Styling

Plain CSS in `static/css/style.css`. Key design tokens:
- Header: `#003566` (dark blue), hover: `#fca311` (orange)
- Footer: `#fca311` (orange)
- Tag badges: `#1E96FC` (bright blue)
- Content max-width: `50rem`
- Mobile breakpoint: `768px`

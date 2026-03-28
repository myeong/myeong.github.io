# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Jekyll 4.3 static site — personal academic portfolio for Myeong Lee (myeonglee.com). Deployed via GitHub Pages with custom domain (CNAME). Originally migrated from Drupal.

## Build & Development Commands

```bash
bundle install              # Install Ruby dependencies
bundle exec jekyll serve    # Local dev server (http://localhost:4000)
bundle exec jekyll build    # Build to _site/
```

No test suite exists. Verify changes by inspecting `_site/` output or the local dev server.

## Architecture

**Data-driven content model**: All structured content lives in YAML files under `_data/`, not in individual markdown files. Pages iterate over these YAML arrays with Liquid templates.

- `_data/publications.yml` (65+ entries) — the main publication database
- `_data/projects.yml` (20+ entries) — project database
- `_data/news.yml`, `_data/media.yml`, `_data/teaching.yml`, `_data/travels.yml`, `_data/navigation.yml`

**Detail pages use client-side rendering**: Each publication/project has a thin `publications/[slug]/index.html` or `projects/[slug]/index.html` with only front matter (slug, layout). JavaScript in the layout templates (`_layouts/publication-detail.html`, `_layouts/project-detail.html`) parses `window.location.pathname` to find the matching YAML entry and renders the content at runtime.

**Layout hierarchy**: `_layouts/default.html` is the single base layout providing header, navigation, two-sidebar structure, and footer. `_includes/sidebar.html` (profile/recent pubs) and `_includes/news-sidebar.html` (news/travel, home page only) are the sidebar partials.

**Single CSS file**: `assets/css/style.css` — uses Google Fonts "Roboto Slab", CSS custom properties for font sizes, max-width 1200px container.

## Adding Content

**New publication**: Add entry to `_data/publications.yml`, create `publications/[slug]/index.html` with front matter (`layout: publication-detail`, `slug: [slug]`), optionally add thumbnail to `assets/images/publications/`.

**New project**: Add entry to `_data/projects.yml`, create `projects/[slug]/index.html` with front matter (`layout: project-detail`, `slug: [slug]`), optionally add image to `assets/images/projects/`.

**News/media/teaching**: Just add entries to the respective `_data/*.yml` file — no additional pages needed.

## Key Conventions

- Permalinks use pretty URLs (`/path/` not `/path.html`)
- Markdown processor: kramdown
- JavaScript in `default.html` forces external links to open in new tabs and underlines "Myeong Lee" in author lists
- Projects page has client-side category filtering

## Work History

### 2026-03-28: Publications page improvements & site polish
- **Venue data consistency**: Standardized venue info across 65+ publications — added missing volume/page numbers, removed inconsistent "In " prefixes, normalized date abbreviations, added publisher info, fixed typos (e.g., "MPDI" → "MDPI")
- **Publication type tags**: Added `publication_type` as a visible tag below author names on both "By Year" and "By Type" listing pages
- **Award + type display**: Publication type and award badge shown on the same line below authors (type first, then award). Publication type styled as small (0.75rem), weight 400, gray (#888)
- **Detail page layout**: Moved award/type tags below title; external link placed inside the gray detail fields box below authors
- **CSS tweaks**: Matched line-height (1.35) for title and venue; aligned thumbnail top with title top (`align-items: flex-start`); reduced `.pub-tags` margin to 2px; removed extra badge margin inside `.pub-tags`
- **Projects page**: Made thumbnail images clickable (link to detail page or external link); reduced gap between thumbnail and status tags (10px → 4px)
- **Favicon**: Added favicon from gmu-cil.github.io; added `<link rel="icon">` to `_layouts/default.html`

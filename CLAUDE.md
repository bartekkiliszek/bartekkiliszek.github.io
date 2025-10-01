# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal blog built with Jekyll and hosted on GitHub Pages, focused on backend development, Java, and software engineering topics.

## Architecture

### Jekyll Site Structure

- **_posts/**: Blog post content files with naming convention `YYYY-MM-DD-title.md`
- **_layouts/**: HTML templates defining page structures
  - `default.html`: Base template with header, nav, and footer
  - `home.html`: Homepage layout
  - `page.html`: Static pages layout
  - `post.html`: Blog post layout
- **assets/css/**: Stylesheets
  - `style.css`: Main site styles
  - `syntax.css`: Code syntax highlighting
- **_config.yml**: Jekyll configuration including site metadata, plugins, and build settings

### Key Configuration

- **Theme**: Minima
- **Plugins**: jekyll-feed, jekyll-seo-tag, jekyll-sitemap
- **Markdown**: kramdown with GitHub-flavored markdown (GFM)
- **Syntax Highlighting**: Rouge with line numbers enabled
- **Permalink Structure**: `/blog/:year/:month/:day/:title/`

### Creating New Blog Posts

All blog posts must:
1. Be placed in `_posts/` directory
2. Follow naming convention: `YYYY-MM-DD-title.md`
3. Include front matter with required fields:

```yaml
---
layout: post
title: "Your Post Title"
date: YYYY-MM-DD
categories: [Category1, Category2]
tags: [tag1, tag2, tag3]
excerpt: "Brief description of your post"
---
```

### Layout System

The site uses Jekyll's template inheritance:
- All pages inherit from `default.html` which provides consistent header/footer
- Posts use `post.html` which wraps content with post metadata
- Static pages use `page.html` for simple content presentation
- Homepage uses `home.html` to display recent posts

### Custom Styling

The blog uses custom CSS (not relying solely on Minima theme defaults):
- Custom fonts: Inter for body text, JetBrains Mono for code
- Responsive design with mobile-first approach
- Code blocks styled with syntax highlighting via Rouge

## Important Notes

- This site is deployed automatically via GitHub Pages when pushing to the `main` branch
- No build step required locally for deployment (GitHub Pages handles Jekyll build)
- Posts are written in Markdown and automatically converted to HTML
- Syntax highlighting is configured for code blocks with line numbers

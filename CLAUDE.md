# CLAUDE.md — AI Assistant Guide for yannarak.github.io

This file provides context for AI assistants (Claude, Copilot, etc.) working in this repository.

---

## Project Overview

This is a **Jekyll-based academic personal website** for **Yannarak Wannasai**, built on the [al-folio](https://github.com/alshedivat/al-folio) theme. It is hosted on **GitHub Pages** and deployed automatically via GitHub Actions.

- **URL:** https://yannarak.github.io
- **Owner:** Yannarak Wannasai (Cybersecurity / Application Security professional at Mercari, ex-Rakuten, ex-Woven by Toyota)
- **License:** MIT

---

## Technology Stack

| Layer | Technology |
|---|---|
| Site Generator | Jekyll 4.x |
| Templating | Liquid |
| Styling | SCSS/SASS → compiled CSS |
| JavaScript | Vanilla JS (no build step required) |
| Ruby Deps | Bundler (`Gemfile`) |
| Node Deps | npm (minimal — only PurgeCSS) |
| Hosting | GitHub Pages |
| CI/CD | GitHub Actions (14 workflows) |
| Containerization | Docker / Docker Compose |

---

## Repository Structure

```
.
├── _bibliography/       # BibTeX publication references (papers.bib)
├── _books/              # Book review entries
├── _data/               # YAML data files (cv, socials, repos, citations)
├── _includes/           # Liquid partial templates (header, footer, figures, etc.)
├── _layouts/            # Page layout templates (about, post, distill, cv, bib)
├── _news/               # News/announcement snippets
├── _pages/              # Website pages (about, blog, publications, projects, cv)
├── _plugins/            # Custom Jekyll Ruby plugins
├── _posts/              # Blog posts (format: YYYY-MM-DD-slug.md)
├── _projects/           # Project pages
├── _sass/               # SCSS source files
├── _scripts/            # Liquid/JS scripts loaded conditionally
├── assets/
│   ├── css/             # Generated CSS (do not edit directly)
│   ├── js/              # JavaScript source files
│   ├── img/             # Images
│   ├── pdf/             # PDF assets
│   └── json/            # JSON data (resume.json, table data)
├── bin/                 # Build and deployment scripts
├── .github/
│   ├── workflows/       # CI/CD workflow definitions
│   └── agents/          # GitHub Copilot agent prompts
├── _config.yml          # Main Jekyll configuration (primary file to understand)
├── Gemfile              # Ruby dependencies
├── package.json         # Node dependencies (Prettier only)
└── docker-compose.yml   # Local development via Docker
```

---

## Key Configuration

### `_config.yml`
The primary configuration file. Key sections:
- **Site metadata:** `title`, `first_name`, `last_name`, `description`, `url`, `keywords`
- **Theme colors:** `repo_theme_light`, `repo_theme_dark`
- **Layout:** `navbar_fixed`, `footer_fixed`, `search_enabled`, `max_width`
- **Plugins:** Lists all 15+ Jekyll plugins used
- **Library versions:** CDN URLs for ~50 third-party libraries (MathJax, Bootstrap, Chart.js, etc.)
- **Scholar settings:** BibTeX rendering configuration

### `_data/cv.yml`
Structured YAML for the CV page (education, experience, skills, etc.).

### `_data/socials.yml`
Social media profile links for the about page.

### `_data/repositories.yml`
GitHub users/repos to showcase on the repositories page.

### `_bibliography/papers.bib`
BibTeX file for all publications. Supports custom fields: `pdf`, `code`, `slides`, `video`, `arxiv`, `doi`, `abstract`, `selected`.

---

## Development Workflow

### Local Development (Docker — recommended)
```bash
docker compose up
# Site available at http://localhost:8888
```

### Local Development (native)
```bash
bundle install
npm install
bundle exec jekyll serve --livereload
```

### Build for Production
```bash
JEKYLL_ENV=production bundle exec jekyll build
```

### CSS Purging (production only)
```bash
npx purgecss --config purgecss.config.js
```

---

## Deployment

Deployment is fully automated:
1. Push to `main`/`master` branch
2. GitHub Actions (`.github/workflows/deploy.yml`) triggers
3. Jekyll builds the site with `JEKYLL_ENV=production`
4. PurgeCSS removes unused styles
5. Site deploys to GitHub Pages via `JamesIves/github-pages-deploy-action`

**Never edit the `gh-pages` branch directly** — it is overwritten on every deploy.

---

## Content Conventions

### Adding a Blog Post
Create `_posts/YYYY-MM-DD-slug.md` with front matter:
```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM:SS +TZ
description: Short description
tags: [tag1, tag2]
categories: cat1 cat2
---
```

### Adding a Publication
Add a BibTeX entry to `_bibliography/papers.bib`. Use `selected: true` to feature it on the about page. Custom fields:
- `pdf: /assets/pdf/paper.pdf`
- `code: https://github.com/...`
- `slides: /assets/pdf/slides.pdf`
- `preview: image.jpg`

### Adding a Project
Create `_projects/N_project.md` with front matter:
```yaml
---
layout: page
title: Project Name
description: Short description
img: assets/img/project.jpg
importance: 1
category: work   # or "fun"
---
```

### Adding News
Create `_news/announcement_N.md` with front matter:
```yaml
---
layout: post
date: YYYY-MM-DD HH:MM:SS +TZ
inline: true   # true for short items, false for full page
---
```

### CV Data
Edit `_data/cv.yml` to update the CV. The structure follows timetable sections (education, experience, etc.) and list sections (skills, languages, etc.).

---

## Styling Conventions

- **Source files:** `_sass/*.scss` — edit these, never the compiled CSS in `assets/css/`
- **Variables:** `_sass/_variables.scss` for SCSS variables
- **Themes:** `_sass/_themes.scss` for light/dark mode color definitions
- **Theme system:** CSS custom properties (`var(--global-*)`) controlled by the `data-theme` attribute on `<html>`
- **Dark mode:** User preference is stored in `localStorage` and synced on load
- **Responsive:** Bootstrap 5 grid; design is mobile-first
- **Images:** Use Jekyll's `{% include figure.liquid %}` tag, not bare `<img>` tags, to get lazy loading and responsive variants

---

## JavaScript Conventions

- Each feature has its own setup file (e.g., `mermaid-setup.js`, `plotly-setup.js`)
- `theme.js` handles light/dark/system mode switching with localStorage
- No transpilation or bundler — scripts are loaded directly
- Libraries are loaded conditionally per page via `_scripts/` Liquid files
- Analytics (`google-analytics-setup.js`, etc.) are loaded only when configured in `_config.yml`

---

## Liquid Templating Conventions

- Layouts live in `_layouts/*.liquid`
- Reusable partials live in `_includes/*.liquid`
- Use `{% include figure.liquid path="..." alt="..." %}` for images
- Use `{% cite key %}` and `{% bibliography %}` for academic citations
- Custom tags from plugins are available: `{% details %}`, `{% tabs %}`

---

## Jekyll Plugins

Custom plugins in `_plugins/`:

| Plugin | Purpose |
|---|---|
| `google-scholar-citations.rb` | Auto-fetch citation counts from Google Scholar |
| `inspirehep-citations.rb` | Fetch citations from InspireHEP |
| `external-posts.rb` | Aggregate posts from external feeds |
| `download-3rd-party.rb` | Download CDN assets as fallback |
| `cache-bust.rb` | Cache-busting fingerprints for assets |
| `hide-custom-bibtex.rb` | Filter custom BibTeX fields from display |
| `file-exists.rb` | Conditional includes based on file existence |
| `details.rb` | `{% details %}` expandable section tag |
| `remove-accents.rb` | Accent normalization for URLs |

---

## CI/CD Workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `deploy.yml` | Push to main | Build and deploy site |
| `prettier.yml` | PR / push | Enforce code formatting |
| `codeql.yml` | Schedule / push | Security scanning |
| `broken-links.yml` | Schedule | Validate internal links |
| `broken-links-site.yml` | Schedule | Validate live site links |
| `lighthouse-badger.yml` | Schedule | Performance metrics |
| `axe.yml` | Schedule | Accessibility testing |
| `update-citations.yml` | Schedule | Refresh Google Scholar data |
| `docker-slim.yml` | Release | Build slim Docker image |
| `deploy-image.yml` | Push to main | Publish Docker image |

---

## Code Formatting

This project uses **Prettier** for all markup and scripting files.

```bash
npx prettier --write .    # format all files
npx prettier --check .    # check formatting without changes
```

Configured in `.prettierrc`:
- 150 character line width
- Shopify Liquid plugin for `.liquid` files
- ES5 trailing commas

Pre-commit hooks (`.pre-commit-config.yaml`) enforce trailing whitespace, EOF newlines, YAML validity, and no large files.

---

## Common Tasks for AI Assistants

### Do
- Edit `_config.yml` to change site-wide settings
- Edit `_pages/about.md` to update the homepage content
- Add entries to `_data/cv.yml` for CV updates
- Add entries to `_bibliography/papers.bib` for new publications
- Modify `_sass/` files for style changes
- Create new posts in `_posts/` following the naming convention

### Don't
- Edit files in `assets/css/` directly — they are generated
- Push to `gh-pages` branch — it's auto-managed
- Modify `Gemfile.lock` manually — use `bundle update`
- Add inline styles to Liquid templates — use SCSS variables and custom properties instead
- Use `<img>` tags directly in posts — use the `{% include figure.liquid %}` partial

### When Updating Personal Info
1. `_config.yml` — name, description, keywords, social handles
2. `_data/socials.yml` — social media links
3. `_data/cv.yml` — education, experience, skills
4. `_pages/about.md` — homepage bio text
5. `assets/json/resume.json` — JSON Resume format (for JSON export)

---

## Third-Party Libraries (CDN, versions in `_config.yml`)

- **Bootstrap 5** — Layout/UI framework
- **MathJax 3** — LaTeX math rendering
- **Mermaid** — Diagram generation from text
- **Chart.js**, **ECharts**, **Plotly**, **Vega-Lite** — Data visualization
- **PhotoSwipe**, **Lightbox2** — Image gallery/zoom
- **Font Awesome**, **Academicons**, **Tabler Icons** — Icon sets
- **Giscus** — GitHub Discussions–based comments
- **Diff2Html** — Git diff rendering
- **TikZJax** — TikZ diagram rendering in browser

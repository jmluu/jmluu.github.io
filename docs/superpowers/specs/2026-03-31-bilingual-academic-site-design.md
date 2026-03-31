# Bilingual Academic Website — Design Spec

## Overview

A bilingual (English/Chinese) academic website for jmluu.github.io, built with Hugo + HugoBlox Academic theme, deployed to GitHub Pages via GitHub Actions.

## Decisions

- **Framework**: Hugo (static site generator)
- **Theme**: HugoBlox Academic (free, via Go module)
- **Languages**: English (default at `/`) + Chinese (at `/zh/`)
- **Domain**: `jmluu.github.io` (no custom domain)
- **Deployment**: GitHub Actions builds Hugo, deploys to GitHub Pages
- **Publications**: BibTeX import via `academic-file-converter`, shared across languages

## Site Sections

| Section | Bilingual? | Notes |
|---------|-----------|-------|
| About/Bio | Yes (EN/ZH) | Includes teaching, contact info, CV download, social links, profile photo |
| Publications | No (shared) | Same content for both languages, auto-generated from BibTeX |
| Projects | Yes (EN/ZH) | One Markdown file per project |
| Talks/Events | Yes (EN/ZH) | One Markdown file per event |
| Blog/Posts | Yes (EN/ZH) | Standard blog posts |

## Content Structure

```
jmluu.github.io/
├── hugo.yaml                  # Main config: site title, theme, language settings
├── go.mod                     # Hugo module reference to HugoBlox theme
├── papers.bib                 # Master BibTeX file for publications
├── content/
│   ├── en/                    # English content (default language)
│   │   ├── _index.md          # Homepage
│   │   ├── about.md           # Bio, contact, teaching, CV
│   │   ├── project/           # One folder per project
│   │   ├── event/             # One folder per talk/event
│   │   └── post/              # Blog posts
│   ├── zh/                    # Chinese content (mirrors en/ structure)
│   │   ├── _index.md
│   │   ├── about.md
│   │   ├── project/
│   │   ├── event/
│   │   └── post/
│   └── publication/           # Shared across languages (not under en/ or zh/)
│       └── <paper-name>/
│           └── index.md       # Auto-generated from papers.bib
├── assets/media/              # Images, profile photo, CV PDF
├── i18n/                      # UI string overrides (if needed)
├── .github/workflows/
│   └── deploy.yml             # GitHub Actions: build Hugo → deploy to Pages
└── .gitignore
```

## Language Configuration

- Default language: `en` (English), served at `/`
- Secondary language: `zh` (Chinese), served at `/zh/`
- Language switcher in navbar (provided by HugoBlox)
- Content translation: parallel files in `content/en/` and `content/zh/`
- Publications: single `content/publication/` folder, displayed on both language versions

## Publications Workflow

1. Maintain a master `papers.bib` file in the repo root
2. Run `academic import --bibtex papers.bib` to auto-generate Markdown files in `content/publication/`
3. Each generated paper gets its own folder with an `index.md` containing frontmatter (title, authors, date, journal, abstract, links)
4. To add new papers: update `papers.bib`, re-run the import command

## Deployment

- GitHub Actions workflow triggered on push to `main`
- Installs Hugo Extended + Go
- Runs `hugo --minify` to build the site
- Deploys the `public/` output to GitHub Pages
- No custom domain configuration needed

## Dependencies

- **Hugo Extended** (required by HugoBlox for SCSS processing)
- **Go** (required for Hugo modules)
- **academic-file-converter** (Python package, for BibTeX → Markdown conversion)

## Local Development

```bash
hugo server
```

Serves the site at `http://localhost:1313` with live reload. Changes to content files are reflected instantly in the browser.

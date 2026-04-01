# Bilingual Academic Website — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Set up a bilingual (EN/ZH) academic website using Hugo + HugoBlox Academic theme, deployed to GitHub Pages.

**Architecture:** Hugo static site with HugoBlox Academic theme pulled via Go modules. Config split across `config/_default/`. Content organized in `content/en/` and `content/zh/` for bilingual pages, with `content/publications/` shared. GitHub Actions builds and deploys.

**Tech Stack:** Hugo Extended, Go, HugoBlox Academic theme (kit/modules/blox), GitHub Actions, academic-file-converter (Python)

---

## Task 0: Install Prerequisites

**Files:** None (system setup)

- [ ] **Step 1: Install Go**

Download and install Go from https://go.dev/dl/ (Windows amd64 installer). After install, verify:

```bash
go version
```

Expected: `go version go1.23.x windows/amd64` (or newer)

- [ ] **Step 2: Install Hugo Extended**

Install via Go (simplest on Windows):

```bash
go install github.com/gohugoio/hugo@latest
```

Or download the `hugo_extended_*_windows-amd64.zip` from https://github.com/gohugoio/hugo/releases, extract `hugo.exe`, and add to PATH. Verify:

```bash
hugo version
```

Expected: output containing `hugo v0.1xx.x...+extended` (the `+extended` part is critical)

- [ ] **Step 3: Install academic-file-converter**

```bash
pip install -U academic
```

Verify:

```bash
academic --version
```

---

## Task 1: Hugo Project Initialization

**Files:**
- Remove: `index.html` (old placeholder)
- Create: `go.mod`
- Create: `config/_default/hugo.yaml`
- Create: `config/_default/module.yaml`
- Create: `.gitignore`
- Create: `hugoblox.yaml`

- [ ] **Step 1: Remove the old placeholder**

```bash
cd c:/Users/jimmy/Workspace/Personal/Website
rm index.html
```

- [ ] **Step 2: Initialize Go module**

```bash
hugo mod init github.com/jmluu/jmluu.github.io
```

This creates `go.mod`. Then fetch the HugoBlox theme modules:

```bash
hugo mod get github.com/HugoBlox/kit/modules/blox@latest
```

This updates `go.mod` and creates `go.sum`.

- [ ] **Step 3: Create `config/_default/module.yaml`**

```yaml
imports:
  - path: github.com/HugoBlox/kit/modules/blox
```

- [ ] **Step 4: Create `config/_default/hugo.yaml`**

```yaml
title: "Jimmy Luu"
baseURL: "https://jmluu.github.io/"

defaultContentLanguage: en
hasCJKLanguage: true
defaultContentLanguageInSubdir: false
removePathAccents: true

build:
  writeStats: true
enableGitInfo: false
summaryLength: 30
pagination:
  pagerSize: 10
enableEmoji: true
enableRobotsTXT: true
footnotereturnlinkcontents: <sup>^</sup>
disableAliases: true

outputs:
  home: [HTML, RSS, headers, redirects, backlinks]
  section: [HTML, RSS]

imaging:
  resampleFilter: lanczos
  quality: 90
  anchor: smart

timeout: 600000

taxonomies:
  author: authors
  tag: tags
  publication_type: publication_types

markup:
  _merge: deep
security:
  _merge: deep
sitemap:
  _merge: deep
```

- [ ] **Step 5: Create `hugoblox.yaml`**

```yaml
hugo:
  version: "0.140.0"
template:
  version: "2025.1.0"
```

- [ ] **Step 6: Create `.gitignore`**

```
public/
resources/
.hugo_build.lock
node_modules/
```

- [ ] **Step 7: Verify Hugo builds without errors**

```bash
hugo server
```

Expected: Site starts at `http://localhost:1313` without build errors. It will be mostly empty — that's expected at this stage. Press Ctrl+C to stop.

- [ ] **Step 8: Commit**

```bash
git add -A
git commit -m "feat: initialize Hugo project with HugoBlox Academic theme"
```

---

## Task 2: Bilingual Language Configuration

**Files:**
- Create: `config/_default/languages.yaml`
- Create: `config/_default/menus.yaml`
- Create: `config/_default/menus.zh.yaml`
- Create: `config/_default/params.yaml`

- [ ] **Step 1: Create `config/_default/languages.yaml`**

```yaml
en:
  locale: en-us
  contentDir: content/en
  title: "Jimmy Luu"
  params:
    description: "Academic website of Jimmy Luu"

zh:
  locale: zh-Hans
  contentDir: content/zh
  title: "Jimmy Luu"
  params:
    description: "Jimmy Luu 的学术主页"
```

- [ ] **Step 2: Create `config/_default/menus.yaml`** (English navigation)

```yaml
main:
  - name: Home
    url: /
    weight: 10
  - name: Publications
    url: /publications/
    weight: 20
  - name: Projects
    url: /projects/
    weight: 30
  - name: Events
    url: /events/
    weight: 40
  - name: Blog
    url: /blog/
    weight: 50
```

- [ ] **Step 3: Create `config/_default/menus.zh.yaml`** (Chinese navigation)

```yaml
main:
  - name: 主页
    url: /
    weight: 10
  - name: 论文
    url: /publications/
    weight: 20
  - name: 项目
    url: /projects/
    weight: 30
  - name: 活动
    url: /events/
    weight: 40
  - name: 博客
    url: /blog/
    weight: 50
```

- [ ] **Step 4: Create `config/_default/params.yaml`**

```yaml
marketing:
  seo:
    site_type: Person
    local_business_type: ''
    org_name: ''
    description: "Academic website of Jimmy Luu"

appearance:
  mode: system
  color: blue

header:
  navbar:
    enable: true
    blox: "navbar"
    show_search: true
    show_theme: true
    show_language: true

footer:
  text: ""
  copyright:
    notice: "© {year} Jimmy Luu. All rights reserved."
```

- [ ] **Step 5: Verify Hugo builds with language config**

```bash
hugo server
```

Expected: Builds without errors. Language switcher should appear in navbar.

- [ ] **Step 6: Commit**

```bash
git add config/_default/languages.yaml config/_default/menus.yaml config/_default/menus.zh.yaml config/_default/params.yaml
git commit -m "feat: add bilingual language configuration (en/zh)"
```

---

## Task 3: Author Profile & Homepage

**Files:**
- Create: `data/authors/me.yaml`
- Create: `content/en/_index.md`
- Create: `content/zh/_index.md`
- Create: `assets/media/` directory (for profile photo and CV)

- [ ] **Step 1: Create `data/authors/me.yaml`** (placeholder — user fills in real data later)

```yaml
schema: hugoblox/author/v1
slug: me
is_owner: true

name:
  display: "Jimmy Luu"
  given: "Jimmy"
  family: "Luu"

role: "Researcher"

bio: |
  Your bio goes here. Describe your research interests and background.

affiliations:
  - name: "Your University"
    url: ""

links:
  - icon: at-symbol
    url: "mailto:your-email@example.com"
    label: E-mail Me
  - icon: brands/github
    url: https://github.com/jmluu

interests:
  - "Interest A"
  - "Interest B"
  - "Interest C"

education:
  - degree: "PhD in Your Field"
    institution: "Your University"
    start: "2020-09-01"
    end: ""
    summary: ""

experience:
  - role: "Your Role"
    org: "Your Organization"
    start: "2020-01-01"
    summary: ""
```

- [ ] **Step 2: Create `content/en/_index.md`** (English homepage)

```yaml
---
title: ""
date: 2026-03-31
type: landing
design:
  spacing: "6rem"

sections:
  - block: resume-biography-3
    content:
      username: me
      button:
        text: Download CV
        url: uploads/resume.pdf
    design:
      background:
        color: white

  - block: collection
    id: papers
    content:
      title: Recent Publications
      filters:
        folders:
          - publications
    design:
      view: citation

  - block: collection
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - projects
    design:
      view: article-grid
      columns: 2

  - block: collection
    id: talks
    content:
      title: Recent Events & Talks
      filters:
        folders:
          - events
    design:
      view: card

  - block: collection
    id: posts
    content:
      title: Recent Posts
      filters:
        folders:
          - blog
    design:
      view: compact
---
```

- [ ] **Step 3: Create `content/zh/_index.md`** (Chinese homepage)

```yaml
---
title: ""
date: 2026-03-31
type: landing
design:
  spacing: "6rem"

sections:
  - block: resume-biography-3
    content:
      username: me
      button:
        text: 下载简历
        url: uploads/resume.pdf
    design:
      background:
        color: white

  - block: collection
    id: papers
    content:
      title: 近期论文
      filters:
        folders:
          - publications
    design:
      view: citation

  - block: collection
    id: projects
    content:
      title: 项目
      filters:
        folders:
          - projects
    design:
      view: article-grid
      columns: 2

  - block: collection
    id: talks
    content:
      title: 近期活动与报告
      filters:
        folders:
          - events
    design:
      view: card

  - block: collection
    id: posts
    content:
      title: 近期博客
      filters:
        folders:
          - blog
    design:
      view: compact
---
```

- [ ] **Step 4: Create placeholder directories**

```bash
mkdir -p assets/media
mkdir -p static/uploads
```

Place a profile photo at `assets/media/avatar.jpg` and a CV at `static/uploads/resume.pdf` later.

- [ ] **Step 5: Verify homepage renders**

```bash
hugo server
```

Expected: Homepage loads with biography section and empty collection sections. Both `/` (English) and `/zh/` (Chinese) should render.

- [ ] **Step 6: Commit**

```bash
git add data/authors/me.yaml content/en/_index.md content/zh/_index.md assets/ static/
git commit -m "feat: add author profile and bilingual homepage"
```

---

## Task 4: Publications Section with BibTeX Support

**Files:**
- Create: `papers.bib` (sample BibTeX)
- Create: `content/publications/_index.md`

- [ ] **Step 1: Create `content/publications/_index.md`** (section listing page)

```yaml
---
title: Publications
---
```

- [ ] **Step 2: Create a sample `papers.bib`** in the repo root

```bibtex
@article{luu2024example,
  title = {An Example Research Paper Title},
  author = {Luu, Jimmy and Collaborator, A.},
  journal = {Journal of Example Studies},
  year = {2024},
  volume = {1},
  pages = {1--10},
  doi = {10.1234/example.2024},
  abstract = {This is a placeholder abstract for testing the BibTeX import workflow. Replace this file with your actual papers.bib.}
}
```

- [ ] **Step 3: Run BibTeX import**

```bash
cd c:/Users/jimmy/Workspace/Personal/Website
academic import papers.bib content/publications/ --compact
```

Expected: Creates `content/publications/luu-2024-example/index.md` with frontmatter extracted from the BibTeX entry.

- [ ] **Step 4: Inspect the generated file**

Open the generated `index.md` and verify it has correct title, authors, date, journal, and abstract in the frontmatter.

- [ ] **Step 5: Verify publications page renders**

```bash
hugo server
```

Expected: Visit `http://localhost:1313/publications/` — the sample paper should appear. Both EN and ZH versions should show the same publication.

- [ ] **Step 6: Commit**

```bash
git add papers.bib content/publications/
git commit -m "feat: add publications section with BibTeX import support"
```

---

## Task 5: Projects Section (Bilingual)

**Files:**
- Create: `content/en/projects/_index.md`
- Create: `content/en/projects/example-project/index.md`
- Create: `content/zh/projects/_index.md`
- Create: `content/zh/projects/example-project/index.md`

- [ ] **Step 1: Create English project listing and sample**

`content/en/projects/_index.md`:

```yaml
---
title: Projects
---
```

`content/en/projects/example-project/index.md`:

```yaml
---
title: "Example Project"
summary: "A placeholder project. Replace with your real projects."
date: 2024-01-01
tags:
  - Example
---

Describe your project here.
```

- [ ] **Step 2: Create Chinese project listing and sample**

`content/zh/projects/_index.md`:

```yaml
---
title: 项目
---
```

`content/zh/projects/example-project/index.md`:

```yaml
---
title: "示例项目"
summary: "这是一个占位项目，请替换为您的真实项目。"
date: 2024-01-01
tags:
  - 示例
---

在这里描述您的项目。
```

- [ ] **Step 3: Verify projects render in both languages**

```bash
hugo server
```

Expected: `/projects/` shows English project, `/zh/projects/` shows Chinese project.

- [ ] **Step 4: Commit**

```bash
git add content/en/projects/ content/zh/projects/
git commit -m "feat: add bilingual projects section with example"
```

---

## Task 6: Events/Talks Section (Bilingual)

**Files:**
- Create: `content/en/events/_index.md`
- Create: `content/en/events/example-talk/index.md`
- Create: `content/zh/events/_index.md`
- Create: `content/zh/events/example-talk/index.md`

- [ ] **Step 1: Create English events listing and sample**

`content/en/events/_index.md`:

```yaml
---
title: Events & Talks
---
```

`content/en/events/example-talk/index.md`:

```yaml
---
title: "Example Talk"
event_name: "Example Conference 2024"
event_url: ""
location: "City, Country"
summary: "A placeholder talk. Replace with your real talks."
date: 2024-06-01
event_start: "2024-06-01T09:00:00Z"
event_end: "2024-06-01T10:00:00Z"
authors:
  - me
tags:
  - Example
---

Talk description goes here.
```

- [ ] **Step 2: Create Chinese events listing and sample**

`content/zh/events/_index.md`:

```yaml
---
title: 活动与报告
---
```

`content/zh/events/example-talk/index.md`:

```yaml
---
title: "示例报告"
event_name: "示例会议 2024"
event_url: ""
location: "城市, 国家"
summary: "这是一个占位报告，请替换为您的真实报告。"
date: 2024-06-01
event_start: "2024-06-01T09:00:00Z"
event_end: "2024-06-01T10:00:00Z"
authors:
  - me
tags:
  - 示例
---

在这里描述您的报告内容。
```

- [ ] **Step 3: Verify events render in both languages**

```bash
hugo server
```

Expected: `/events/` shows English event, `/zh/events/` shows Chinese event.

- [ ] **Step 4: Commit**

```bash
git add content/en/events/ content/zh/events/
git commit -m "feat: add bilingual events/talks section with example"
```

---

## Task 7: Blog Section (Bilingual)

**Files:**
- Create: `content/en/blog/_index.md`
- Create: `content/en/blog/hello-world/index.md`
- Create: `content/zh/blog/_index.md`
- Create: `content/zh/blog/hello-world/index.md`

- [ ] **Step 1: Create English blog listing and sample post**

`content/en/blog/_index.md`:

```yaml
---
title: Blog
---
```

`content/en/blog/hello-world/index.md`:

```yaml
---
title: "Hello World"
summary: "My first blog post."
date: 2026-03-31
authors:
  - me
tags:
  - General
---

Welcome to my blog! This is a sample post.
```

- [ ] **Step 2: Create Chinese blog listing and sample post**

`content/zh/blog/_index.md`:

```yaml
---
title: 博客
---
```

`content/zh/blog/hello-world/index.md`:

```yaml
---
title: "你好世界"
summary: "我的第一篇博客文章。"
date: 2026-03-31
authors:
  - me
tags:
  - 一般
---

欢迎来到我的博客！这是一篇示例文章。
```

- [ ] **Step 3: Verify blog renders in both languages**

```bash
hugo server
```

Expected: `/blog/` shows English post, `/zh/blog/` shows Chinese post.

- [ ] **Step 4: Commit**

```bash
git add content/en/blog/ content/zh/blog/
git commit -m "feat: add bilingual blog section with sample post"
```

---

## Task 8: GitHub Actions Deployment

**Files:**
- Create: `.github/workflows/deploy.yml`

- [ ] **Step 1: Create `.github/workflows/deploy.yml`**

```yaml
name: Build and deploy

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.147.0
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.23"

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Install Hugo
        run: |
          wget -O hugo.tar.gz "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          tar -xzf hugo.tar.gz -C "${HOME}/.local/bin"
          rm hugo.tar.gz
          echo "${HOME}/.local/bin" >> "${GITHUB_PATH}"

      - name: Build site
        run: |
          hugo build --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 2: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "feat: add GitHub Actions workflow for Hugo deployment"
```

---

## Task 9: Configure GitHub Pages & First Deploy

- [ ] **Step 1: Configure GitHub Pages source**

Go to your repo on GitHub: `https://github.com/jmluu/jmluu.github.io/settings/pages`

Under **Source**, select **GitHub Actions** (not "Deploy from a branch").

- [ ] **Step 2: Push all commits to GitHub**

```bash
git push origin main
```

- [ ] **Step 3: Monitor the deployment**

Go to `https://github.com/jmluu/jmluu.github.io/actions` and watch the workflow run. It should complete in 1-2 minutes.

- [ ] **Step 4: Verify the live site**

Visit `https://jmluu.github.io` — the site should load with:
- English homepage with biography section
- Language switcher in navbar
- Navigation links to Publications, Projects, Events, Blog
- `/zh/` shows Chinese version

If the build fails, check the Actions log for error details.

- [ ] **Step 5: Commit any fixes if needed**

---

## Task 10: Local Development Verification

- [ ] **Step 1: Run local server**

```bash
cd c:/Users/jimmy/Workspace/Personal/Website
hugo server
```

Expected: Site at `http://localhost:1313` with live reload.

- [ ] **Step 2: Test language switching**

- Visit `http://localhost:1313` (English)
- Click the language switcher → should go to `http://localhost:1313/zh/` (Chinese)
- Verify all sections appear in both languages
- Verify publications appear on both language versions

- [ ] **Step 3: Test content editing workflow**

Edit `content/en/blog/hello-world/index.md` — change the title. The browser should auto-refresh and show the change instantly.

---

## Summary: What to fill in after setup

After completing all tasks, you'll have a working site with placeholder content. Replace these with your real data:

| File | What to fill in |
|------|----------------|
| `data/authors/me.yaml` | Real name, role, affiliation, bio, education, experience, social links |
| `assets/media/avatar.jpg` | Your profile photo |
| `static/uploads/resume.pdf` | Your CV |
| `papers.bib` | Your real BibTeX file, then re-run `academic import` |
| `content/en/projects/` | Your real projects (remove example) |
| `content/zh/projects/` | Chinese translations of projects |
| `content/en/events/` | Your real talks/events (remove example) |
| `content/zh/events/` | Chinese translations of events |
| `content/en/blog/` | Your blog posts |
| `content/zh/blog/` | Chinese blog posts |

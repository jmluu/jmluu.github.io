# Jimmy Luu - Bilingual Academic Website

Hugo static site using the HugoBlox Academic theme, deployed to GitHub Pages at https://jmluu.github.io.

## Architecture

- **Framework:** Hugo Extended + HugoBlox Academic theme (via Go modules)
- **Languages:** English (default) and Chinese (zh-Hans)
- **Styling:** TailwindCSS v4 (required by HugoBlox, installed via npm)
- **Deployment:** GitHub Actions builds and deploys to GitHub Pages

## Project Structure

```
config/_default/       Hugo configuration (split files)
  hugo.yaml            Core Hugo settings
  module.yaml          HugoBlox theme module import
  languages.yaml       EN/ZH language definitions
  menus.yaml           English navigation
  menus.zh.yaml        Chinese navigation
  params.yaml          Theme params (SEO, navbar, footer)
content/en/            English content (blog, projects, events)
content/zh/            Chinese content (mirrors EN structure)
content/publications/  Shared publications (both languages)
data/authors/me.yaml   Author profile (name, bio, links, education)
assets/media/          Profile photo (avatar.jpg)
static/uploads/        CV file (resume.pdf)
papers.bib             BibTeX source for publications
.github/workflows/     CI/CD pipeline
```

## How to Fill In Your Data

### Author profile

Edit `data/authors/me.yaml` — replace all placeholder values (name, role, bio, affiliations, links, interests, education, experience).

Place your profile photo at `assets/media/avatar.jpg` and CV at `static/uploads/resume.pdf`.

### Publications

1. Replace `papers.bib` with your real BibTeX file
2. Run the import:
   ```
   academic import papers.bib content/publications/ --compact
   ```
   If `academic` is not on PATH, use: `python -c "from academic import cli; import sys; sys.argv = ['academic', 'import', 'papers.bib', 'content/publications/', '--compact']; cli.main()"`
3. Each paper gets a folder under `content/publications/` with `index.md` and `cite.bib`

### Projects

- English: create a folder under `content/en/projects/your-project/index.md`
- Chinese: create matching folder under `content/zh/projects/your-project/index.md`
- See `content/en/projects/example-project/index.md` for the frontmatter format

### Events / Talks

- English: `content/en/events/your-talk/index.md`
- Chinese: `content/zh/events/your-talk/index.md`
- See `content/en/events/example-talk/index.md` for the frontmatter format

### Blog posts

- English: `content/en/blog/your-post/index.md`
- Chinese: `content/zh/blog/your-post/index.md`

## Local Preview

Requires: Go, Hugo Extended, Node.js (for TailwindCSS).

```bash
npm install          # first time only
hugo server          # starts at http://localhost:1313
```

If Go/Hugo are not on PATH in your terminal, add them:
```bash
export PATH="/c/Program Files/Go/bin:$HOME/go/bin:$PATH"
```

English site: http://localhost:1313  
Chinese site: http://localhost:1313/zh/

## Deploy and Publish

1. **Configure GitHub Pages** (one-time): Go to https://github.com/jmluu/jmluu.github.io/settings/pages and set Source to **GitHub Actions**
2. **Push to main** — the GitHub Actions workflow automatically builds and deploys:
   ```bash
   git push origin main
   ```
3. **Monitor:** Check https://github.com/jmluu/jmluu.github.io/actions for build status
4. **Live site:** https://jmluu.github.io

## Build Commands

```bash
hugo build                    # production build to public/
hugo build --gc --minify      # optimized production build
hugo server                   # dev server with live reload
```

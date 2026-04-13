# Infrastructure Setup — sgouri.dev

## Overview

| Component | Service | Details |
|-----------|---------|---------|
| Domain | Cloudflare Registrar | `sgouri.dev` — $12.20/year |
| DNS | Cloudflare | A records + CNAME |
| Hosting | GitHub Pages | Via GitHub Actions |
| SSL | GitHub Pages | Auto-provisioned (Enforce HTTPS enabled) |
| CI/CD | GitHub Actions | `.github/workflows/deploy.yml` |
| Static Site Generator | Hugo v0.160.1 | Extended edition |
| Theme | PaperMod | Git submodule |

---

## Accounts

| Asset | Value |
|-------|-------|
| GitHub Account | `sgouri-dev` |
| GitHub Email | `sgouri.dev@gmail.com` |
| Repo | `sgouri-dev/sgouri.dev` |
| Repo URL | https://github.com/sgouri-dev/sgouri.dev |
| Live URL | https://sgouri.dev |

---

## GitHub Setup

### Repository

- Created as **public** repo: `sgouri-dev/sgouri.dev`
- Local git config scoped to this repo only:
  ```
  git config user.name "sgouri-dev"
  git config user.email "sgouri.dev@gmail.com"
  ```
- `gh` CLI authenticated as `sgouri-dev` (separate from `gsalpha-ai-labs`)
  ```
  gh auth login  # select GitHub.com, HTTPS, web browser
  ```

### GitHub Pages Configuration

- **Settings > Pages > Source**: GitHub Actions
- **Custom domain**: `sgouri.dev`
- **Enforce HTTPS**: Enabled
- SSL certificate auto-provisioned by GitHub after DNS verification

### GitHub Actions Workflow

File: `.github/workflows/deploy.yml`

- Triggers on push to `main`
- Installs Hugo extended v0.160.1
- Checks out repo with submodules (for PaperMod theme)
- Builds with `hugo --gc --minify`
- Deploys to GitHub Pages via `actions/deploy-pages@v4`

Pipeline: `git push → GitHub Actions builds Hugo → Deploys to GitHub Pages → Live at sgouri.dev`

---

## DNS Setup (Cloudflare)

### A Records (GitHub Pages IPs)

| Type | Name | Content | Proxy Status | TTL |
|------|------|---------|-------------|-----|
| A | `sgouri.dev` | `185.199.108.153` | DNS only | Auto |
| A | `sgouri.dev` | `185.199.109.153` | DNS only | Auto |
| A | `sgouri.dev` | `185.199.110.153` | DNS only | Auto |
| A | `sgouri.dev` | `185.199.111.153` | DNS only | Auto |

### CNAME Record (www redirect)

| Type | Name | Content | Proxy Status | TTL |
|------|------|---------|-------------|-----|
| CNAME | `www` | `sgouri-dev.github.io` | DNS only | Auto |

### Important Notes

- **Proxy status must be "DNS only"** (gray cloud, not orange) — GitHub Pages requires direct DNS for SSL certificate provisioning
- DNS propagation takes a few minutes after adding records
- SSL certificate is auto-issued by GitHub after DNS check passes

---

## Hugo Site Structure

```
sgouri.dev/
├── hugo.yaml                        # Site config
├── .github/workflows/deploy.yml     # CI/CD
├── .gitignore                       # Excludes public/, resources/, .hugo_build.lock
├── assets/css/extended/custom.css   # Custom theme overrides
├── content/
│   ├── about.md                     # About page
│   └── articles/
│       ├── _index.md                # Articles list config
│       ├── dreamers-paradise/index.md
│       ├── markdown-new-source-code/index.md
│       ├── security-age-of-agency/index.md
│       ├── right-sizing-intelligence/index.md
│       └── agents-are-ready/index.md
├── layouts/partials/
│   ├── extend_footer.html           # Hooks LinkedIn CTA into articles
│   └── linkedin-cta.html            # "Discuss on LinkedIn" button
├── static/CNAME                     # Custom domain for GitHub Pages
├── docs/                            # Documentation (this file)
└── themes/PaperMod/                 # Git submodule
```

---

## Theme & Design

- **Theme**: PaperMod (git submodule)
- **Mode**: Light only (no dark mode, toggle disabled)
- **Custom CSS**: `assets/css/extended/custom.css`
  - Fonts: Playfair Display (headings) + Inter (body)
  - Warm cream background (`#f3ede4`)
  - Brown-toned body text (`#3d2b1a`)
  - Orange accent color (`#c45e2c`)
  - Editorial list layout (no cards)
  - LinkedIn CTA styled block

---

## Deployment Workflow

```
Edit .md files locally
  → git add & commit
    → git push origin main
      → GitHub Actions: Hugo build
        → Deploy to GitHub Pages
          → Live at https://sgouri.dev
```

---

## Local Development

```bash
# Start dev server
hugo server -D --port 1313

# Build locally
hugo --gc --minify

# Preview at http://localhost:1313/
```

Requires Hugo extended v0.160.1+ installed via `brew install hugo`.

---

## Content & Branding

- **Site title**: "Building Intelligence"
- **Subtitle**: "An engineer's perspective on AI, agents, and autonomous systems — from imagination to execution."
- **About page**: Short, no company affiliation — "Engineer by practice. Builder by nature."
- **Social icons**: Disabled for now (LinkedIn, GitHub commented out in hugo.yaml)
- **X/Twitter**: @gouriswamy1 — bio aligns with site voice

### Articles (5 stubs, content to be added from LinkedIn)

| # | Title | Slug | Date |
|---|-------|------|------|
| 1 | Dreamer's Paradise: AI from Imagination to Execution | `dreamers-paradise` | 2025-12-13 |
| 2 | Markdown Is the New Source Code | `markdown-new-source-code` | 2026-01-15 |
| 3 | Security in the Age of Agency | `security-age-of-agency` | 2026-02-15 |
| 4 | Right-Sizing Intelligence: From Cloudonomics to Tokenomics | `right-sizing-intelligence` | 2026-03-15 |
| 5 | Agents Are Ready. Is Your Data? | `agents-are-ready` | 2026-04-12 |

### Adding a new article

```bash
# Create article folder
mkdir -p content/articles/my-new-article

# Add index.md with front matter + banner.png (1200x627px)
# git add, commit, push — auto-deploys
```

---

## Setup Date

April 12, 2026

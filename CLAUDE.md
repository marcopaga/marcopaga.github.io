# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal website/blog built with Hugo static site generator. The site is deployed to GitHub Pages via GitHub Actions and includes automated performance monitoring with sitespeed.io.

## Development Commands

**Local development:**
```bash
hugo server -D
```
Access the site at http://localhost:1313

**Build for production:**
```bash
hugo --minify --baseURL="https://marco-paga.eu"
```

**DevContainer:**
The repository includes a DevContainer configuration at `.devcontainer/devcontainer.json` that provides a ready-to-use Hugo environment.

## Architecture

**Theme:**
- Uses the `coder-portfolio` theme, maintained as a Git submodule
- Submodule URL: https://github.com/marcopaga/hugo-coder-portfolio
- Located at: `themes/coder-portfolio/`
- Theme customizations via `config.toml` and override files in `layouts/`

**Content Structure:**
- `content/entries/`: Blog posts and articles (markdown files)
- `content/about-me.md`: About page
- `content/articles.md`: Articles listing page
- `content/impressum.md`: Imprint/legal page
- `static/`: Static assets (images, etc.)
- `layouts/`: Custom layout overrides for the theme
  - `layouts/shortcodes/tableOfContents.html`: Custom ToC shortcode

**Configuration:**
- `config.toml`: Main Hugo configuration
  - Site settings (baseURL, title, author)
  - Theme configuration and parameters
  - Menu structure
  - Social media links
  - Language settings (currently English only)

## Deployment

**GitHub Actions workflow (`.github/workflows/.github-pages.yml`):**
- Triggers on pushes to `main` and `gh-action` branches
- Two jobs:
  1. `deploy`: Builds and deploys to GitHub Pages using `peaceiris/actions-gh-pages`
  2. `run-sitespeed`: Performance testing with sitespeed.io

**Test Workflow (`.github/workflows/test.yml`):**
- Runs on all PRs and pushes to main
- Two jobs:
  1. `build-test`: Validates Hugo build succeeds and uploads artifact
  2. `content-validation`: Checks critical pages exist and have valid HTML structure
- Required to pass before PRs can be merged

**Performance Monitoring:**
- Sitespeed.io runs on every push/PR
- Configuration: `.github/sitespeed-budget.json`
- Test URLs: `.github/sitespeed-urls.txt`
- Budgets include: total requests (max 100), transfer size (max 610KB), no third-party requests, performance score targets, and CO2 emissions limits

## Automated Dependency Management

**Renovate Bot (`renovate.json`):**
- Runs weekly (Mondays before 6am CET)
- Auto-merges safe updates (patches and minors) after tests pass
- Requires manual review for major updates and Hugo version changes
- Groups related updates together
- Rate limits: max 5 PRs concurrent, 2 per hour

**Auto-merge rules:**
- ✅ GitHub Actions patch/minor updates
- ✅ Docker image patches
- ⚠️ Manual review for major updates
- ⚠️ Never auto-merge Hugo version updates (breaking changes common)

## Key Considerations

When modifying the theme, prefer creating override files in `layouts/` rather than editing the submodule directly to maintain clean separation and easier updates.

The site emphasizes performance and sustainability (zero third-party requests, CO2 tracking), so keep this in mind when adding new features or content.

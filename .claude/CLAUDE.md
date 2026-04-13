# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Jekyll site documenting conversations with Claude about AI, identity, and institutional ethics. Organized as an alphabetical series: **A**pplying to Anthropic, **B**ias, **C**ontext, etc. Conversation summaries live in top-level directories as numbered HTML files.

## Build & Development

**First-time setup:**
```bash
npm install       # installs @ouroboros-consulting/agentic-design from ../agentic-design
bundle install    # installs Jekyll + gems
```

**Local dev:**
```bash
npm run dev       # runs: jekyll serve → http://localhost:4000
```

**Static build:**
```bash
npm run build     # runs: jekyll build → _site/
```

**Ruby environment:** Use rbenv with Ruby 3.3.x. Homebrew Ruby 4.x has a Bundler path bug incompatible with Jekyll.

## Design System

Styles come from the shared npm package `@ouroboros-consulting/agentic-design` (source at `../agentic-design`). Dependency is `file:../agentic-design` — local path, not the npm registry.

- Entry point: `assets/css/main.scss` — imports `@ouroboros-consulting/agentic-design/scss/index`
- Jekyll resolves the import via `sass: load_paths: [node_modules]` in `_config.yml`
- Do **not** use the `~` prefix in SCSS imports (webpack convention, not native Sass)

## CI / GitHub Actions

**No workflow file yet for this repo.** The OUROBOROS-Consulting.github.io repo has a working pattern at `.github/workflows/jekyll-gh-pages.yml` — copy and adapt it. Key requirement: checkout `OUROBOROS-Consulting/agentic-design` as a sibling (`path: agentic-design`) before running `npm install`, so the `file:../agentic-design` dependency resolves correctly in CI.

## Known Active Issue

CI build for `OUROBOROS-Consulting/OUROBOROS.github.io` is still failing after workflow rewrite. Likely cause: `file:` protocol symlink resolution in GitHub Actions CI. Next debugging step: get the exact new Actions error log to confirm whether `node_modules/@ouroboros-consulting/agentic-design` symlink is created correctly.

## Site Structure

- **Conversation directories:** e.g., `A is for "Applying to Anthropic"/` — numbered HTML files
- `_config.yml` — Jekyll config; `sass: load_paths: [node_modules]` is required
- `assets/css/main.scss` — SCSS entry point
- `package.json` — npm dependency on design system
- `Gemfile` — Jekyll 4.3, minima, webrick, jekyll-feed

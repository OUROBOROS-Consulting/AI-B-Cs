# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Jekyll-based site documenting conversations with Claude about AI, identity, and institutional ethics. Organized as an alphabetical series: **A**pplying to Anthropic, **B**ias, **C**ontext, etc. Currently active conversations in top-level directories with HTML conversation summaries. Collections defined in `_config.yml` but not yet populated.

## Build & Development

**Setup (first time only):**

```bash
# Install Node.js dependencies (includes design system package)
npm install

# Install Ruby/Jekyll dependencies (if using Bundler)
bundle install
```

**Local development:**

```bash
npm run dev
# Runs: jekyll serve
# Serves at http://localhost:4000
```

**Static build:**

```bash
npm run build
# Runs: jekyll build
# Outputs to _site/
```

**Dependencies:**
- Ruby + Jekyll
- Node.js + npm (for design system package)
- `@OUROBOROS-Consulting/agentic-design` (shared design system)

## Site Structure

- **Conversation directories:** Top-level directories (e.g., `A is for "Applying to Anthropic"`) contain numbered HTML files with conversation summaries
- **Design system:** Imported from npm package `@OUROBOROS-Consulting/agentic-design`
- **Config:** `_config.yml` handles collections routing and build settings
- **Home:** `index.md` (Jekyll home layout)

## Content Format

HTML conversation summaries use consistent styling from `@OUROBOROS-Consulting/agentic-design`:
- Dark theme: `#141414` background, `#E8E4DC` text, `#C9A84C` accents
- System font stack for accessibility
- Semantic structure: h1/h2, paragraphs, lists

## Key Files

- `_config.yml` — Jekyll config; defines collections and SCSS load paths
- `index.md` — Home page
- `README.md` — AI-B-C conversation series index
- `assets/css/main.scss` — Entry point for design system styles
- `package.json` — Node dependencies (design system package)
- Conversation directories — Named by topic letter; contain numbered HTML files

## Notes

- Site uses accessible color contrast and responsive viewport meta
- Styles come from centralized npm package, not local CSS files
- Git history shows workflow: MD→HTML conversions; cleanup of old workflows

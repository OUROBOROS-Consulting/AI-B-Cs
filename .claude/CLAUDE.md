# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

Jekyll-based site documenting conversations with Claude about AI, identity, and institutional ethics. Organized as an alphabetical series: **A**pplying to Anthropic, **B**ias, **C**ontext, etc. Currently active conversations in top-level directories with HTML conversation summaries. Collections defined in `_config.yml` but not yet populated.

## Build & Development

**Local development:**
```bash
jekyll serve
# Serves at http://localhost:4000
```

**Static build:**
```bash
jekyll build
# Outputs to _site/
```

**Dependencies:**
- Ruby + Jekyll (no Gemfile yet; uses system Jekyll)
- No Node/npm dependencies

## Site Structure

- **Conversation directories:** Top-level directories (e.g., `A is for "Applying to Anthropic"`) contain numbered HTML files with conversation summaries
- **Jekyll collections** (defined in `_config.yml` but not yet used): services, essays, projects, notes, linkedin (posts), resources, case_studies, psas
- **Config:** `_config.yml` handles collections routing and excludes
- **Home:** `index.md` (Jekyll home layout with hero/about sections)

## Content Format

HTML conversation summaries use consistent styling:
- Dark theme: `#141414` background, `#E8E4DC` text, `#C9A84C` accents
- System font stack for accessibility
- Semantic structure: h1/h2, paragraphs, lists

## Key Files

- `_config.yml` — Jekyll config; defines all collections and build excludes
- `index.md` — Home page with consulting pitch and stats
- `README.md` — AI-B-C conversation series index
- Conversation directories — Named by topic letter; contain numbered HTML files

## Notes

- Site uses accessible color contrast and responsive viewport meta
- No CSS files yet; styles embedded in HTML
- Git history shows workflow: MD→HTML conversions; cleanup of old workflows

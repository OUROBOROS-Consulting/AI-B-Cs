# Homepage Redesign — AI A–Z

**Date:** 2026-04-25  
**Status:** Approved  
**Audience:** Public editorial — shareable with Anthropic, journalists, researchers

---

## Goal

Replace the current bland hero + generic series grid with a high-impact "Monolith" layout that signals credibility and editorial intent at first glance.

---

## Architecture

Three files change:

| File | Change |
|------|--------|
| `_layouts/default.html` | Add Google Fonts, fix nav wordmark, fix CSS vars |
| `_layouts/home.html` | Full rewrite — hero, alphabet index, series cards |
| `index.md` | Update front matter — fix tagline (currently uses consulting site copy) |

No new files. No changes to conversation page layouts or design system.

---

## Sections

### Nav

- Left: `AI A–Z` in Cormorant SC, gold — replaces cryptic `M`
- Right: `Apostolos Stamenos · OUROBOROS Consulting` in Inter, muted
- Sticky, `background: var(--surface)`, 1px bottom border

### Hero

Two-column grid:

- **Left col:** Giant ghost `A–Z` in Cormorant SC (~12–14rem), gold at 7% opacity. Purely decorative — `user-select: none`, `pointer-events: none`.
- **Right col (stacked):**
  1. Eyebrow: `Documented AI Conversations` — Inter, 0.65rem, gold-dim, 0.3em letter-spacing
  2. Title: `A Public Record / of Thinking / With Claude` — Cormorant SC, ~2.5rem, `With Claude` in orange
  3. Orange rule (2rem wide, 1px, 50% opacity)
  4. Lede: Lora italic, ~0.95rem, text-dim — *"An ongoing series of verbatim conversations — on identity, bias, ethics, and what AI institutions owe us. Twenty-six topics. One interlocutor."*
  5. Meta line: `26 chapters · In progress · Est. 2024` — Inter, 0.65rem, muted

### Alphabet Index

- 13×2 grid of single-letter tiles (all 26, A–Z)
- **Active letters** (those with content): gold border, gold-ghost background, orange dot below letter
- **Inactive letters**: no border, no background, text-muted color
- Active letters are determined dynamically from `site.static_files` — same Liquid logic as current series grid
- Section label: `The Series` — 0.6rem, 0.35em tracking, muted

### Series Cards

Grid of cards, one per active letter only (not all 26):

- `grid-template-columns: repeat(auto-fill, minmax(300px, 1fr))`
- 1px gaps, `background: var(--border)` on container (creates hairline separators)
- Each card:
  - Ghost letter (Cormorant SC, 8rem, border color) — absolute positioned top-right
  - Label: `X is for` — Inter, 0.6rem, gold-dim
  - Title: Series name — Lora, 1.15rem
  - Orange accent rule (1.5rem)
  - File list: `–` prefixed links in text-dim, 0.78rem
- Hover: background lightens, top border becomes orange, ghost letter darkens

### Typography

Load via Google Fonts CDN in `default.html`:

```
Cormorant SC: 300, 400, 600
Lora: 400, 400i, 600
Inter: 300, 400, 500
```

Replace `font-family: "Georgia", serif` with `font-family: 'Inter', sans-serif` on `html`.  
Lora used for: hero lede, series card titles.  
Cormorant SC used for: nav wordmark, hero ghost, hero title, series card ghost letters.

### Colors

Use existing CSS vars — no changes needed. Key ones:

```css
--gold: #C9A84C       /* Cormorant SC wordmark, active tiles */
--gold-dim: #7a6228   /* eyebrow, nav right, card labels */
--orange: #E8640A     /* hero title accent, hover borders, rules */
--text-dim: #888075   /* lede, card file links */
--text-muted: #444    /* inactive alpha tiles, nav right */
```

---

## Index.md Front Matter

Remove the OUROBOROS Consulting tagline. The `home.html` layout hardcodes the hero copy — `index.md` only needs:

```yaml
---
layout: home
title: null
---
```

---

## Data Requirement

`_data/series.yml` must have an entry for every letter directory that contains HTML files. Currently `D/` exists but has no `series.yml` entry — add `d: Dialogue` (or correct title) before implementation. The Liquid template should also guard against missing entries with `{% if series_name %}`.

---

## Out of Scope

- Conversation page layout (`_layouts/conversation.html` or inline styles in `.html` files)
- Design system (`agentic-design` npm package)
- CI/GitHub Actions
- Any new Jekyll plugins or data files

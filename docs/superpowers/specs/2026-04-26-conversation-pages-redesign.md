# Conversation Pages Redesign — AI A–Z

**Date:** 2026-04-26  
**Status:** Approved  
**Audience:** Internal implementation reference

---

## Goal

Upgrade all standalone conversation and background HTML pages to match the visual quality of the redesigned homepage. Bring pages into the Jekyll layout system, introduce two specialized layouts (editorial and transcript), fix the chamfered card design, and unify all transcript pages to a single card style.

---

## Architecture

Three existing files modified, four standalone HTML files converted, two new layouts added:

| File | Change |
|------|--------|
| `_layouts/default.html` | Add Google Fonts, fix nav wordmark, update nav right, set Inter on `html` |
| `_layouts/conversation.html` | NEW — extends default; page header + chamfered card content area |
| `_layouts/background.html` | NEW — extends default; editorial header with ghost letter, eyebrow, lede |
| `assets/css/conversation.css` | Fix 4-corner chamfer, two-layer border technique, unify dialogue pages to card style |
| `_data/series.yml` | Add `d: Dialogue` |
| All standalone `.html` files | Add front matter, strip html/head/body wrappers, assign layout |

No new Jekyll plugins. No changes to homepage layout or design system package.

---

## Section 1: `_layouts/default.html`

**Google Fonts** — add to `<head>`:
```
Cormorant SC: 300, 400, 600
Lora: 400, 400i, 600
Inter: 300, 400, 500
```

**Nav left:** `AI A–Z` in Cormorant SC 400, `--gold` — replaces `M`.

**Nav right:** `Apostolos Stamenos · OUROBOROS Consulting` — Inter 300, `--text-dim` — replaces the orange series label (series label moves into individual page layouts).

**`html` element:** `font-family: 'Inter', sans-serif` — replaces `"Georgia", serif`.

Everything else in `default.html` unchanged.

---

## Section 2: `_layouts/background.html`

Used for series overview / summary pages (e.g., `a/00_Background.html`).

**Front matter required by each page:**
```yaml
---
layout: background
title: "Why I Kept the Transcripts"
letter: a
lede: "I applied to Anthropic in late 2024..."
---
```

**Page structure** (inside `main` wrapper from `default.html`):

1. **Ghost letter** — `page.letter | upcase` — Cormorant SC, ~10rem, `--orange` at 10% opacity, absolute top-right, `user-select: none`, `pointer-events: none`
2. **Eyebrow** — `· A is for Applying to Anthropic` — orange pip + series name from `_data/series.yml[page.letter]`; Inter, 0.6rem, `--gold-dim`, 0.2em tracking
3. **Title** — `page.title` — Lora, ~1.8rem, `--text`
4. **Orange rule** — 2rem wide, 2px, `--orange`, full opacity
5. **Lede** — `page.lede` — Lora italic, 0.95rem, `--text-dim`; 2px left border in `--orange`, slight left padding
6. **Divider** — 1px `--border`, full width
7. **Body** — `{{ content }}` — Inter, 0.9rem, `--text-dim`, line-height 1.8; `h2`/`h3` in `--orange`, Inter 0.65rem, uppercase, 0.2em tracking
8. **Back link** — `← AI A–Z` — Inter 0.7rem, `--orange`; hover: `--text`

---

## Section 3: `_layouts/conversation.html`

Used for all conversation transcript pages.

**Front matter required by each page:**
```yaml
---
layout: conversation
title: "Why Anthropic"
letter: a
number: "01"
---
```

**Page structure** (inside `main` wrapper from `default.html`):

**Header:**
- Breadcrumb left: `← AI A–Z` — Inter 0.65rem, `--gold-dim`
- Breadcrumb right: series name from `_data/series.yml[page.letter]` — Inter 0.65rem, `--text-dim`
- Conversation number: `page.number` — Cormorant SC, 3rem, `--orange` at 15% opacity, floated right, `user-select: none`
- Title: `page.title` — Lora, 1.3rem, `--text`
- 1px `--border` bottom, 1.5rem margin below

**Content:** `{{ content }}` — chamfered card HTML; all styles from `conversation.css`

---

## Section 4: `assets/css/conversation.css`

### 4-corner chamfer

Both `.apostolos-card` and `.claude-card` use symmetric 8-point chamfer:
```css
clip-path: polygon(
  12px 0%, calc(100% - 12px) 0%,
  100% 12px, 100% calc(100% - 12px),
  calc(100% - 12px) 100%, 12px 100%,
  0% calc(100% - 12px), 0% 12px
);
```

### Two-layer speaker color border

Outer element carries clip-path and speaker color background. Inner element provides surface and margin so the outer color reads as a perimeter border:

```css
.claude-card    { background: var(--orange); }
.apostolos-card { background: var(--teal); }

.card__interior {
  background: var(--surface-2);
  margin: 1.5px;
  padding: 1rem 1.25rem;
}
```

### Speaker labels

Inter, 0.6rem, weight 700, 0.1em tracking. `--orange` for CLAUDE, `--teal` for APOSTOLOS.

### Remove `.dialogue` style

The `.dialogue` border-left class is removed. Pages using it (`a/02_Book.html`) are converted to chamfered card markup during file conversion.

---

## Section 5: Standalone file conversion

Each file receives:
1. Jekyll front matter at top
2. `<!DOCTYPE html>` through opening `<body>` tag stripped
3. Closing `</body></html>` stripped
4. Inline `<style>` blocks and `<link rel="stylesheet">` tags removed

| File | Layout | Notes |
|------|--------|-------|
| `a/00_Background.html` | `background` | Extract `title` and `lede` from existing prose for front matter |
| `a/01_Why-Anthropic.html` | `conversation` | `number: "01"` — largest file; content unchanged |
| `a/02_Book.html` | `conversation` | `number: "02"` — `.dialogue` divs converted to `.chamfered-card` + `.card__interior` markup |
| `D/01_Chat.html` | `conversation` | `number: "01"` — already references `conversation.css`; inline styles stripped |

---

## Section 6: `_data/series.yml`

Add:
```yaml
d: Dialogue
```

---

## Out of Scope

- Homepage layout (`_layouts/home.html`)
- Design system (`agentic-design` npm package)
- CI/GitHub Actions
- Any new Jekyll plugins or data files
- Prev/next navigation between conversations

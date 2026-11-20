# Minimal Redesign — Design Spec

**Date:** 2026-11-20

## Goal

Replace the VS Code-themed personal site with a single minimal white page: centered profile photo with a hover-triggered CSS glitch effect, name, tagline lines, and a mailto link.

## What Gets Removed

The entire VS Code chrome is deleted:
- Activity bar, sidebar (file tree), tab bar, status bar
- All editor panes (README, orgs, cat-pics)
- All related JS (openFile, toggleFolder, openLightbox, etc.)
- The lightbox overlay
- All CSS for those components

## Layout

Vertically and horizontally centered on a full-height white page (`min-height: 100vh`, flexbox, `align-items: center`, `justify-content: center`).

### Photo

- `assets/christy.JPG`, circular, ~170px diameter
- `border-radius: 50%`, `object-fit: cover`
- Wrapped in a `.glitch` div that handles the effect

### Text block (below photo, centered, ~16px gap)

```
Christy Frink                                          ← ~28px, bold, dark
product management // community advocate               ← ~13px, muted gray
data privacy // tech accountability // AI safety // civic transparency  ← ~13px, muted gray
get in touch                                           ← ~13px, accent color, mailto: link
```

`mailto:` address: `info@christyfrink.com`

Font: keep existing Fira Code (already loaded from Google Fonts) — it fits the brand and is already in the asset chain.

## Glitch Effect

Pure CSS, no JavaScript, no external library.

**Mechanism:**
- `.glitch` wrapper: `position: relative`, `border-radius: 50%`, `overflow: hidden`, fixed width/height matching the photo
- `img` inside: normal, `position: relative`, `z-index: 1`
- `.glitch::before` and `.glitch::after`: `position: absolute`, `inset: 0`, `background-image: url(assets/christy.JPG)`, `background-size: cover`, `background-position: center`, `opacity: 0` by default, `z-index: 2`
  - `::before`: `filter: hue-rotate(90deg) saturate(3)` — shifts toward cyan/green channel
  - `::after`: `filter: hue-rotate(200deg) saturate(3)` — shifts toward magenta/red channel

**Trigger:** `.glitch:hover::before` and `.glitch:hover::after` activate the animations.

**Animations** (two separate `@keyframes`):
- Each pseudo-element gets a `clip-path: inset(...)` animation stepping through 5–6 random horizontal slices
- Each also gets a small `translateX` offset (±4–6px) to create the RGB-split separation
- Duration: ~0.45s, `steps(1)` timing (hard cuts, not smooth — more authentic glitch feel)
- The base `img` also gets a subtle `translateX` animation on hover to reinforce the shift

## CSS Variables to Keep

Retain only what's needed:
- `--accent` (#29c9be) — used for the "get in touch" link
- Font stack (Fira Code)

Everything else from the existing `:root` block can be removed.

## Files Changed

- `index.html` — full rewrite (single file, everything inline as before)
- No new files created
- `assets/christy.JPG` — unchanged, still referenced
- `CNAME` — unchanged

## Out of Scope

- No analytics, no other pages, no dark mode toggle, no mobile-specific changes beyond the layout being inherently responsive (centered flex works on all screen sizes)

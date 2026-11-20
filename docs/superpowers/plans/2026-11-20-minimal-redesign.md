# Minimal Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the VS Code-themed single-page site with a minimal white page — centered circular photo with a hover-triggered CSS glitch effect, name, two tagline lines, and a mailto link.

**Architecture:** Single `index.html` rewrite — no build step, no dependencies, no new files. The glitch effect is pure CSS using `::before`/`::after` pseudo-elements on the photo wrapper, animated with `clip-path` keyframes triggered by `:hover`.

**Tech Stack:** HTML, CSS (no JS, no libraries, no framework)

---

## File Map

| File | Action | Notes |
|------|--------|-------|
| `index.html` | Full rewrite | Only file that changes |
| `assets/christy.JPG` | Unchanged | Referenced by `<img>` and CSS background-image |
| `CNAME` | Unchanged | — |

---

### Task 1: Rewrite index.html

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace index.html with the complete new implementation**

The new file contains everything — layout, photo, text, glitch CSS — in one shot. Write it now:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Christy Frink</title>
  <meta name="description" content="Christy Frink — product manager, community advocate, civic transparency researcher. Baltimore." />
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500&display=swap" rel="stylesheet">
  <style>
    :root {
      --accent: #29c9be;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Fira Code', 'Consolas', 'Courier New', monospace;
      font-size: 13px;
      background: #ffffff;
      color: #111;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .card {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
      text-align: center;
      padding: 24px;
    }

    /* ── Glitch photo ───────────────────────── */

    .glitch {
      position: relative;
      width: 170px;
      height: 170px;
      border-radius: 50%;
      overflow: hidden;
      flex-shrink: 0;
    }

    .glitch img {
      width: 170px;
      height: 170px;
      object-fit: cover;
      display: block;
      position: relative;
      z-index: 1;
    }

    .glitch::before,
    .glitch::after {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url('assets/christy.JPG');
      background-size: cover;
      background-position: center;
      opacity: 0;
      z-index: 2;
    }

    .glitch::before { filter: hue-rotate(90deg) saturate(3); }
    .glitch::after  { filter: hue-rotate(200deg) saturate(3); }

    .glitch:hover::before {
      opacity: 0.75;
      animation: glitch-clip-1 0.45s steps(1) infinite;
    }

    .glitch:hover::after {
      opacity: 0.75;
      animation: glitch-clip-2 0.45s steps(1) infinite;
    }

    .glitch:hover img {
      animation: glitch-shift 0.45s steps(1) infinite;
    }

    @keyframes glitch-clip-1 {
      0%  { clip-path: inset(0 0 95% 0); transform: translateX(0); }
      15% { clip-path: inset(10% 0 55% 0); transform: translateX(-5px); }
      30% { clip-path: inset(55% 0 25% 0); transform: translateX(4px); }
      50% { clip-path: inset(72% 0 8% 0);  transform: translateX(-3px); }
      65% { clip-path: inset(3% 0 85% 0);  transform: translateX(5px); }
      80% { clip-path: inset(0 0 95% 0);   transform: translateX(0); }
      100%{ clip-path: inset(0 0 95% 0);   transform: translateX(0); }
    }

    @keyframes glitch-clip-2 {
      0%  { clip-path: inset(95% 0 0 0); transform: translateX(0); }
      20% { clip-path: inset(60% 0 15% 0); transform: translateX(6px); }
      35% { clip-path: inset(20% 0 60% 0); transform: translateX(-4px); }
      55% { clip-path: inset(82% 0 3% 0);  transform: translateX(3px); }
      70% { clip-path: inset(38% 0 48% 0); transform: translateX(-6px); }
      85% { clip-path: inset(95% 0 0 0);   transform: translateX(0); }
      100%{ clip-path: inset(95% 0 0 0);   transform: translateX(0); }
    }

    @keyframes glitch-shift {
      0%  { transform: translateX(0); }
      15% { transform: translateX(-3px); }
      30% { transform: translateX(2px); }
      50% { transform: translateX(-2px); }
      65% { transform: translateX(3px); }
      80% { transform: translateX(0); }
      100%{ transform: translateX(0); }
    }

    /* ── Text ───────────────────────────────── */

    .info {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .name {
      font-size: 26px;
      font-weight: 500;
      color: #111;
      letter-spacing: -0.3px;
    }

    .tagline {
      font-size: 12px;
      color: #888;
      line-height: 1.6;
    }

    .contact {
      display: inline-block;
      margin-top: 4px;
      font-size: 12px;
      color: var(--accent);
      text-decoration: none;
    }

    .contact:hover { text-decoration: underline; }
  </style>
</head>
<body>
  <div class="card">
    <div class="glitch">
      <img src="assets/christy.JPG" alt="Christy Frink" />
    </div>
    <div class="info">
      <div class="name">Christy Frink</div>
      <div class="tagline">product management // community advocate</div>
      <div class="tagline">data privacy // tech accountability // AI safety // civic transparency</div>
      <a class="contact" href="mailto:info@christyfrink.com">get in touch</a>
    </div>
  </div>
</body>
</html>
```

---

### Task 2: Verify in browser

**Files:** (none — read-only verification)

- [ ] **Step 1: Open index.html in a browser**

```bash
open index.html
```

- [ ] **Step 2: Check layout**

Verify:
- Page background is white
- Photo is circular and centered on the page
- Name, two tagline lines, and "get in touch" link appear below the photo
- "get in touch" is teal/accent colored

- [ ] **Step 3: Check glitch effect**

Hover your mouse over the photo. Verify:
- Colored horizontal slices appear over the photo and jump around while hovering
- Effect stops when you move the mouse away
- Photo remains circular (no color bleed outside the circle)

- [ ] **Step 4: Check the mailto link**

Click "get in touch". Verify it opens your mail client addressed to `info@christyfrink.com`.

- [ ] **Step 5: Check mobile-ish layout**

Resize the browser window to a narrow width (~375px). Verify the card stays centered and the text wraps without breaking the layout.

---

### Task 3: Commit

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Stage and commit**

```bash
git add index.html
git commit -m "$(cat <<'EOF'
redesign: minimal white page with glitch photo effect

Replace VS Code-themed layout with a centered profile card:
circular photo with CSS hover glitch, name, taglines, mailto link.

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

- [ ] **Step 2: Verify commit**

```bash
git log --oneline -3
```

Expected: new commit at top referencing the redesign.

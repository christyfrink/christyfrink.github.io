# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal portfolio site deployed via GitHub Pages. The entire site is a single file: `index.html`. There is no build step, no framework, no package manager, and no dependencies beyond a Google Fonts CDN link.

To preview: open `index.html` directly in a browser.

## Architecture

The site simulates a VS Code interface with these visual components:

- **Title bar** — fake macOS window controls + current filename
- **Activity bar** — left icon strip (decorative; Explorer icon is "active")
- **Sidebar** — file tree with two folders (`orgs-ive-supported`, `cat-pics`) and `README.md`
- **Editor area** — tab bar + content panes (one per file)
- **Status bar** — git branch, language, encoding

All content is hardcoded directly in HTML as `editor-pane` divs. There are no external data files or templates.

### File/pane system

Each "file" in the tree maps to an entry in the `files` object in the `<script>` block. The object keys (e.g. `readme`, `bfd`, `cat1`) link a tree item (`treeId`), a content pane (`pane`), a tab label (`name`), and a status bar language (`lang`). Opening a file swaps the active pane, updates the tab bar, title bar, and status bar, and highlights the tree item.

### Content panes

- Markdown-style panes use CSS classes: `.md-h1`, `.md-h2`, `.md-p`, `.md-link`, `.md-role`
- Image panes use `.img-view` wrapping a `<img loading="lazy">`
- All content lives inside `#pane-<key>` divs

### Mobile behavior

Below 768px: activity bar hides, sidebar slides in as an overlay triggered by a hamburger button in the title bar. Backdrop click closes it.

### Lightbox

Clicking the avatar in the activity bar opens a circular lightbox overlay of `assets/christy.JPG`. ESC or click anywhere closes it.

## Conventions

- All CSS is in a `<style>` block in `<head>` using CSS custom properties (`--bg-*`, `--text-*`, `--color-*`, `--accent`, `--border`)
- Icons are inline SVGs — no icon libraries
- The `cat-pics` folder starts collapsed; `orgs-ive-supported` starts open
- Images live in `assets/`

## Adding a new "file"

1. Add a tree item `<div class="ti file-md depth-2" id="tree-KEY" onclick="openFile('KEY')">` inside the appropriate subtree
2. Add a content pane `<div class="editor-pane" id="pane-KEY">` in the editor area
3. Add an entry to the `files` object in the script with `name`, `lang`, `pane`, `iconHtml`, and `treeId`

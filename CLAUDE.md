# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static event site for **CIBC Demo Days** (Citizen Infrastructure Builders Collective). Plain HTML, no framework, no build step. Hosted on **GitHub Pages** at https://citizen-infra.github.io/demo-days/ (org: `Citizen-Infra`). The landing page is the canonical event share link (it replaced a Luma link). Issue tracking: GitHub Issues on `Citizen-Infra/demo-days`.

## Commands

There is no build, lint, or test step — the `.html` files are served as-is (`.nojekyll` disables Jekyll processing).

- **Preview locally:** run any static server from the repo root, e.g. `python -m http.server`, then open `http://localhost:8000/`. Relative cross-page links resolve under a server.
- **Deploy:** push to `main`. Pages is "Deploy from a branch" → `main` / root and redeploys in ~30–90s. No Actions workflow.
- **Syntax-check inline JS** (no bundler exists, so this is the only automated gate). Extract the inline script and run `node --check`:
  ```bash
  awk '/<script[^>]*>/{i=1;b="";next} /<\/script>/{if(i)printf "%s",b;i=0;next} i{b=b $0 "\n"}' index.html > /tmp/x.js && node --check /tmp/x.js
  ```

## Architecture

Each page is a **single self-contained HTML file** — inline `<style>` and inline `<script>`, 80–150 KB each. No shared CSS/JS files, no components, no imports. Cross-page nav is relative `<a href="page.html">`. Pages: `index.html` (landing), `ecosystem-map.html`, `get-involved.html`, `research-report.html`, `first-meeting-2026-05-08.html`, `planning-recap-2026-05-15.html`.

### The particle-field background (`initFieldAtmosphere`) — read before touching it

Every page renders an animated canvas "field atmosphere" behind its sections (boids flocking + curl noise + a 4-phase visual cycle). The function `initFieldAtmosphere` is **copy-pasted inline into all six files — it is NOT a shared module.** Any change to the field system must be applied to every file that has it; verify with `grep -c "function initFieldAtmosphere" *.html` (one copy per file). The landing page instantiates it on four canvases (nav, hero, demos, footer); other pages on one or two.

It is expensive. The animation loops are **viewport-gated (`IntersectionObserver`), throttled to ~30fps, and pause on tab-hidden**, with a `prefers-reduced-motion` branch that paints one static frame. Preserve that gating — an ungated 60fps version with the O(n²) flocking pins the CPU/GPU (this was the cause of issue #1). Don't raise particle counts (`CONFIG.mainCount` / `whisperCount`) without checking perf.

## Conventions & gotchas

- **This repo is a published copy, not the source of truth.** Per `HOW-TO-DEPLOY.md`, the canonical files live in an Obsidian vault (`Obsidian_MCP/Cuts/frames/`) and are exported here under release names. Edits made directly in this repo can be overwritten by the next vault export — when making non-trivial changes, flag that they need back-porting to the vault.
- **Files use CRLF line endings.** Keep them consistent; avoid introducing mixed LF/CRLF within a file.
- **`assets/atlas-internal.js` is gitignored deliberately** — internal team/outreach data, not for the open web. Never commit it.
- **`#` hrefs are intentional placeholders, not bugs** — presenter LinkedIn/GitHub URLs, the first session date ("TBD"), and Substack/Telegram/OG-image links are pending (see `HOW-TO-DEPLOY.md` → "What's not in V1"). They fill in as the team confirms them.
- **Link integrity:** old export filenames (`20260515_cibc-demo-day-`, `20260508_cibc-demo-days-`) must not appear anywhere — a leftover reference 404s on Pages. Grep should return zero.
- Imagery assets live in `assets/` as `.webp`/`.png`; UI icons are inline Lucide-style SVG already embedded in each page.

# How to Deploy

The CIBC Demo Day site is static HTML. No build step. Push to GitHub, enable Pages, done.

## First-time deploy

### 1. Create the public repo on GitHub

1. Go to <https://github.com/new>.
2. Owner: `Citizen-Infra` (the CIBC org).
3. Repository name: `demo-days` (recommended). Short, on-brand, future-proof if more demo events get added.
4. Description: `Static event site for CIBC Demo Days. Tools for community deliberation, peer-knowledge sharing, and citizen-owned systems.`
5. **Public.** This site replaces Luma as the share link; broad shareability is the point.
6. Do NOT initialize with README, .gitignore, or license. We're pushing our own.
7. Click "Create repository".

### 2. Push from this folder

From inside `cibc-demo-day-release-1/`:

```bash
git remote add origin https://github.com/Citizen-Infra/demo-days.git
git branch -M main
git push -u origin main
```

If the initial commit hasn't been made yet:

```bash
git init
git add .
git commit -m "first demo days release"
git remote add origin https://github.com/Citizen-Infra/demo-days.git
git branch -M main
git push -u origin main
```

### 3. Enable GitHub Pages

1. In the repo, go to **Settings → Pages** (left sidebar).
2. Under "Build and deployment", set **Source** to "Deploy from a branch".
3. **Branch**: `main`, folder `/ (root)`. Save.
4. Wait 1 to 3 minutes. The Pages settings panel will show the URL: `https://citizen-infra.github.io/demo-days/`.

### 4. Test

Open the URL in an incognito window. The landing page should load with:

- Hero with eyebrow, H1, register CTA, info card
- Demos section with 5 presenter cards
- Field atmosphere on the demos section
- Nav links to ecosystem-map, get-involved
- Footer

Click through to each companion page (ecosystem map, research report, get involved, first meeting recap, planning recap). All cross-page links should resolve.

If a link 404s, the most likely cause: a stray reference to one of the old `Cuts/frames/` filenames slipped through. Grep the repo for `20260515_cibc-demo-day-` and `20260508_cibc-demo-days-`; both should return zero matches.

---

## Updating content

1. Edit the source file in `Obsidian_MCP/Cuts/frames/` (the canonical version).
2. Copy the updated file into this folder under its release name (`index.html`, `ecosystem-map.html`, etc.).
3. If cross-page links changed, re-run the rename script (see `_activity/v1-publishing-plan.md` in the vault).
4. `git add . && git commit -m "<what changed>"` and push.
5. Pages redeploys automatically in 30 to 90 seconds.

**Important.** This folder is a published copy. The canonical source of truth is `Obsidian_MCP/Cuts/frames/`. Never edit the release files in place without updating the source — they will drift.

---

## When you're ready to graduate to a real domain

Buy `demos.cibc.network` (or similar). In repo **Settings → Pages → Custom domain**, enter the domain. Add the DNS records GitHub shows you (a CNAME pointing at `citizen-infra.github.io`). The share link on social, in emails, and on the calendar stays stable while the underlying repo can evolve.

---

## File manifest

Root:

- `index.html` (landing, replaces the Luma share link)
- `ecosystem-map.html` (the 9-org ecosystem grid)
- `get-involved.html` (Telegram, Substack, builders showcase, apply)
- `research-report.html` (full editorial render of the CIBC research report)
- `first-meeting-2026-05-08.html` (May 8 first planning meeting recap)
- `planning-recap-2026-05-15.html` (May 15 second planning meeting recap)
- `HOW-TO-DEPLOY.md` (this file)
- `.gitignore`

Folder:

- `assets/` (currently: `megan.webp`. More presenter photos to be added as cards fill in.)

## What's not in V1

These are tracked in `Obsidian_MCP/@SORT/Session Carryover - 2026-05-20.md` and the v1 publishing plan:

- Real LinkedIn URLs for the 5 presenters (currently `#` placeholders)
- Real GitHub URLs for the 5 presenters (currently `#`)
- Real product URL for Momcilo (currently `#`)
- Demo Day first session date (currently "Mid-June 2026 · TBD")
- "Critical Suit" title (Kyle's card) — likely typo for "Critical Suite" or "Compass Suite", needs confirmation
- Real Substack subscribe URL on the get-involved page
- Real Telegram invite URL on the get-involved page
- OG social-share image

These fill in via the update workflow above as the team confirms them.

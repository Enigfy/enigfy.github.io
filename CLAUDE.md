# Enigfy.com

Static website for Enigfy, a Tech Lab — playing where people meet technology. This folder (`~/Projects/enigfy.com/`) is the Git repo `Enigfy/enigfy.github.io`, published by GitHub Pages at the custom domain www.enigfy.com.

Archetype: **Static website**. Platform-wide rules live in `~/Projects/_platform/CLAUDE.md`; where this file and that one disagree, the platform file wins and this file is the bug.

## Structure

- `index.html` — Home page
- `montecarlo.html`, `monaco.html` — App pages
- `contact.html` — Contact page
- `privacy.html` — General privacy notice
- `privacy-montecarlo.html`, `privacy-monaco.html` — Per-app privacy policies. App Store records and in-app Help screens are likely to link to these URLs, so don't rename or move them without checking those first.
- `terms.html` — Terms of Use
- `privacy-montecarlo.md`, `privacy-monaco.md` — Earlier drafts of the per-app policies; see "Privacy policies" below
- `assets/css/style.css` — The only stylesheet
- `favicon.*`, `apple-touch-icon.png`, `web-app-manifest-*.png`, `site.webmanifest` — Icons and web-app manifest
- `CNAME` — Custom domain (`www.enigfy.com`). Do not edit or delete; GitHub Pages drops the domain without it.

## Page anatomy

There is no templating and no build step. Every page is a standalone HTML file that repeats the same `<head>` icon links, header, `<nav>` and `<footer>`, so a change to any of these must be made in **every** `.html` file.

- **Nav order:** Home · Monte Carlo · Monaco (set 22 May 2026). The current page's link carries `class="active"`.
- **Footer:** `© 2026 Enigfy`, then Contact · Terms · Privacy.
- **Body:** page title in `h1.page-title`, subtitle in `p.page-meta`, content inside `div.prose`.

## Naming conventions

- **Monte Carlo** (two words) — user-facing copy: page titles, nav labels, body text, app name in privacy/terms.
- **montecarlo** (one word, lowercase) — technical only: filenames, URLs, anchors, `href` attributes.

## Privacy policies

- The published `.html` page is authoritative: it is what Apple reviewers and users read. Edit the `.html`.
- The `.md` files were the drafting sources but have drifted from the published pages (e.g. `privacy-monaco.md` is a full policy, while `privacy-monaco.html` is a stub). Do not treat them as current. GitHub Pages also serves them publicly as raw files. Whether to delete them or regenerate the HTML from them is undecided.
- What a policy says about an app (data stored, servers, third parties) must come from that app's own specification, not from this repo or from memory. For Monte Carlo, that is the 2.0 architecture proposal and execution plan in `~/Projects/montecarlo/docs/`; for Monaco, `~/Projects/monaco/`.
- When a policy changes materially, update its "Effective Date" line.

## Deployment

- Push to `main` to publish. There is no CI, no Jekyll config and no build. GitHub Pages usually has the change live within a couple of minutes.
- Remote: `https://github.com/Enigfy/enigfy.github.io.git`. Capital E is canonical.
- Commit messages: imperative mood, one logical change per commit, as in the existing history.

## Current state (26 November 2026)

Update this section with every change that alters it (platform CLAUDE.md, rule 3).

- **Monte Carlo is out of date.** `montecarlo.html` and `privacy-montecarlo.html` (effective 27 April 2026) still describe 1.x: Versus play through the opponent's browser and the Enigfy Rails server on Render. That server was retired on 16 November 2026, and 2.0 uses GameKit. Both pages need a rewrite for 2.0, timed with its release.
- **Monaco:** `privacy-monaco.html` is a stub ("In preparation"), and it has a stray closing `</div>`.
- **Nav order:** `privacy-monaco.html` and `terms.html` still use the old Monaco-first order.
- The Marienbad page and its privacy policy were removed on 22 May 2026.

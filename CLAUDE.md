# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A single-page static marketing site for **Buona Terra Women's Health Coaching** (Kirsten Warstler). The site was exported from **Carrd** (carrd.co) and committed as-is — `assets/main.css` and `assets/main.js` are Carrd-generated bundles (note the `Carrd Site JS | carrd.co | License: MIT` header in `main.js`). There is no build system, no package manager, no tests, and no framework.

## Repo layout

- `index.html` — the entire site (one page, three sections: hero, About Us, Connect form).
- `assets/main.css` (~1850 lines, minified-ish reset + generated rules), `assets/noscript.css`, `assets/main.js` (~3000 lines, Carrd runtime).
- `assets/icons.svg` — sprite referenced via `<use xlink:href="assets/icons.svg#…">`.
- `assets/images/image02.png` — hero image.

## Working on this site

- **Preview locally:** `python3 -m http.server 8000` from the repo root, then open `http://localhost:8000`. No build step.
- **Deploy:** plain `git push origin main`. GitHub Pages is not currently configured via the API (`/repos/.../pages` returns 404), so confirm the actual hosting target with the user before assuming a deploy pipeline exists.
- **Editing content (text, links, images):** edit `index.html` directly. Element IDs like `text22`, `container16`, `buttons05` are Carrd-assigned — keep them stable; `main.css` selectors are keyed to them.
- **Editing styles:** prefer adding a small override stylesheet linked *after* `assets/main.css` rather than editing the generated CSS. The generated file will be clobbered the next time the site is re-exported from Carrd.
- **Re-exporting from Carrd:** if the user updates the design in Carrd and re-exports, expect `main.css`, `main.js`, `noscript.css`, `icons.svg`, and `index.html` to be wholesale replaced. Treat hand-edits to those files as at risk on every re-export — keep custom changes minimal and documented in commit messages.

## Things to know before changing anything

- The contact form posts to `action="#"` — it does not actually submit anywhere. If the user wants form submissions to work, this needs a backend (Formspree, Netlify Forms, etc.) and is a real change, not a tweak.
- Social/contact links in `index.html`: Instagram `@buonaterrawhc`, Facebook profile id `61587481711205`, email `buonaterrawhc@gmail.com`. Update all three together if the client rebrands.
- `<body class="is-loading">` — the `is-loading` class is removed by `main.js` on load. Don't strip it; the CSS depends on it for the fade-in.

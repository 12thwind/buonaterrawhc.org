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

- The contact form posts to FormSubmit (`https://formsubmit.co/<hash>`) — the hash maps to `buonaterrawhc@gmail.com`. Do not replace the hash with the plaintext address; it's there to keep Kirsten's inbox out of scraped HTML. Hidden fields control behavior: `_subject`, `_template=table`, `_next=https://buonaterrawhc.org/?sent=1`, `_autoresponse`, and a `_honey` honeypot. CAPTCHA is left at FormSubmit's default (on) — only disable with `_captcha=false` if spam stays low. After redirect, an inline script at the end of `<body>` swaps the form for a thank-you message and scrolls it into view when `?sent=1` is present.
- **If the FormSubmit endpoint ever needs re-issuing** (e.g. Kirsten changes inbox, hash is rotated): submit the live form once with the new email in `action=`, click the activation link FormSubmit sends, then replace `action=` with the new hash from that email. A re-export from Carrd will wipe these form changes — re-apply from git history.
- Social/contact links in `index.html`: Instagram `@buonaterrawhc`, Facebook profile id `61587481711205`, email `buonaterrawhc@gmail.com`. Update all three together if the client rebrands.
- `<body class="is-loading">` — the `is-loading` class is removed by `main.js` on load. Don't strip it; the CSS depends on it for the fade-in.

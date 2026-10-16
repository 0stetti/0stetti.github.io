# 0stetti.github.io — handoff for Claude Code

## What this is
Personal site + e-poster page for **Elton Ostetti** (PhD Student, Butantan Institute / University of São Paulo), hosted on GitHub Pages at **https://0stetti.github.io**.

Elton's printed business card (for a scientific conference in Japan) has a QR code pointing to the site root. **The URL must never change** — don't rename the repo, don't move the page to a subpath. The site can grow later (publications, CV), but `index.html` at the root must always load.

People scan the QR on their phone, so mobile is the primary target. **The e-poster is withheld for now** (see Rules): the page only says it will be available soon.

## Repo
- Remote: `https://github.com/0stetti/0stetti.github.io.git`
- Branch: `main`
- GitHub Pages: Settings → Pages → Source "Deploy from a branch", `main`, `/ (root)`. Pages serves the files as they are, no build step.

## Your task
1. Clone the repo (or init and add the remote if it's empty).
2. Copy the site files from this folder to the repo root: `index.html`, `support.js`, `CLAUDE.md`, `.nojekyll`, the icons and `og.png`. **Never copy `_nao-publicar/`** or anything in it.
3. Only add `poster.jpg` / `poster.pdf` when Elton explicitly says the poster can be published.
4. Commit and push to `main`.
5. Make sure GitHub Pages is on (see above). With the `gh` CLI:
   `gh api -X POST repos/0stetti/0stetti.github.io/pages -f "source[branch]=main" -f "source[path]=/"` (if it's already on, this returns an error you can ignore).
6. Check that https://0stetti.github.io loads (it can take 1–2 minutes).

## Files
- `index.html` — the whole page. It's written as a "Design Component": HTML with inline styles, `{{ value }}` holes, `<sc-for>`/`<sc-if>` blocks, and a logic class in the `<script type="text/x-dc">` at the bottom. It runs in the browser through `support.js`, so it works without a build step.
- `support.js` — runtime that renders `index.html`. **Don't edit or delete it.**
- `.nojekyll` — tells GitHub Pages not to run Jekyll on the files.
- `favicon-32.png`, `favicon-192.png`, `apple-touch-icon.png` — "EO" in Gloock, white on black.
- `og.png` — 1200×630 link-preview image (screenshot of the hero without the button). The `og:*` tags live in the static `<head>`, not in `<helmet>`, because link-preview crawlers don't run JS. Regenerate it if the hero changes.

## Page structure
1. **Hero (white)**: name in Gloock, katakana エルトン・オステッチ (Noto Sans JP; the spelling is confirmed, don't change it), "PhD Student", list of specialties, and a "View e-poster" button. The background is a field of ATCG letters at low opacity; a B-DNA double helix (pitch 1.7× diameter, 10.5 bp per turn, unequal grooves) runs through it, drawn by brighter letters. The helix turns slowly (`TURN_SECONDS` at the top of the logic script): the letters stay put and `tick()` relights the ones in the helix band directly in the DOM, outside React, pausing when the section is off screen, while the poster viewer is open, or under `prefers-reduced-motion`. Letter brightness is `color: rgba(…)`, **not** `opacity` — per-letter opacity made each frame repaint a few thousand layers and dropped desktop to ~4 fps.
2. **E-poster**: for now only the heading "The e-poster will be available soon." The poster logic is still in place: once `poster.jpg` / `poster.pdf` exist, the image, Full screen viewer (tap to zoom) and Download PDF button appear on their own. Title, authors and conference were removed and must be written back when the poster is released.
3. **About (white)**: short bio (research on UCOEs, bench + code, own bioinformatics tools). Elton chose not to name personal projects or list publications here.
4. **Contact (black)**: same idea in negative, with 0/1 characters; shows email, profile links as plain text (ORCID, Lattes, LinkedIn) and affiliations. If you add content here, check that `footH` in the logic still covers the section height, or the 0/1 background stops short.

The visual style copies the business card: black and white only, Gloock + Jost Light + IBM Plex Mono, wide letter-spacing, no logos/icons/lines. Keep it that way.

## How to edit the content
- **Poster files**: constants at the top of the logic script in `index.html`:
  ```js
  const POSTER_IMAGE = 'poster.jpg';
  const POSTER_PDF = 'poster.pdf';
  const DOI = ''; // e.g. '10.5281/zenodo.1234567' — the DOI button only appears when this is filled in
  ```
  If `poster.jpg` is missing, no image or viewer button shows, and the "Download PDF" button only appears once `poster.pdf` exists.
- **Title / authors / conference**: plain text in the `#poster` section, under the "E-poster" label.
- **Poster image tip**: export it as JPG, about 3000–4000 px on the long side, under ~3 MB, so it loads fast on mobile data.

## Rules
- Keep the styles inline (that's how this format works); don't add a stylesheet or a build tool.
- Never change the site URL or the katakana.
- Don't publish the poster — image, PDF, title, authors or results — until Elton explicitly says it's cleared. `_nao-publicar/` holds local-only files and must never reach the repo, which is public.
- No emoji-capable symbols (↗ ↘ ↔ ▶ ✓ ★ …): iPhones render them as colour emoji. The ↓ (U+2193) in the hero button is safe; for anything else, use plain text.

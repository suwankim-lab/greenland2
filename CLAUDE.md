# CLAUDE.md

Guidance for AI assistants (and humans) working in this repository.

## What this is

A single-page, mobile-first **promotional landing page** for the film
*그린랜드2: 마이그레이션* ("Greenland 2: Migration"), produced by **kinolights**.
The page is written in Korean and designed for a phone-width viewport (max 520px).
It is a **static site** — plain HTML, CSS, and vanilla JavaScript with **no build
step, no framework, no dependencies, and no package manager**. Everything ships
as-is.

The page stitches together a movie-teaser experience in a single scroll:

1. **Title hero** — animated `title.png` reveal with tagline and release meta.
2. **Teaser poster** — `teaser-poster.jpg` in a tappable frame that opens a lightbox.
3. **Official trailer** — click-to-load YouTube embed (poster until played).
4. **Booking** — grid of Korean cinema-chain links (CGV, 메가박스, 롯데시네마, etc.).
5. **Short-form feed** — tabbed Instagram embed + horizontally-scrolling YouTube Shorts.
6. **Kinolights detail** — cards linking to the kinolights title pages.
7. **Footer** — branding and release date.

## Repository layout

```
index.html         # Canonical, most-refined version of the page — edit THIS
greenland2.html    # Older/alternate copy of the same page (see note below)
title.png          # Title-logo graphic used in the hero (~127 KB)
teaser-poster.jpg  # Teaser poster image, also used in the lightbox (~338 KB)
README.md          # One line ("# greenland2")
```

There is **no `src/`, no config, no tooling**. The two HTML files are
self-contained: all CSS lives in a single `<style>` block in `<head>`, and all
JS lives in one `<script>` block before `</body>`.

### `index.html` vs `greenland2.html`

Both files render essentially the same page. `index.html` is the **canonical**
entry point and is slightly more polished than `greenland2.html` — its
differences are refinements to the title hero:

- An extra dark radial gradient behind the title for depth.
- A faster `titleReveal` animation (1.4s vs 1.8s) with `will-change` and a
  simpler keyframe (drop-shadow moved onto the element instead of animated).

**Treat `index.html` as the source of truth.** When making content or styling
changes, edit `index.html`. If a change should apply everywhere, port it to
`greenland2.html` too — otherwise the two will keep drifting apart. If you're
unsure whether both should change, ask.

## Conventions

- **Language:** UI copy and code comments are in **Korean**. Keep new copy and
  comments in Korean to match. `lang="ko"` is set on `<html>`.
- **Fonts:** Loaded from Google Fonts via `<link>` in `<head>`. Three families,
  exposed as CSS variables:
  - `--display` → `Black Han Sans` (Korean display headings, logos)
  - `--korean` → `Gothic A1` (body text)
  - `--mono` → `Oswald` (uppercase Latin labels, meta, captions)
- **Design tokens:** All colors, fonts, and the hairline border live as CSS
  custom properties in `:root` at the top of the `<style>` block. The palette is
  a dark, rust/brown cinematic theme (`--bg`, `--rust`, `--rust-bright`,
  `--cream`, `--ink`, etc.). **Reuse these variables** — do not hardcode new hex
  values when a token already fits.
- **Layout:** Mobile-only. Content is capped by `.wrap { max-width: 520px }`.
  Sections use `--line` hairline top borders and a consistent
  `.section-tag` / `.section-title` heading pattern (numbered `01 /`, `02 /`, …).
- **Styling approach:** Hand-written CSS, BEM-ish flat class names (`.poster-frame`,
  `.yt-short-poster`, `.kino-card`). Animations via `@keyframes`. Effects like
  film grain and scanlines are done with inline SVG data-URIs and
  `repeating-linear-gradient` pseudo-elements.
- **JavaScript:** Vanilla, no libraries. Small imperative handlers for: trailer
  click-to-load, short-form tab switching, YouTube Shorts click-to-load, and the
  poster lightbox (open/close/Escape). Videos are **not** loaded until tapped
  (iframes are created on click) to keep the initial page light.

## Placeholders you may be asked to fill

Several spots are intentionally stubbed and marked with Korean `※` notes:

- **YouTube Shorts:** `data-embed` URLs contain `SHORTID1`/`SHORTID2`/`SHORTID3`.
  The click handler ignores any URL containing `SHORTID`, so shorts stay inert
  until replaced with real video IDs.
- **Trailer:** The `trailerUrl` const near the top of the `<script>` holds the
  official trailer embed ID — swap the ID to change trailers.
- **Instagram embed:** Only renders on a public/deployed domain (noted in the
  `.ig-note`).

## Running & previewing

It's static — just open the file, or serve the directory:

```bash
# Open directly
open index.html            # macOS
xdg-open index.html        # Linux

# Or serve (needed for the Instagram embed / realistic testing)
python3 -m http.server 8000
# then visit http://localhost:8000/
```

Because the page is designed for phone width, preview in a mobile viewport
(≤520px) or the browser's device-emulation mode. There are **no tests, no linter,
and no build** — verification is visual: load the page and check the section you
changed.

## Making changes — checklist

- Edit `index.html` (the canonical file); port to `greenland2.html` if the change
  should apply to both.
- Reuse existing CSS variables and the established section/heading patterns.
- Keep new copy and comments in Korean.
- Verify visually at mobile width after any change.
- Don't add build tooling, frameworks, or dependencies unless explicitly asked —
  the zero-dependency, single-file nature is intentional.

## Git workflow

- Default branch is `main`.
- Do all work on the assigned feature branch and push there; never push directly
  to `main` without explicit permission.
- Do not open a pull request unless explicitly asked.

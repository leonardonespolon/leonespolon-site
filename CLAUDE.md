# leonespolon-site

Personal website for Leonardo Nespolon.

## Stack
- Plain HTML/CSS — no frameworks, no build step
- Static files served directly

## File Structure
- `index.html` — homepage
- `article-template.html` — template for articles at root level
- `writing/_template.html` — template for articles in the writing/ directory (use relative paths: `../public/style.css`, `../index.html`)
- `public/style.css` — shared stylesheet (tokens, reset, nav, footer, retro mode)
- `public/` — static assets (favicons, shared CSS)

## Design
Always read `DESIGN.md` before making any visual or UI decisions.
All fonts, colors, spacing, and aesthetic direction are defined there.

## Rules
- No frameworks or build tools — keep it plain HTML/CSS
- Do not restructure files without asking first
- Prefer editing existing files over creating new ones

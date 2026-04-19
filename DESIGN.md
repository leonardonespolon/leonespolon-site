# Design System — leonespolon.com

Always follow this document before making any visual or UI decisions.
Do not deviate without explicit user approval.

---

## Aesthetic

Minimal, editorial, warm. Inspired by literary personal sites — not a portfolio, not a blog platform. Feels hand-crafted. Serif-first.

---

## Fonts

| Role | Font | Fallback |
|------|------|----------|
| Body / headings | Source Serif 4 | Georgia, serif |
| UI / labels / nav | Instrument Sans | system-ui, sans-serif |
| Code / monospace | JetBrains Mono | monospace |

Loaded via Google Fonts. Base font size: `18px` (desktop), `16px` (mobile ≤600px). Line height: `1.75`.

---

## Colors

### Light mode (default)
| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#F7F4EE` | Page background |
| `--surface` | `#F0EDE5` | Cards, elevated surfaces |
| `--text` | `#1C1917` | Primary text |
| `--muted` | `#78716C` | Secondary text |
| `--faint` | `#A8A29E` | Dates, labels, metadata |
| `--accent` | `#3B7DD8` | Links, interactive elements |
| `--accent-h` | `#2E63B0` | Accent hover state |
| `--border` | `#E2DDD5` | Dividers, card borders |

### Dark mode (auto via `prefers-color-scheme` or `[data-theme="dark"]`)
| Token | Value |
|-------|-------|
| `--bg` | `#1C1917` |
| `--surface` | `#28231F` |
| `--text` | `#F5F0E8` |
| `--muted` | `#A8A29E` |
| `--faint` | `#78716C` |
| `--accent` | `#5A9AE8` |
| `--accent-h` | `#E08060` |
| `--border` | `#2E2A26` |

Theme toggle is user-controlled via a pill button in the nav. Respect `[data-theme="light"]` and `[data-theme="dark"]` attributes on the root element.

---

## Layout

- Max content width: `660px` (`--col`)
- Horizontal padding: `40px` desktop, `20px` mobile (`--pad`)
- Centered with `margin: 0 auto`

---

## Spacing

- Nav: `40px` top, `56px` bottom
- Bio section: `56px` bottom padding
- Sections: `48px` top and bottom
- Section label margin-bottom: `32px`
- Footer: `40px` top, `56px` bottom

---

## Typography Scale

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| Site name | Sans | `0.88rem` | 600 | Letter-spacing `0.02em` |
| Nav links | Sans | `0.78rem` | 400 | Muted color |
| Section labels | Sans | `0.68rem` | 600 | Uppercase, letter-spacing `0.1em`, faint color |
| Body / post titles | Serif | `1rem` | 400 | Line height `1.75` |
| Bio text | Serif | `1.08rem` | 400 | Line height `1.8`, max-width `520px` |
| Section subtext | Serif | `0.875rem` | 400 | Italic, muted color |
| Dates / metadata | Sans | `0.72rem` | 400 | Faint color |
| Post tags | Sans | `0.6rem` | 500 | Accent color, pill shape |
| Footer | Sans | `0.72rem` | 400 | Faint color |

---

## Components

### Buttons
- **Primary:** Accent background, white text, `border-radius: 4px`, `padding: 9px 18px`
- **Ghost:** Transparent, border `var(--border)`, hover turns accent
- **Text:** Accent color, underline, no border/background

### Cards (Notes)
- Background: `var(--surface)`
- Border: `1px solid var(--border)`, `border-radius: 6px`
- Padding: `16px 20px`
- Hover: border turns accent color

### Post list
- Flex row: title left, date right
- Title underline appears on hover (accent color)
- Collapses to stacked layout on mobile (≤480px)

### Notes grid
- 2-column grid, `gap: 12px`
- Collapses to 1 column on mobile (≤520px)

---

## Interactions

- All transitions: `0.15s` ease
- Hover states: color shifts to `--accent`
- Theme toggle: smooth `0.2s` background/color transition
- Count-up animation: scroll-triggered, eased cubic, `~1400ms` (e.g. km counter)

---

## Do Not
- Use frameworks (Bootstrap, Tailwind, etc.)
- Change fonts without approval
- Introduce new color values outside the token system
- Add animations beyond the existing theme transitions, retro mode blink, and count-up

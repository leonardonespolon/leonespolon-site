# Retro Mode — Design Spec

**Date:** 2026-04-13
**Status:** Approved

## Overview

Add a "Retro Mode" to leonespolon.com that transforms the site's clean, warm aesthetic into a 90s internet experience. Activated via a nav button or the Konami code. State persists across page loads via localStorage.

## Triggers

### 1. Nav Button
A "Retro" button added to the nav, next to the existing Dark/Light toggle. When retro mode is active, the button label changes to "☆ Normal". Clicking toggles the mode.

### 2. Konami Code (Easter Egg)
Typing ↑↑↓↓←→←→BA on any page activates retro mode. On activation, a brief "🕹️ CHEAT CODE ACTIVATED" flash message appears on screen for ~2 seconds, then fades out.

## State Management

- `data-retro="true"` attribute set on `<html>` when active
- Persisted to `localStorage` under the key `"retro"`
- Loaded on page init (same pattern as existing dark/light theme toggle)

## Implementation Approach

CSS attribute toggle — a single CSS block using `[data-retro="true"]` selector overrides design tokens and styles. No extra files. All changes scoped to that selector block.

Both `index.html` and `writing/_template.html` need the retro CSS block and JS logic added.

## Visual Changes (Retro Mode Active)

| Element | Normal | Retro |
|---|---|---|
| Background | `#F7F4EE` warm paper | `#FFFF99` yellow + subtle diagonal pattern |
| Font | Source Serif 4 / Instrument Sans | Comic Sans MS throughout |
| Text color | `#1C1917` near-black | `#000066` dark blue |
| Accent / links | `#3B7DD8` calm blue | `#0000CC` 90s blue |
| Nav border | `1px solid var(--border)` | `3px dashed #FF0000` |
| Site name | Normal weight | Red (`#CC0000`) + orange text-shadow |
| Top of page | Nothing | Scrolling marquee banner: "★ WELCOME TO MY HOMEPAGE ★ BEST VIEWED IN NETSCAPE NAVIGATOR ★" |
| Site name suffix | Nothing | Blinking ★ (CSS animation, 1s step-end) |
| Section labels | Uppercase faint sans | Bold red uppercase |
| Post links | Serif, subtle underline on hover | Blue underline always visible |

## Easter Eggs

### Konami Code
- JS listens for the sequence: `ArrowUp ArrowUp ArrowDown ArrowDown ArrowLeft ArrowRight ArrowLeft ArrowRight KeyB KeyA`
- On match: activates retro mode + shows a `position:fixed` flash message ("🕹️ CHEAT CODE ACTIVATED") that fades out after 2 seconds
- If retro mode is already active when Konami is triggered: shows the flash anyway (reward repeat visitors)

### Console Message
When retro mode is active, an extended console message fires:
```
🕹️ You found retro mode.
   If you're reading this, we're going to get along just fine.
   — Leonardo
```

## Scope

- Changes apply to `index.html` and `writing/_template.html`
- No new files created
- No build tools, no dependencies
- The `.superpowers/` directory should be added to `.gitignore`

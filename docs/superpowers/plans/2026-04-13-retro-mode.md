# Retro Mode Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a toggleable 90s retro mode to leonespolon.com, triggered by a nav button or the Konami code, that transforms the site's visual aesthetic into a Comic Sans / yellow / 90s internet experience.

**Architecture:** A `data-retro="true"` attribute on `<html>` drives all visual changes via a CSS block that overrides design tokens. State is persisted in `localStorage`. A Konami code listener and a nav button both call the same `setRetro()` function. No new files — all changes go inside the two existing HTML files.

**Tech Stack:** Plain HTML, CSS, vanilla JS. No dependencies.

---

## File Map

| File | Changes |
|---|---|
| `index.html` | Add retro CSS block, marquee HTML, blinking star, retro button, retro JS, flash div |
| `writing/_template.html` | Mirror all the same changes from index.html |

---

### Task 1: Add retro CSS to `index.html`

**Files:**
- Modify: `index.html` (inside the `<style>` block, before line 115 `</style>`)

- [ ] **Step 1: Add the retro CSS block**

Find line 114 (`  .footer-links { display: flex; gap: 16px; }`) and insert after it, before `  </style>`:

```css
    /* ── Retro Mode ───────────────────────────────────────────── */
    [data-retro="true"] {
      --bg:       #FFFF99;
      --surface:  #FFFFCC;
      --text:     #000066;
      --muted:    #CC0000;
      --faint:    #CC6600;
      --accent:   #0000CC;
      --accent-h: #0000AA;
      --border:   #FF0000;
      --serif: 'Comic Sans MS', 'Chalkboard SE', cursive;
      --sans:  'Comic Sans MS', 'Chalkboard SE', cursive;
    }
    [data-retro="true"] body {
      background-image: repeating-linear-gradient(
        45deg, transparent, transparent 10px,
        rgba(255,0,0,0.04) 10px, rgba(255,0,0,0.04) 20px
      );
    }
    [data-retro="true"] nav { border-bottom: 3px dashed #FF0000; padding-bottom: 16px; }
    [data-retro="true"] .site-name { color: #CC0000; text-shadow: 2px 2px 0 #FF9900; }
    [data-retro="true"] .nav-links a { color: #0000CC; text-decoration: underline; }
    [data-retro="true"] .post-link { color: #0000CC; text-decoration: underline; }
    [data-retro="true"] .section-label { color: #CC0000; font-weight: 700; }
    [data-retro="true"] .theme-btn { border: 2px solid #000; border-radius: 0; }
    [data-retro="true"] .retro-btn { background: #00CC00; color: #FFFF00; border: 2px solid #000; border-radius: 0; font-weight: bold; }

    /* Blinking star — hidden in normal mode */
    .retro-star { display: none; }
    [data-retro="true"] .retro-star { display: inline; animation: retro-blink 1s step-end infinite; }
    @keyframes retro-blink { 50% { opacity: 0; } }

    /* Marquee banner — hidden in normal mode */
    .retro-marquee { display: none; background: #000080; padding: 5px 0; overflow: hidden; }
    [data-retro="true"] .retro-marquee { display: block; }
    .retro-marquee-inner { display: inline-block; white-space: nowrap; color: #FFFF00; font-family: 'Comic Sans MS', cursive; font-size: 0.75rem; font-weight: bold; animation: retro-scroll 20s linear infinite; }
    @keyframes retro-scroll { from { transform: translateX(100vw); } to { transform: translateX(-100%); } }

    /* Konami flash message */
    .retro-flash { position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background: #000080; color: #FFFF00; font-family: 'Comic Sans MS', cursive; font-size: 1.2rem; font-weight: bold; padding: 20px 32px; border: 4px solid #FFFF00; z-index: 9999; pointer-events: none; opacity: 0; transition: opacity 0.3s; }
    .retro-flash.show { opacity: 1; }
```

- [ ] **Step 2: Open `index.html` in browser and verify no visual change**

Open `index.html` in your browser. The page should look exactly the same as before — all the new CSS is gated behind `[data-retro="true"]` which isn't set yet.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add retro mode CSS to index.html"
```

---

### Task 2: Add retro HTML elements to `index.html`

**Files:**
- Modify: `index.html` (3 HTML edits)

- [ ] **Step 1: Add the marquee banner**

Find the line `<div class="wrap">` (just after `<body>`) and insert the marquee immediately after it:

```html
<div class="wrap">

  <div class="retro-marquee">
    <span class="retro-marquee-inner">★ WELCOME TO MY HOMEPAGE ★ BEST VIEWED IN NETSCAPE NAVIGATOR ★ EST. 2026 ★ YOU HAVE FOUND THE FUN ZONE ★ &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>
  </div>
```

- [ ] **Step 2: Add the blinking star to the site name**

Find:
```html
      <span class="site-name">Leonardo Nespolon</span>
```
Replace with:
```html
      <span class="site-name">Leonardo Nespolon<span class="retro-star"> ★</span></span>
```

- [ ] **Step 3: Add the Retro button to the nav**

Find:
```html
        <button class="theme-btn" id="theme-btn" aria-label="Toggle colour scheme">Dark</button>
```
Replace with:
```html
        <button class="theme-btn" id="theme-btn" aria-label="Toggle colour scheme">Dark</button>
        <button class="theme-btn retro-btn" id="retro-btn" aria-label="Toggle retro mode">Retro</button>
```

- [ ] **Step 4: Add the flash div**

Find `</body>` at the bottom and insert before it:
```html
<div class="retro-flash" id="retro-flash">🕹️ CHEAT CODE ACTIVATED</div>

</body>
```

- [ ] **Step 5: Open `index.html` in browser and verify HTML is in place**

Open `index.html`. The marquee div and flash div should be invisible (display:none). The nav should show "Dark" and "Retro" buttons side by side. The blinking star should not be visible. Nothing should look broken.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add retro mode HTML elements to index.html"
```

---

### Task 3: Add retro JS to `index.html`

**Files:**
- Modify: `index.html` (add a new `<script>` block after the existing theme toggle script)

- [ ] **Step 1: Add the retro JS block**

Find the closing of the theme toggle script:
```javascript
});
</script>

<script>
console.log
```

Insert a new script block between the theme script and the console script:

```html
<script>
// Retro mode
const retroBtn = document.getElementById('retro-btn');
const retroFlash = document.getElementById('retro-flash');
let retro = false;

function setRetro(on) {
  retro = on;
  document.documentElement.setAttribute('data-retro', on ? 'true' : 'false');
  retroBtn.textContent = on ? '☆ Normal' : 'Retro';
  try { localStorage.setItem('retro', on ? 'true' : 'false'); } catch(e) {}
  if (on) {
    console.log('%c 🕹️ You found retro mode.\n   If you\'re reading this, we\'re going to get along just fine.\n   — Leonardo', 'font-family: Comic Sans MS, cursive; font-size: 13px; color: #0000CC;');
  }
}

// Restore from localStorage
try {
  if (localStorage.getItem('retro') === 'true') setRetro(true);
} catch(e) {}

retroBtn.addEventListener('click', () => setRetro(!retro));

// Konami code
const KONAMI = ['ArrowUp','ArrowUp','ArrowDown','ArrowDown','ArrowLeft','ArrowRight','ArrowLeft','ArrowRight','KeyB','KeyA'];
let konamiIdx = 0;
document.addEventListener('keydown', e => {
  if (e.code === KONAMI[konamiIdx]) {
    konamiIdx++;
    if (konamiIdx === KONAMI.length) {
      konamiIdx = 0;
      if (!retro) setRetro(true);
      retroFlash.classList.add('show');
      setTimeout(() => retroFlash.classList.remove('show'), 2000);
    }
  } else {
    konamiIdx = e.code === KONAMI[0] ? 1 : 0;
  }
});
</script>
```

- [ ] **Step 2: Open `index.html` in browser and test the Retro button**

Open `index.html`. Click the "Retro" button:
- Page background turns yellow
- Font switches to Comic Sans
- Nav gets a red dashed border
- Site name turns red with orange shadow
- Marquee appears at top scrolling
- Blinking star appears next to name
- Button label changes to "☆ Normal"

Click "☆ Normal" — everything reverts to the normal warm aesthetic.

- [ ] **Step 3: Test localStorage persistence**

With retro mode on, close the tab and reopen `index.html`. The page should load in retro mode immediately.

- [ ] **Step 4: Test the Konami code**

On the page, type: ↑ ↑ ↓ ↓ ← → ← → B A (arrow keys + B and A keys). The "🕹️ CHEAT CODE ACTIVATED" flash should appear for 2 seconds, and retro mode should activate.

- [ ] **Step 5: Test the console message**

Open DevTools → Console. Activate retro mode. You should see:
```
🕹️ You found retro mode.
   If you're reading this, we're going to get along just fine.
   — Leonardo
```

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add retro mode JS toggle and Konami code to index.html"
```

---

### Task 4: Mirror all changes to `writing/_template.html`

**Files:**
- Modify: `writing/_template.html`

- [ ] **Step 1: Add the retro CSS block**

The `writing/_template.html` has the same `<style>` block structure as `index.html`. Find the line:
```css
    .footer-links { display: flex; gap: 16px; }
```
And insert the full retro CSS block after it (identical to Task 1, Step 1).

- [ ] **Step 2: Add the marquee banner**

Find `<div class="wrap">` and insert the marquee div immediately after it (identical to Task 2, Step 1).

- [ ] **Step 3: Add blinking star to site name**

Find:
```html
      <a href="index.html" class="site-name">Leonardo Nespolon</a>
```
Replace with:
```html
      <a href="index.html" class="site-name">Leonardo Nespolon<span class="retro-star"> ★</span></a>
```

- [ ] **Step 4: Add the Retro button to the nav**

Find:
```html
        <button class="theme-btn" id="theme-btn" aria-label="Toggle colour scheme">Dark</button>
```
Replace with:
```html
        <button class="theme-btn" id="theme-btn" aria-label="Toggle colour scheme">Dark</button>
        <button class="theme-btn retro-btn" id="retro-btn" aria-label="Toggle retro mode">Retro</button>
```

- [ ] **Step 5: Add the flash div before `</body>`**

```html
<div class="retro-flash" id="retro-flash">🕹️ CHEAT CODE ACTIVATED</div>

</body>
```

- [ ] **Step 6: Add the retro JS block**

Insert the full retro JS `<script>` block (identical to Task 3, Step 1) after the theme toggle script.

- [ ] **Step 7: Open `writing/_template.html` in browser and verify**

Open `writing/_template.html`. Test the Retro button — same visual changes should apply. Test that "← Back" link and nav links still work. Test Konami code.

- [ ] **Step 8: Commit**

```bash
git add writing/_template.html
git commit -m "feat: add retro mode to article template"
```

---

### Task 5: Push to GitHub

- [ ] **Step 1: Push**

```bash
git push origin main
```

Expected output: `main -> main` with no errors.

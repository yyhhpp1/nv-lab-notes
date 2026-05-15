# NV Lab Notes — Style Guide

This document defines the visual and structural conventions for all notes in this repo.
Every new note should start from the template below. Do not introduce new color variables,
font families, or component patterns without updating this guide.

---

## Design principles

- **Adaptive light/dark** — all pages respond to `prefers-color-scheme`. Never hard-code
  a dark-only or light-only palette.
- **System sans-serif body** — no web font for body text; avoids loading delay and FOUT.
  JetBrains Mono is loaded for monospace elements only (eq labels, table data, TOC labels).
- **One color family** — every page shares the same `--blue-*` / `--gray-*` variables.
  Technique colors (`--sted`, `--gsd`, …) are available everywhere but used only in
  comparison tables.
- **No decoration for decoration's sake** — no animations, grain textures, or drop shadows
  on text. Visual weight serves structure, not aesthetics.

---

## Required `<head>` block

Every page must include these two lines before the `<style>` tag:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

MathJax is loaded from jsDelivr CDN with the `async` attribute:

```html
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js" async></script>
```

---

## CSS variables

Paste this `:root` block verbatim into every new page. Do not rename or add variables
without updating all existing pages.

```css
:root {
  --blue-dark:      #1f4e79;
  --blue-mid:       #2e75b6;
  --blue-box:       #ebf3fb;
  --blue-der:       #f0f7e6;
  --blue-derborder: #5a9e2f;
  --gray-text:      #3d3d3a;
  --gray-mid:       #6b6b68;
  --gray-rule:      #cccccc;
  --bg:             #ffffff;
  --sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Arial, sans-serif;
  --mono: "JetBrains Mono", "Courier New", monospace;
  /* technique colors — use in comparison tables only */
  --sted:    #185FA5;  --gsd:     #3B6D11;
  --csd:     #993556;  --smlm:    #BA7517;
  --resolft: #533AB7;  --other:   #5F5E5A;
}
@media (prefers-color-scheme: dark) {
  :root {
    --blue-dark:      #93c5e8;
    --blue-mid:       #7eb8e8;
    --blue-box:       #1a2a3a;
    --blue-der:       #1a2a1a;
    --blue-derborder: #5a9e2f;
    --gray-text:      #d4d2c8;
    --gray-mid:       #9a9890;
    --gray-rule:      #444441;
    --bg:             #1a1a18;
  }
}
```

---

## Layout

| Page type | `max-width` | `padding` |
|-----------|-------------|-----------|
| Note (narrative) | `840px` | `2.5rem 2rem 5rem` |
| Reference table | `1100px` | `2.5rem 1.5rem 5rem` |

```css
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: var(--sans);
  font-size: 16px; line-height: 1.8;
  color: var(--gray-text); background: var(--bg);
  max-width: 840px; margin: 0 auto;
  padding: 2.5rem 2rem 5rem;
}
```

---

## Components

### Nav breadcrumb

Always present at the top of every page. Links: `← Home`, peer notes by number, table.

```html
<nav class="nav">
  <a href="index.html">← Home</a>
  <span class="sep">|</span>
  <a href="NV_misalignment.html">Note 02: Misalignment</a>
  <span class="sep">|</span>
  <a href="superresolution_table.html">Superresolution Table</a>
</nav>
```

```css
.nav { display: flex; align-items: center; gap: 1.2rem; margin-bottom: 2rem; font-size: 0.88rem; }
.nav a { color: var(--blue-mid); text-decoration: none; }
.nav a:hover { text-decoration: underline; }
.nav .sep { color: var(--gray-rule); }
```

---

### Title block

```html
<div class="title-block">
  <h1>Note Title</h1>
  <div class="subtitle">One-line description — with derivations</div>
</div>
```

```css
.title-block { border-bottom: 2px solid var(--blue-mid); padding-bottom: 1.2rem; margin-bottom: 2rem; }
.title-block h1 { font-size: 1.7rem; font-weight: 700; color: var(--blue-dark); line-height: 1.3; margin-bottom: 0.3rem; }
.title-block .subtitle { font-size: 1rem; color: var(--gray-mid); font-style: italic; margin-top: 0.4rem; }
```

---

### Table of contents

Required on all note pages. Place immediately after the title block. Anchors must match
the `id="sN"` attributes on each `<h2>`.

```html
<nav class="toc">
  <div class="toc-title">Contents</div>
  <ol>
    <li><a href="#s1">Section One Title</a></li>
    <li><a href="#s2">Section Two Title</a></li>
  </ol>
</nav>
```

```css
.toc {
  background: var(--blue-box); border: 1px solid var(--blue-mid);
  border-radius: 4px; padding: 1rem 1.4rem; margin: 0 0 2.4rem; font-size: 0.88rem;
}
.toc-title {
  font-family: var(--mono); font-size: 0.72rem; font-weight: 500;
  color: var(--blue-dark); text-transform: uppercase; letter-spacing: 0.1em; margin-bottom: 0.5rem;
}
.toc ol { padding-left: 1.4rem; }
.toc li { margin-bottom: 0.25rem; }
.toc a { color: var(--blue-mid); text-decoration: none; }
.toc a:hover { text-decoration: underline; }
```

---

### Section headings

Auto-numbered via CSS counter. Always add an `id="sN"` matching the TOC anchor.

```html
<h2 id="s1">Section Title</h2>
```

```css
body { counter-reset: section; }
h2 {
  font-size: 1.12rem; font-weight: 700; color: var(--blue-dark);
  margin: 2.4rem 0 0.6rem; padding-bottom: 0.25rem;
  border-bottom: 1.5px solid var(--blue-mid);
}
h2::before { counter-increment: section; content: counter(section) ". "; }
h3 { font-size: 1rem; font-weight: 700; color: var(--blue-mid); margin: 1.4rem 0 0.4rem; }
p  { margin-bottom: 0.85rem; }
```

---

### Key-result box

For boxed equations that are results or definitions worth highlighting.
Left-border stripe only — do **not** use a full surrounding border.

```html
<div class="key-result">
  $$\sigma_x \geq \frac{\sigma_\mathrm{PSF}}{\sqrt{N}}$$
  <div class="eq-label">Thompson–Ober–Webb (2002)</div>
</div>
```

```css
.key-result {
  background: var(--blue-box);
  border-left: 4px solid var(--blue-mid);
  border-radius: 0 6px 6px 0;
  padding: 0.85rem 1.1rem 0.7rem;
  margin: 1rem 0 0.5rem;
}
.key-result .eq-label {
  font-family: var(--mono); font-size: 0.72rem;
  color: var(--gray-mid); text-align: right; margin-top: 0.25rem;
}
.display-eq { text-align: center; margin: 0.85rem 0; overflow-x: auto; }
```

---

### Callout boxes

For caveats, key conclusions, and warnings. A monospace label is auto-generated via
`::before`. Use `.callout.warn` for cautionary notes.

```html
<!-- informational (auto-label: "NOTE") -->
<div class="callout">
  <strong>Key point:</strong> …
</div>

<!-- warning (auto-label: "WARNING") -->
<div class="callout warn">
  <strong>Critical:</strong> …
</div>
```

```css
.callout {
  border-left: 3px solid var(--blue-mid);
  background: var(--blue-box);
  padding: 0.7rem 1rem; margin: 1rem 0;
  border-radius: 0 4px 4px 0; font-size: 0.93rem;
}
.callout::before {
  display: block; font-family: var(--mono);
  font-size: 0.68rem; letter-spacing: 0.1em;
  text-transform: uppercase; color: var(--blue-mid);
  margin-bottom: 0.3rem; content: "Note";
}
.callout.warn { border-left-color: #c0392b; }
.callout.warn::before { color: #c0392b; content: "Warning"; }
@media (prefers-color-scheme: dark) { .callout.warn { background: #2a1a1a; } }
```

---

### Derivation toggle

Collapsible step-by-step derivation. The button is **full-width** — do not use the
old pill-style (inline, `border-radius: 20px`). Button and panel share a continuous
border for a tab-panel appearance.

```html
<button class="deriv-btn" onclick="toggle(this)">
  <span class="arrow">&#9658;</span> Derive Eq. (1)
</button>
<div class="deriv-panel">
  <div class="step">
    <div class="step-label">Step 1 — …</div>
    <p>…</p>
  </div>
</div>
```

```css
.deriv-btn {
  display: flex; align-items: center; gap: 0.5rem;
  width: 100%; text-align: left;
  margin-top: 0.75rem; margin-bottom: 0;
  padding: 0.42rem 0.9rem;
  font-size: 0.85rem; font-family: var(--sans);
  color: var(--blue-derborder); background: var(--blue-der);
  border: 1px solid var(--blue-derborder);
  border-radius: 4px 4px 0 0; cursor: pointer;
}
.deriv-btn:hover { filter: brightness(0.95); }
.deriv-btn .arrow { font-size: 0.75rem; transition: transform 0.2s; display: inline-block; }
.deriv-btn.open .arrow { transform: rotate(90deg); }

.deriv-panel {
  display: none;
  border: 1px solid var(--blue-derborder); border-top: none;
  background: var(--blue-der); border-radius: 0 0 4px 4px;
  padding: 0.85rem 1.1rem 1rem;
  font-size: 0.91rem; margin-bottom: 0.8rem;
}
.deriv-panel.open { display: block; }
.deriv-panel p { margin-bottom: 0.7rem; }
.step { margin-bottom: 0.65rem; }
.step-label { font-weight: 700; color: var(--blue-derborder); font-size: 0.84rem; margin-bottom: 0.2rem; }
```

Required JavaScript — place once at the bottom of `<body>`:

```html
<script>
function toggle(btn) {
  btn.classList.toggle('open');
  const panel = btn.nextElementSibling;
  panel.classList.toggle('open');
  if (panel.classList.contains('open') && window.MathJax) {
    MathJax.typesetPromise([panel]).catch(console.error);
  }
}
</script>
```

---

### Tables

Solid blue-mid header with white text. Row hover on `tbody`. Wrap in `.table-wrap` for
horizontal scroll on mobile.

```html
<div class="table-wrap">
<table>
  <thead>
    <tr><th>Column A</th><th>Column B</th></tr>
  </thead>
  <tbody>
    <tr><td>…</td><td>…</td></tr>
  </tbody>
</table>
</div>
```

```css
.table-wrap { overflow-x: auto; margin: 1.2rem 0; }
table { width: 100%; border-collapse: collapse; font-size: 0.87rem; }
thead th { background: var(--blue-mid); color: #fff; padding: 8px 10px; text-align: left; font-weight: 600; }
tbody td { padding: 7px 10px; border-bottom: 1px solid var(--gray-rule); vertical-align: top; }
tbody tr:hover { background: var(--blue-box); }
tbody tr:last-child td { border-bottom: none; }
.mono-cell { font-family: var(--mono); font-size: 0.82em; }
.table-caption { font-size: 0.84rem; color: var(--gray-mid); margin-top: 0.5rem; line-height: 1.55; }
```

---

## New note checklist

When adding a note:

1. Copy the `<head>` block (font links + MathJax config + the full `<style>` above).
2. Set `max-width: 840px` (notes) or `1100px` (tables).
3. Add the nav breadcrumb and update peer note links in all existing pages.
4. Add the title block and TOC.
5. Number all `<h2>` with sequential `id="sN"` attributes matching the TOC.
6. Add the `toggle()` script at the bottom of `<body>`.
7. Update `index.html` to add a card for the new note.
8. Add the new note to the nav breadcrumbs of sibling pages.

---

## What not to do

| Anti-pattern | Why |
|---|---|
| Hard-coded dark colors (e.g. `background: #0e0f14`) | Breaks light mode |
| Serif or display fonts for body/headings | Inconsistent with the rest of the site |
| Full-border key-result box (border on all 4 sides) | Use `.callout` for that pattern instead |
| Pill-style derivation button (`border-radius: 20px`, inline) | Replaced by full-width tab style |
| `tbody tr:nth-child(even)` stripe without hover | Add hover; stripes alone are weaker |
| Animating section entrance (`fadeUp`, `opacity: 0`) | Distracting in a reference document |
| Inline `style.display` toggle in JavaScript | Use `classList.toggle('open')` instead |

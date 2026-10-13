# Personal Portfolio: Responsive Layout & Component Library

A multi-page personal portfolio built with **plain HTML and CSS** (no JavaScript, no frameworks). It focuses on a responsive grid layout, a reusable component library, automatic dark mode, and high accessibility standards.

> **Course assignment** | Student: Mohamed Maher | Course: `[course name]` | Instructor: `[instructor name]`

---

## Table of contents

1. [Assignment requirements](#assignment-requirements)
2. [Project structure](#project-structure)
3. [Getting started](#getting-started)
4. [Responsive breakpoints](#1-responsive-breakpoints)
5. [Component library](#2-component-library)
6. [Dark mode](#3-dark-mode)
7. [Accessibility audit](#4-accessibility-audit)
8. [Design tokens](#design-tokens)
9. [Known limitations](#known-limitations)

---

## Assignment requirements

| # | Requirement | Where it lives | Status |
|---|-------------|----------------|--------|
| 1 | Responsive media queries: **2 columns** on tablet, **3 columns** on desktop | `CSS/stylesheet.css` (section 5), demo in `component.html` and `about.html` | Done |
| 2 | Form elements (inputs, labels, buttons, selects) | `CSS/stylesheet.css` (sections 7, 8), demo in `component.html` and `contact.html` | Done |
| 2 | Modal window, **CSS only** | `CSS/stylesheet.css` (section 10), demo in `component.html` | Done |
| 2 | Alert / notification box | `CSS/stylesheet.css` (section 9), demo in `component.html` | Done |
| 3 | Dark mode with `@media (prefers-color-scheme: dark)` | `CSS/stylesheet.css` (section 1) | Done |
| 4 | Lighthouse Accessibility score above 90 | All pages | See [results](#lighthouse-results) |

---

## Project structure

```
.
├── index.html          Home page (intro and article)
├── about.html          Skills cards + strengths table
├── contact.html        Contact form
├── component.html      Component library showcase
└── CSS/
    └── stylesheet.css  Single stylesheet for every page
```

## Getting started

No build step or dependencies are needed.

1. Download or clone the project.
2. Open `index.html` in any modern browser (or use the VS Code *Live Server* extension).
3. Use the navigation bar to move between pages.

To test dark mode, change your OS theme, or in Chrome DevTools open **Rendering → Emulate CSS media feature `prefers-color-scheme`**.

---

## 1. Responsive breakpoints

The layout is **mobile-first**. The `.cards` grid adds columns as the screen grows:

| Screen | Breakpoint | Columns |
|--------|-----------|---------|
| Mobile | below 48em (768px) | 1 |
| Tablet | `min-width: 48em` (768px) | **2** |
| Desktop | `min-width: 64em` (1024px) | **3** |

```css
.cards { display: grid; grid-template-columns: minmax(0, 1fr); gap: 1rem; }

@media (min-width: 48em) { .cards { grid-template-columns: repeat(2, minmax(0, 1fr)); } }
@media (min-width: 64em) { .cards { grid-template-columns: repeat(3, minmax(0, 1fr)); } }
```

Other responsive details: fluid typography with `clamp()`, a flexible page gutter, a wrapping header/nav, and a table that scrolls horizontally inside its own container instead of breaking the page.

## 2. Component library

All components are documented live in **`component.html`**. They use shared design tokens, so they adapt to dark mode automatically.

### Form elements

Inputs, textareas, selects, checkboxes, radios, labels, hint text, and disabled states. Every control has an associated `<label>`, a minimum 44px touch target, and a visible focus ring.

```html
<div class="field">
  <label for="email">Email</label>
  <input type="email" id="email" name="email" autocomplete="email" aria-describedby="email-hint">
  <p class="hint" id="email-hint">I'll only use it to reply to you.</p>
</div>
```

### Buttons

```html
<button type="button">Primary</button>
<button type="button" class="btn--ghost">Secondary</button>
<button type="button" disabled>Disabled</button>
<a class="btn" href="#">Link styled as a button</a>
```

### Alert / notification

Four variants: default, `alert--success`, `alert--error`, `alert--warning`. Each includes a text label, so meaning never depends on colour alone. Use `role="alert"` for urgent errors and `role="status"` for everything else.

```html
<div class="alert alert--success" role="status">
  <p><strong>Success:</strong> Your changes were saved.</p>
</div>
```

### Modal window (CSS only)

The modal uses the `:target` pseudo-class: the open link points to the modal's `id`, and the close links point back to the page section. It is hidden with `visibility: hidden`, which removes it from the tab order and from screen readers until opened.

```html
<a class="btn" href="#demo-modal">Open modal</a>

<div class="modal" id="demo-modal" role="dialog" aria-modal="true" aria-labelledby="modal-title">
  <a class="modal__backdrop" href="#modal-demo" tabindex="-1" aria-hidden="true"></a>
  <div class="modal__dialog">
    <a class="modal__close" href="#modal-demo" aria-label="Close dialog">&times;</a>
    <h2 id="modal-title">Confirm your choice</h2>
    <p>Dialog content goes here.</p>
  </div>
</div>
```

## 3. Dark mode

Colours are CSS custom properties defined on `:root` for the light theme and overridden inside `@media (prefers-color-scheme: dark)`. The stylesheet also sets `color-scheme: light dark`, so native controls and scrollbars follow the theme. No toggle or JavaScript is needed: the page follows the user's system preference.

All text/background pairs were chosen to meet the WCAG AA contrast ratio (4.5:1) in both themes. For example, the light-mode highlight colour was darkened to `#8a4f00` because the original `#b26a00` was only about 3.9:1.

## 4. Accessibility audit

### What was fixed

- Valid, well-formed HTML (fixed a broken `<!DOCTYPE>`, stray `>` characters, a missing `<form>` tag, and mismatched `name` / `id` attributes).
- One `<h1>` per page with a logical heading order.
- Landmarks on every page: `<header>`, `<nav aria-label="Main">`, `<main>`, `<footer>`.
- A **skip to main content** link, visible on keyboard focus.
- `aria-current="page"` on the active navigation link.
- Descriptive page titles on every page.
- Lists use real `<ul>` / `<li>` elements.
- The data table has a `<caption>`, `scope` attributes, a labelled empty header cell, and a keyboard-focusable scroll region.
- Radio groups and checkbox groups use `<fieldset>` and `<legend>`.
- Colour contrast meets WCAG AA in light and dark mode.
- Visible `:focus-visible` outlines, `prefers-reduced-motion` support, and a viewport that allows zooming.

### How to run Lighthouse

1. Open a page in **Google Chrome**.
2. Press `F12` to open DevTools and select the **Lighthouse** tab.
3. Tick **Accessibility** only, choose **Mobile** or **Desktop**, then click **Analyze page load**.
4. Review any failed audits, fix them, and re-run.
5. Repeat for each page.

### Lighthouse results

Fill this table in after running the audit and add screenshots to a `screenshots/` folder.

| Page | Accessibility score |
|------|--------------------|
| `index.html` | __ / 100 |
| `about.html` | __ / 100 |
| `contact.html` | __ / 100 |
| `component.html` | __ / 100 |

---

## Design tokens

Defined once in `:root` and reused everywhere.

| Token | Light | Dark | Purpose |
|-------|-------|------|---------|
| `--bg` | `#f5f6fb` | `#0c1224` | Page background |
| `--surface` | `#ffffff` | `#141c38` | Cards, inputs, modal |
| `--surface-2` | `#eceffa` | `#1b2447` | Alerts, hover states |
| `--text` | `#182042` | `#e2e7f7` | Body text |
| `--muted` | `#55608a` | `#94a0c8` | Secondary text |
| `--accent` | `#3b4bd6` | `#8b9bff` | Links, buttons, borders |
| `--highlight` | `#8a4f00` | `#f2b45c` | Hover and focus accents |
| `--success` / `--danger` | `#0f6b30` / `#b3261e` | `#5fd38d` / `#ff8a80` | Alert variants |

## Known limitations

- The CSS-only modal cannot close with the **Esc** key or trap keyboard focus; both need JavaScript. A native `<dialog>` element would be the next step if scripting is allowed.
- The contact form is a static layout demo with no submit button or backend.

## Tech stack

HTML5, CSS3 (Grid, Flexbox, custom properties, media queries). No JavaScript, no libraries.

## Author

**Mohamed Maher**: beginner web developer learning HTML, CSS, and JavaScript.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page pricing/portfolio site for Martin Straňanek (SpinteQ web services), written in Slovak. Deployed via GitHub Pages at `darkmaster9452.github.io/Dark-s-Imperium`.

**There is no build step, no package manager, no test runner.** The entire site is one file: `index.html`.

To preview locally, open `index.html` directly in a browser (or use any static server, e.g. `python -m http.server`).

## Architecture

Everything lives in `index.html` in three parts:

1. **CSS** — inline `<style>` block. Design tokens defined as CSS custom properties in `:root` at the top (`--accent-gold`, `--bg-card`, etc.). All component styles follow below, organized by section with `/* === SECTION === */` comments.

2. **HTML** — structured as:
   - `#dotted-surface` — fixed Three.js canvas container (background animation)
   - `.pricing-section` tabs — two sections toggled by `.toggle-btn`: `#section-tvorba` (web creation packages) and `#section-sprava` (management packages with monthly/annual billing)
   - `.extras` — additional services grid
   - `.process`, `.faq`, `.trust-banner`, `.contact`, `.site-footer`
   - Cart UI — `#cart-fab` (fixed button), `#cart-drawer` (slide-in panel), `#cart-overlay`

3. **JavaScript** — inline `<script>` blocks at the bottom, each in an IIFE:
   - **Tab toggle** — switches between Tvorba/Správa sections, re-triggers card animations
   - **Billing toggle** — monthly/annual price swap using `data-monthly`/`data-annual` attributes on `.card-price` elements
   - **FAQ accordion** — single-open accordion
   - **Scroll-reveal** — IntersectionObserver adds `.revealed` to `.scroll-reveal` elements
   - **Three.js dot grid** — animated wave of points in `#dotted-surface`
   - **Cart** — manages `cart[]` array in memory; "add to cart" buttons use `data-add-to-cart`, `data-id`, `data-name`, `data-price` (or `data-price-ref` to look up the currently-active billing price); submit opens a pre-filled Gmail compose URL

## Key Patterns

- Prices for "Správa" packages are driven by `data-monthly` / `data-annual` attributes, not hardcoded text — update those attributes, not the visible text.
- Cart items with `data-price-ref` read the price from a DOM element at submit time, so billing-toggle state is reflected automatically.
- The `.scroll-reveal` class hides an element by default; `IntersectionObserver` adds `.revealed` when it enters the viewport. Apply this class to new sections.
- External dependencies loaded from CDN: Three.js r128, Font Awesome 6.4.0, Google Fonts (Cormorant + Work Sans).

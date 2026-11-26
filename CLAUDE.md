# The Potato

A single-page, scroll-driven site all about potatoes, built around an interactive 360° 3D potato in the hero.

## Stack
- One self-contained file, `index.html`, holds the inline `<style>` and two inline `<script>` blocks. There is no build step and no package.json.
- Three.js 0.160.0 loads from jsDelivr through the `<script type="importmap">`. Fonts are Fraunces (headings) and Inter (body) from Google Fonts.
- Keep it one file with vanilla JS. Ask before adding a framework, bundler, CSS library, npm package or extra files.

## Run and verify
- Serve over HTTP, not `file://`: `python3 -m http.server 8000`, then open http://localhost:8000.
- Before calling a change done:
  - The browser console shows zero errors.
  - The 3D potato renders, spins, and responds to zoom, reset and variety-pick.
  - Screenshot the changed section at 375px and 1280px wide. There must be no horizontal scroll.
- Report the screenshot or console output as evidence.

## How the file is organized
- `<style>`: design tokens live in `:root`. Breakpoints are `980px` and `640px`. There is also a `prefers-reduced-motion` block.
- First `<script>` (CONTENT): most sections are **rendered from JS data arrays** into empty containers (`#varGrid`, `#timeline`, `#cookGrid`, `#factsGrid`, `#bars`, `#dishes`, the nutrition label). To change content, edit the data array, not the HTML.
- Second `<script type="module">` (3D POTATO): procedural geometry from seeded simplex noise (`mulberry32(1847)`), plus eyes, scuffs, OrbitControls and the renderer.

## Conventions
- Colors, radius and shadow come from the `:root` tokens (`--paper`, `--ink`, `--gold`, `--bark`, etc.). Don't hardcode new hex values in CSS. Add a token instead.
- Headings use `Fraunces`. Each section heading follows the `<h2>Plain text <em>italic accent.</em></h2>` pattern.
- A new section follows the existing shape: `<section id="...">` inside `.wrap`, plus a matching link in `<nav> .links`. The nav highlights the active link by matching `href="#id"`.
- Animate-on-scroll elements use the `.reveal` class. Bar widths use the `data-w` attribute (a percentage).
- Use `<button>` for interactive controls and give every icon-only button a `title` or `aria-label`.
- Nutrition numbers are per 100 g, in `PER100`. `DV` holds FDA daily values. Keep facts and figures accurate and don't invent statistics.

## Gotchas
- `.reveal` elements stay invisible (`opacity:0`) until the IntersectionObserver near the end of the content script adds the `.in` class. That observer runs once. Any dynamically rendered `.reveal` element must be inserted **before** that block, or it never appears.
- The RNG seed `1847` makes the potato's shape deterministic. Changing the seed, or the order of `rand()` calls, changes the potato's shape.
- Rendering pauses when the stage is off-screen (an IntersectionObserver on `#stage`). Keep that behavior for performance.
- There is no dark mode. The light paper look is intentional.

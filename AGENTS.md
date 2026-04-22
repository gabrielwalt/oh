# AGENTS.md — Grown in Idaho EDS Project

> **TOP PRIORITY — Self-Maintenance Rules:**
> 1. **Keep this file up-to-date.** After every significant change, learning, or mistake, update this file with relevant instructions. Remove stale info. Keep it lean — only what matters for making good decisions.
> 2. **Keep PROJECT.md up-to-date.** After every change to blocks, styles, tokens, content, or project structure, update `/workspace/PROJECT.md` with the current state. Load PROJECT.md into context when you need project-specific details (blocks, variants, tokens, etc.).
> 3. **Learn from mistakes.** When something breaks or you take a wrong approach, add a brief note here so you don't repeat it.

## Project Overview

This is an **AEM Edge Delivery Services** site for **Grown in Idaho** (https://growninidaho.com/), a Lamb Weston brand for Idaho potato products. Built from `aem-boilerplate`, initialized via Slicc.

See **[PROJECT.md](/workspace/PROJECT.md)** for full project state: blocks, variants, CSS tokens, content pages, and design details.

## Key Technologies
- Edge Delivery Services (docs: https://www.aem.live/)
- Vanilla JS (ES6+), no transpiling, no build steps
- CSS3 with custom properties, no frameworks
- Node.js tooling

## Commands
- `npm install` — install deps
- `aem up` or `npx -y @adobe/aem-cli up --no-open --forward-browser-logs` — dev server at http://localhost:3000
- `npm run lint` / `npm run lint:fix` — lint

## Project Structure
```
blocks/{name}/{name}.js|css   — Block implementations
styles/styles.css             — Global styles + CSS tokens (imports brand.css)
styles/brand.css              — Brand design tokens
styles/lazy-styles.css        — Post-LCP styles
styles/fonts.css              — Font definitions
scripts/aem.js                — Core AEM lib (NEVER MODIFY)
scripts/scripts.js            — Page decoration entry point
scripts/delayed.js            — Delayed loading
content/                      — Content .plain.html files + images
content/*-preview.html        — Preview pages for dev server
drafts/                       — Slicc-generated source files
```

## Code Style
- **JS:** ES6+, Airbnb ESLint, `.js` extensions in imports, LF line endings
- **CSS:** Stylelint standard, mobile-first with `min-width` breakpoints at 600/900/1200px, selectors scoped to block (`.blockname .child`, never bare `.child`)
- Avoid `-container` / `-wrapper` classes (reserved for sections)
- **HTML:** Semantic HTML5, ARIA labels, proper heading hierarchy

## Block Development
- Blocks export `default async function decorate(block) {}`
- Block HTML structure = the author-developer contract — change carefully
- Inspect markup with `curl` before coding; handle optional fields gracefully
- Each block is self-contained with its own `.js` and `.css`

## Three-Phase Loading
1. **Eager** — LCP-critical: sections, blocks, buttons, first section
2. **Lazy** — Rest of content, header, footer
3. **Delayed** — Martech, non-essential scripts

## Content & Previewing
- Content lives in `content/` as `.plain.html` files
- Preview at `http://localhost:3000/content/home-preview` (full page with header/footer)
- Preview files need `<meta name="nav" content="/content/nav">` and `<meta name="footer" content="/content/footer">`
- Typekit ID: `eze8bto`

## Performance & Accessibility
- Optimize images, lazy-load non-critical resources
- No unnecessary dependencies; use automatic code splitting via `/blocks/`
- WCAG 2.1 AA, proper heading hierarchy, alt text on images

## Deployment
- **Preview:** `https://main--{repo}--{owner}.aem.page/`
- **Live:** `https://main--{repo}--{owner}.aem.live/`
- **Branch:** `https://{branch}--{repo}--{owner}.aem.page/`
- PRs must include a preview URL demonstrating the change

## Lessons Learned
<!-- Add entries here as you learn from mistakes -->
- Slicc projects: content is already structured — don't re-run migration workflows, just wire up content files
- Slicc setup only needs index.plain.html, nav.plain.html, footer.plain.html + images — don't import preview HTML files
- Don't duplicate nav/footer to project root — use `<meta name="nav">` and `<meta name="footer">` in head.html to point to content versions

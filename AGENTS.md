# AGENTS.md — Grown in Idaho EDS Project

> **TOP PRIORITY — Self-Maintenance Rules:**
> 1. **Keep this file up-to-date.** After every significant change, learning, or mistake, update this file with relevant instructions. Remove stale info. Keep it lean — only what matters for making good decisions.
> 2. **Keep PROJECT.md up-to-date.** After every change to blocks, styles, tokens, content, or project structure, update `/workspace/PROJECT.md` with the current state. Load PROJECT.md into context when you need project-specific details (blocks, variants, tokens, etc.).
> 3. **Learn from mistakes.** When something breaks or you take a wrong approach, add a brief note here so you don't repeat it.

## Project Overview

This is an **AEM Edge Delivery Services** site for **Grown in Idaho** (https://growninidaho.com/). See **[PROJECT.md](/workspace/PROJECT.md)** for full project state: structure, blocks, variants, CSS tokens, content pages, deployment URLs, and design details.

## Commands
- `npm install` — install deps
- `aem up` or `npx -y @adobe/aem-cli up --no-open --forward-browser-logs` — dev server at http://localhost:3000
- `npm run lint` / `npm run lint:fix` — lint

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

## Performance & Accessibility
- Optimize images, lazy-load non-critical resources
- No unnecessary dependencies; use automatic code splitting via `/blocks/`
- WCAG 2.1 AA, proper heading hierarchy, alt text on images
- PRs must include a preview URL demonstrating the change

## Lessons Learned
<!-- Add entries here as you learn from mistakes -->
- Slicc projects: content is already structured — don't re-run migration workflows, just wire up content files
- Slicc setup only needs index.plain.html, nav.plain.html, footer.plain.html + images — don't import preview HTML files
- NEVER store content files (nav, footer, pages) in the code repo root — content belongs in the CMS, not in git. The `content/` folder is for local dev only.
- Nav/footer are authored as CMS documents at `/nav` and `/footer` in production (da.live). For local dev, the `content/` folder has copies but they should NOT be duplicated to the project root.
- NEVER create preview HTML files (`*-preview.html`) — they are unnecessary artifacts

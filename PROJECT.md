# PROJECT.md — Grown in Idaho (EDS)

> **Keep this file current.** Update after every change to blocks, styles, tokens, content, or project configuration.

## Site Info
- **Original site:** https://growninidaho.com/
- **Brand:** Grown in Idaho® — a Lamb Weston Holdings brand
- **Product focus:** Frozen Idaho potato products (fries, tots, hash browns)

## Content Pages

| Page | Content File | Preview URL |
|------|-------------|-------------|
| Homepage | `content/index.plain.html` | `/content/` |
| Header | `content/nav.plain.html` | (loaded via head.html meta tag) |
| Footer | `content/footer.plain.html` | (loaded via head.html meta tag) |

**Nav/Footer routing:** `head.html` has `<meta name="nav" content="/content/nav">` and `<meta name="footer" content="/content/footer">`. No root-level copies.

## Blocks

| Block | Status | Description |
|-------|--------|-------------|
| `hero` | Styled | Full-bleed background image with text overlay, primary (yellow) / secondary (outline) CTA buttons |
| `feature-split` | Styled | Product image left + badge/text right on beige texture background (`texture-beige.jpg`) |
| `product-slider` | Styled | Horizontal product carousel on lightbeige texture. Brown cards, nav arrows, 3 visible (desktop), 1 (mobile) |
| `recipe-cards` | Styled | 4-across (desktop) / 2x2 (mobile) recipe cards on dark brown texture. Cards have lightbeige content area |
| `social-feed` | Styled | Instagram feed grid (4-col desktop, 2-col mobile) on lightbeige texture. Header with CTA button |
| `header` | Styled | Fixed dark brown nav bar with logo left, nav links right. Hamburger on mobile |
| `footer` | Styled | Mountains background image, logo, social icons, legal links, copyright |
| `columns` | Available | Generic column layout (boilerplate) |
| `cards` | Available | Generic card layout (boilerplate) |
| `fragment` | Available | Fragment loader (boilerplate) |

## Section Backgrounds

| Section | Background | Source |
|---------|-----------|--------|
| Hero | Hero image (in content) | `homepageheroimage-d.jpg` |
| Feature Split | `.section.feature-split-container` | `texture-beige.jpg` |
| Product Slider | `.section:has(.product-slider)` | `texture-lightbeige.jpg` |
| Recipe Cards | `.section.recipe-cards-container` | `texture-darkbrown.jpg` |
| Social Feed | `.section.social-feed-container` | `texture-lightbeige.jpg` |
| Footer | `footer` element | `mountains.jpg` |

All background images stored in `content/images/`.

## CSS Design Tokens (`styles/brand.css` + `styles/styles.css`)

### Colors
| Token | Value | Usage |
|-------|-------|-------|
| `--background-color` | `rgb(255 255 255)` | Page background |
| `--text-color` | `rgb(103 103 103)` | Body text |
| `--link-color` | `rgb(43 16 10)` | Link text (dark brown) |
| `--link-hover-color` | `inherit` | Link hover |
| `--light-color` | `#f8f8f8` | Light section background |
| `--dark-color` | `#505050` | Dark accents |

### Hard-coded Brand Colors (in block CSS)
| Color | Usage |
|-------|-------|
| `#ffd400` | Primary CTA yellow (hero buttons) |
| `#2b100a` | Dark brown (hero button text, nav bar, footer text) |
| `#fffbee` | Cream white (hero text, nav text) |
| `#f5edd6` | Light beige (section backgrounds fallback) |
| `rgb(108 63 34)` | Product card brown |
| `rgb(255 241 199)` | Product card text / light gold |

### Typography
| Token | Value |
|-------|-------|
| `--heading-font-family` | `"brothers", serif` |
| `--body-font-family` | `"avenir-lt-pro", sans-serif` |
| `--body-font-size-m` | `22px` (mobile) / `18px` (desktop) |
| `--heading-font-size-xxl` | `56px` |
| Typekit ID | `eze8bto` |

### Layout
| Token | Value |
|-------|-------|
| `--nav-height` | `90px` |
| `--section-padding` | `0` |
| Sections | Full-bleed, no gaps between sections |
| Content max-width | `1200px` (inside blocks) |

## scripts.js Customizations
- Custom `decorateButtons()` — requires `<strong>` (primary) or `<em>` (secondary) wrapping for buttonization; bare links are not converted
- `buildHeroBlock()` — auto-blocks hero from h1+picture, but skips if already inside `.hero`
- Fragment auto-loading for `/fragments/` links

## Project Setup
- **Origin:** Slicc (Chrome extension) — generated blocks and content from growninidaho.com
- **Slicc setup status:** Complete — content wired up, backgrounds applied, blocks critiqued and improved

## Known Lint Issues
- 3 pre-existing `no-descending-specificity` warnings in hero.css, social-feed.css, styles.css (inherited from Slicc generation, cosmetic only)

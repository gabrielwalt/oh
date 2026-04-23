# Slicc Project Setup

Convert a Slicc-generated project into a working AEM Edge Delivery Services local environment.

## What is Slicc?

Slicc is the "Experience Modernization Agent" — a Chrome browser extension that:
1. Generates a project from `aem-boilerplate` (https://github.com/adobe/aem-boilerplate)
2. Creates a `drafts/` folder with EDS document format (`.plain.html` files)
3. Decomposes the source page into individual blocks (one preview page per block)
4. Styles each block (including header and footer) with CSS
5. Assembles all blocks into a full page (e.g. `home.plain.html`)

## When to use this skill

Trigger when ANY of these conditions are true:
- `content/` folder is empty but `drafts/` folder has `.plain.html` files
- User mentions "Slicc" or asks to initialize/set up the project
- User asks to create content from drafts
- User says the content folder is empty or nothing shows up

## Project structure (Slicc output)

```
drafts/
├── home.plain.html              # Assembled full page
├── nav.plain.html               # Navigation content
├── footer.plain.html            # Footer content
├── hero.plain.html              # Individual block content
├── {block-name}.plain.html      # More blocks...
├── *-preview.html               # Preview pages (ignore these)
└── images/                      # All local images
```

## Setup workflow

### Step 1: Inventory the drafts folder

```bash
ls drafts/*.plain.html
ls drafts/images/
```

Identify:
- The **assembled page** (usually `home.plain.html` — the largest file with all blocks)
- The **nav** file (`nav.plain.html`)
- The **footer** file (`footer.plain.html`)
- The **images** folder

### Step 2: Create content folder structure

```bash
mkdir -p content/images
cp -r drafts/images/* content/images/
```

### Step 3: Create content .plain.html files

For each assembled page (e.g. `home.plain.html`):

1. Read `drafts/home.plain.html`
2. Create `content/index.plain.html` with the **exact same markup**
3. Replace all image paths: `/drafts/images/` → `/content/images/`
4. Do NOT change any block structure, class names, or HTML nesting

For nav and footer:
1. Copy `drafts/nav.plain.html` → `content/nav.plain.html` (fix image paths)
2. Copy `drafts/footer.plain.html` → `content/footer.plain.html` (fix image paths)

**CRITICAL: Do NOT modify the HTML structure. The blocks are already styled and working. Only change image paths.**

**Do NOT create or copy preview HTML files (`*-preview.html`). They are Slicc development artifacts and not needed.**

### Step 4: Add `getContentRoot()` to scripts.js and update header/footer blocks

Nav and footer are **content, not code** — they must never be stored in the code repo. Instead, add a
`getContentRoot()` helper to `scripts.js` that resolves fragment paths relative to the current page's
directory. This way nav/footer resolve correctly everywhere — local dev and production — without
root-level file copies or meta tag overrides.

**Add to `scripts.js`** (before `buildHeroBlock`):

```js
export function getContentRoot() {
  const { pathname } = window.location;
  const segments = pathname.split('/').filter(Boolean);
  if (segments.length > 1) {
    return `/${segments.slice(0, -1).join('/')}`;
  }
  return '';
}
```

**Update `blocks/header/header.js`:**
1. Add import: `import { getContentRoot } from '../../scripts/scripts.js';`
2. Change nav path resolution to: `const navPath = navMeta ? new URL(navMeta, window.location).pathname : \`\${getContentRoot()}/nav\`;`

**Update `blocks/footer/footer.js`:**
1. Add import: `import { getContentRoot } from '../../scripts/scripts.js';`
2. Change footer path resolution to: `const footerPath = footerMeta ? new URL(footerMeta, window.location).pathname : \`\${getContentRoot()}/footer\`;`

**How it works:**
- Page at `/content/` → fetches `/content/nav.plain.html` and `/content/footer.plain.html`
- Page at `/content/about` → fetches `/content/nav.plain.html` and `/content/footer.plain.html`
- Page at `/` (production) → fetches `/nav.plain.html` and `/footer.plain.html` from the CMS

**Do NOT:**
- Copy nav/footer files to the project root
- Add `<meta name="nav">` or `<meta name="footer">` tags to head.html or preview files
- Store any content files in the code repo root

### Step 5: Verify

1. Navigate to `http://localhost:3000/content/` (serves `index.plain.html`)
2. Confirm header renders with logo and navigation
3. Confirm all blocks render correctly
4. Confirm footer renders with links and copyright
5. Confirm all images load

## Key rules

- **Never modify block HTML structure** — Slicc has already styled the blocks; changing structure breaks styling
- **Only change image paths** — `/drafts/images/` → `/content/images/`
- **Don't import preview files** — only `index.plain.html`, `nav.plain.html`, and `footer.plain.html` are needed
- **Nav/footer are content, not code** — never store them in the code repo root. Use `getContentRoot()` so header/footer blocks resolve paths dynamically
- **The assembled page becomes `index.plain.html`** — the homepage lives at `content/index.plain.html`
- **Don't run the full migration workflow** — Slicc already did the block decomposition and styling. Just copy and wire up the content.

## Checklist

```
- [ ] content/images/ populated from drafts/images/
- [ ] content/index.plain.html created from drafts/home.plain.html (paths fixed)
- [ ] content/nav.plain.html created from drafts/nav.plain.html (paths fixed)
- [ ] content/footer.plain.html created from drafts/footer.plain.html (paths fixed)
- [ ] getContentRoot() added to scripts.js
- [ ] header.js updated to use getContentRoot() for nav path
- [ ] footer.js updated to use getContentRoot() for footer path
- [ ] Preview verified — header, blocks, footer all render
```

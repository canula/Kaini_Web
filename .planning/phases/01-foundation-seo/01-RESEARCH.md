# Phase 1: Foundation & SEO - Research

**Researched:** 2026-02-26
**Domain:** Static HTML, Tailwind CSS v4 CDN, SEO meta tags, GitHub Pages deployment
**Confidence:** HIGH

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

- **Dark palette:** Deep navy/charcoal background, vibrant/electric orange accent (TC-01 brand color), subtle gradients, clean sans-serif (Inter, DM Sans, or similar)
- **Logos:** TC-01 logo prominent, Kaini logo in footer — user will provide logo files
- **Section order:** Hero > Features > Gallery > Web App > Contact
- **Nav:** "TC-01" text with "Tone Calculator" context. Real section IDs from day one (#features, #gallery, #web-app, #contact)
- **Section stubs:** Heading title + short placeholder text; subtle background alternation between sections
- **Mobile nav:** Hamburger below 768px using vanilla JS toggle (no framework, no CSS-only hack)
- **SEO keywords:** Primary "Melody Generator App"; secondary "melody creator", "song maker app"
- **Title tag:** "Melody Generator App | TC-01 Tone Calculator"
- **Meta description:** ease-focused messaging ("generate melodies in seconds, no music theory needed" direction)
- **Copy tone:** Professional & polished; audience: beginners AND hobbyist musicians equally
- **Custom domain:** kaini-instruments.com (used for canonical, CNAME, OG url)
- **JSON-LD:** SoftwareApplication schema, category MusicApplication, OS: Android + Windows + Web, pricing: freemium (free download + optional Pro license key)
- **OG image:** Placeholder path og-image.png
- **Footer:** "© 2026 Kaini Instruments. All rights reserved.", Kaini logo, placeholder Privacy Policy and Terms of Service links (href="#"), no social links
- **Favicon:** PNG (user provides), apple-touch-icon.png, browser theme-color dark navy
- **Infrastructure:** Single index.html, Tailwind CSS v4 via CDN, no build process, vanilla JS only

### Claude's Discretion

- Exact accent color shade (vibrant/electric orange on navy — Claude picks the hex)
- Text color hierarchy (white headings, gray body — Claude picks exact values)
- Nav bar behavior (sticky vs static)
- Hamburger menu open animation style
- Section stub min-height
- Footer layout and content arrangement
- Nav label text for section links
- OG image placeholder approach

### Deferred Ideas (OUT OF SCOPE)

None — discussion stayed within phase scope
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| INFR-01 | Single index.html using Tailwind CSS v4 via CDN (no build process) | Tailwind v4 CDN script tag documented; @theme directive for custom colors confirmed |
| INFR-02 | Vanilla JavaScript only (no frameworks) | Hamburger toggle pattern documented; no library needed |
| INFR-03 | CNAME file committed to repo root for custom domain | GitHub Pages CNAME file format confirmed |
| INFR-04 | .nojekyll file committed to repo root | .nojekyll disables Jekyll processing; single empty file |
| INFR-05 | Dark color palette with clean modern marketing design | Tailwind v4 @theme custom colors; CSS variable pattern documented |
| SEO-01 | Optimized title and meta description for primary keywords | HTML meta tag patterns confirmed; character limits documented |
| SEO-02 | Semantic h1/h2 tags targeting primary keywords | One h1 per page rule; h2 per section confirmed as best practice |
| SEO-03 | Open Graph tags (og:title, og:description, og:image, og:url) | OG tag spec fetched from ogp.me; all four required tags documented |
| SEO-04 | Canonical URL tag pointing to custom domain | rel="canonical" pattern confirmed; must use absolute HTTPS URL |
| SEO-05 | JSON-LD SoftwareApplication schema for Google rich results | Schema properties confirmed; MusicApplication category note documented |
</phase_requirements>

---

## Summary

Phase 1 is a pure HTML/CSS/vanilla JS scaffolding task with no backend, no build tooling, and no dynamic content. The entire deliverable is a single `index.html` file plus two deployment marker files (`CNAME`, `.nojekyll`). All technical choices have been locked by the user; this phase is well-understood with HIGH confidence.

The most important implementation nuance is Tailwind CSS v4's CDN delivery model. The official docs label it "development only" due to runtime browser compilation (~300KB+ JS bundle), but this is a conscious project constraint (INFR-01 explicitly bans build tooling). For a low-traffic marketing page on GitHub Pages, the CDN tradeoff is acceptable. The v4 CDN supports custom color tokens via `<style type="text/tailwindcss">` and the `@theme` directive, enabling the full brand color system without a build step.

The second nuance is the JSON-LD SoftwareApplication schema. Google's verified rich-results documentation lists specific `applicationCategory` values (GameApplication, BusinessApplication, etc.) and `MusicApplication` is not on Google's confirmed list. However, Schema.org itself defines `applicationCategory` as free-form text — `"MusicApplication"` is valid schema.org and follows the same naming pattern as Google's listed types. Use it; the schema is still valid and crawlable even if it doesn't unlock a specific Google rich result template.

**Primary recommendation:** Build `index.html` with Tailwind v4 CDN + `@theme` for brand colors. Wire all five real section IDs, complete head metadata, and commit CNAME/nojekyll. Nothing else belongs in this phase.

---

## Standard Stack

### Core

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Tailwind CSS (browser CDN) | @tailwindcss/browser@4 (v4.x) | Utility-first CSS, no build | Project constraint INFR-01; fastest no-build path |
| Vanilla JavaScript | ES2020+ (browser native) | Hamburger menu toggle | Project constraint INFR-02 |

### Supporting

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| Google Fonts (CDN) | Latest | Inter or DM Sans typeface | Load via `<link>` preconnect pattern in `<head>` |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| Tailwind CDN | Tailwind CLI build | Better production perf, but violates INFR-01 no-build constraint |
| Tailwind CDN | Plain CSS custom properties | Full control, no external dep, but more verbose; user chose Tailwind |
| Vanilla JS toggle | CSS-only checkbox hack | No JS required, but user explicitly rejected CSS-only approach |

**Installation:**

No npm/installation step. Add to `<head>`:
```html
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
```

---

## Architecture Patterns

### Recommended Project Structure

```
/                           # repo root
├── index.html              # single deliverable for this phase
├── CNAME                   # "kaini-instruments.com" (one line, no trailing newline issues)
├── .nojekyll               # empty file, disables GitHub Pages Jekyll processing
├── assets/
│   ├── images/
│   │   ├── tc01-logo.png       # placeholder path (user provides)
│   │   ├── kaini-logo.png      # placeholder path (user provides)
│   │   ├── favicon.png         # placeholder path (user provides)
│   │   ├── apple-touch-icon.png # 180x180 (user provides)
│   │   └── og-image.png        # placeholder (user provides later)
```

### Pattern 1: Tailwind v4 Custom Color Tokens via @theme

**What:** Define brand colors as CSS custom properties inside a `<style type="text/tailwindcss">` block. Tailwind v4 picks these up and generates utility classes automatically.

**When to use:** Any time project requires custom brand colors without a build step.

**Example:**
```html
<!-- Source: https://tailwindcss.com/docs/installation/play-cdn -->
<style type="text/tailwindcss">
  @theme {
    --color-accent: #f97316;       /* electric orange — Claude to finalize exact shade */
    --color-surface: #0f1623;      /* deep navy background */
    --color-surface-alt: #141e2e;  /* slightly lighter for section alternation */
    --color-heading: #ffffff;
    --color-body: #94a3b8;
  }
</style>

<!-- Usage: -->
<body class="bg-surface text-body">
  <h1 class="text-heading">...</h1>
  <button class="bg-accent text-white">...</button>
</body>
```

### Pattern 2: Semantic Section Stubs

**What:** Each section uses the correct HTML5 landmark elements with real IDs. Stubs include an `<h2>` (required for valid section semantics) and placeholder paragraph.

**Example:**
```html
<!-- One h1 on the page (in Hero) -->
<section id="features" class="py-24 bg-surface-alt">
  <div class="max-w-6xl mx-auto px-6">
    <h2 class="text-3xl font-bold text-heading">Features</h2>
    <p class="text-body mt-4">Coming soon.</p>
  </div>
</section>
```

### Pattern 3: Vanilla JS Hamburger Toggle (Accessible)

**What:** Single button with `aria-expanded` and `aria-controls` attributes; JS toggles class and ARIA state together.

**Example:**
```html
<!-- HTML -->
<button
  id="nav-toggle"
  aria-controls="mobile-menu"
  aria-expanded="false"
  aria-label="Toggle navigation"
  class="md:hidden p-2"
>
  <!-- hamburger icon (3 spans or SVG) -->
</button>
<nav id="mobile-menu" class="hidden md:flex gap-6">
  <a href="#features">Features</a>
  <!-- ... -->
</nav>

<script>
  const btn = document.getElementById('nav-toggle');
  const menu = document.getElementById('mobile-menu');
  btn.addEventListener('click', () => {
    const expanded = btn.getAttribute('aria-expanded') === 'true';
    btn.setAttribute('aria-expanded', String(!expanded));
    menu.classList.toggle('hidden');
  });
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') {
      btn.setAttribute('aria-expanded', 'false');
      menu.classList.add('hidden');
    }
  });
</script>
```

### Pattern 4: Complete Head Metadata Block

**What:** Full `<head>` with all SEO, OG, favicon, and canonical tags in one ordered block.

**Example:**
```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Primary SEO -->
  <title>Melody Generator App | TC-01 Tone Calculator</title>
  <meta name="description" content="Generate melodies in seconds — no music theory needed. TC-01 Tone Calculator is a free melody generator app for Android, Windows, and Web." />
  <link rel="canonical" href="https://kaini-instruments.com/" />

  <!-- Open Graph -->
  <meta property="og:type" content="website" />
  <meta property="og:title" content="Melody Generator App | TC-01 Tone Calculator" />
  <meta property="og:description" content="Generate melodies in seconds — no music theory needed." />
  <meta property="og:image" content="https://kaini-instruments.com/assets/images/og-image.png" />
  <meta property="og:url" content="https://kaini-instruments.com/" />
  <meta property="og:site_name" content="TC-01 Tone Calculator" />
  <meta property="og:locale" content="en_US" />

  <!-- Favicon -->
  <link rel="icon" href="/assets/images/favicon.png" type="image/png" />
  <link rel="apple-touch-icon" href="/assets/images/apple-touch-icon.png" />
  <meta name="theme-color" content="#0f1623" />

  <!-- JSON-LD -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "SoftwareApplication",
    "name": "TC-01 Tone Calculator",
    "applicationCategory": "MusicApplication",
    "operatingSystem": "Android, Windows, Web",
    "offers": {
      "@type": "Offer",
      "price": "0",
      "priceCurrency": "USD",
      "description": "Free download with optional Pro license key"
    },
    "url": "https://kaini-instruments.com/",
    "publisher": {
      "@type": "Organization",
      "name": "Kaini Instruments"
    }
  }
  </script>

  <!-- Tailwind CSS v4 CDN -->
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <style type="text/tailwindcss">
    @theme {
      --color-accent: #f97316;
      --color-surface: #0f1623;
      --color-surface-alt: #141e2e;
      --color-heading: #ffffff;
      --color-body: #94a3b8;
    }
  </style>
</head>
```

### Anti-Patterns to Avoid

- **Skipping section IDs now:** Using placeholder IDs like `#section-1` and renaming in Phase 2 will break anchor links. Use real IDs (#features, #gallery, #web-app, #contact) from day one.
- **CSS-only hamburger (checkbox hack):** User explicitly rejected this. Use vanilla JS toggle.
- **Relative canonical URL:** `<link rel="canonical" href="/">` — Google requires absolute URLs. Use `https://kaini-instruments.com/`.
- **Multiple h1 tags:** One h1 per page. Hero headline is the h1. All section titles are h2.
- **Putting JSON-LD in body:** Place `<script type="application/ld+json">` inside `<head>`, not in `<body>`.
- **CNAME with trailing whitespace or newline issues:** CNAME must contain exactly one line: `kaini-instruments.com` — no leading/trailing spaces, no `www.`, no protocol.
- **Jekyll processing interference:** Without `.nojekyll`, GitHub Pages tries to process files starting with `_`. The empty `.nojekyll` file disables this globally.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| CSS utility classes | Custom `.flex-center`, `.text-orange`, etc. | Tailwind v4 utilities + @theme tokens | Tailwind's JIT covers every case; custom utilities conflict with CDN compilation |
| Hamburger open/close | Complex state machine | Single `classList.toggle('hidden')` + aria-expanded toggle | The pattern is 8 lines; anything more is over-engineering for a stub |
| OG image | Generated canvas/server image | Static og-image.png placeholder with real asset later | Phase 1 only needs the path referenced; real image is Phase 2 content work |

**Key insight:** This phase is scaffolding. Every hand-rolled solution adds Phase 2 cleanup debt. Keep stubs truly minimal — headings + 1 sentence placeholder per section.

---

## Common Pitfalls

### Pitfall 1: Tailwind CDN Script Tag Version Drift

**What goes wrong:** Using `@tailwindcss/browser@latest` resolves to whatever jsDelivr serves; a future breaking v5 could silently break styles.

**Why it happens:** Unpinned CDN versions auto-update.

**How to avoid:** Pin to major version: `@tailwindcss/browser@4` (locks to latest v4.x). This is what the official docs show and is a stable, non-breaking target.

**Warning signs:** Utility class names changing behavior, `@theme` syntax errors in console.

### Pitfall 2: OG Image Must Be an Absolute URL

**What goes wrong:** `og:image` set to a relative path (`/assets/images/og-image.png`) — Facebook/LinkedIn scrapers fail to find the image.

**Why it happens:** OG scrapers don't follow HTML base tags; they need full URLs.

**How to avoid:** Always use `https://kaini-instruments.com/assets/images/og-image.png` in the meta tag. Even with a placeholder file, the tag value must be absolute.

### Pitfall 3: Section Without Heading Fails Semantic Validation

**What goes wrong:** Section stubs that contain only a paragraph or a div — no heading element. HTML validators warn; search engines may not parse section context correctly.

**Why it happens:** Shortcutting the stub to the minimum visible output.

**How to avoid:** Every `<section>` must have at least an `<h2>`. Section stubs must include `<h2>Section Title</h2>` even if all other content is placeholder.

### Pitfall 4: CNAME File Gets Wiped on GitHub Pages Deploy

**What goes wrong:** If using a GitHub Actions workflow that deploys a build output directory, the CNAME file in repo root is not automatically included.

**Why it happens:** GitHub Pages from branch deploys include the full branch; GitHub Actions workflows that deploy a dist folder require the CNAME to be inside that dist folder or copied by the workflow.

**How to avoid:** Since this project deploys the repo root directly (no build step), committing CNAME to the repo root is correct and sufficient. No special handling needed.

### Pitfall 5: Tailwind v4 `@theme` Syntax is Different from v3 Config

**What goes wrong:** Writing `theme: { colors: { accent: '#f97316' } }` (v3 JS config syntax) inside the style block — this is ignored silently.

**Why it happens:** v3 used `tailwind.config.js`; v4 uses CSS-native `@theme` directive.

**How to avoid:** Use `@theme { --color-accent: #f97316; }` inside `<style type="text/tailwindcss">`. The `--color-` prefix is mandatory — Tailwind generates `text-accent`, `bg-accent`, `border-accent` etc. automatically from this.

### Pitfall 6: MusicApplication Category May Not Trigger Google Rich Result

**What goes wrong:** Expecting a Google rich result snippet for the SoftwareApplication schema — Google's documented applicationCategory list does not include MusicApplication.

**Why it happens:** Google's rich results documentation lists ~10 example categories; MusicApplication is not among them. Schema.org itself allows any free-text value.

**How to avoid:** Use `"applicationCategory": "MusicApplication"` anyway — it is valid Schema.org and provides machine-readable context for other crawlers and AI indexers. Do not expect a starred rating rich result unless `aggregateRating` is also added in a later phase.

---

## Code Examples

Verified patterns from official sources:

### Tailwind v4 CDN + @theme Custom Colors

```html
<!-- Source: https://tailwindcss.com/docs/installation/play-cdn -->
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
<style type="text/tailwindcss">
  @theme {
    --color-accent: #f97316;
    --color-surface: #0f1623;
  }
</style>
<!-- Usage: class="bg-surface text-accent" -->
```

### Canonical Tag (Absolute URL)

```html
<!-- Source: https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls -->
<link rel="canonical" href="https://kaini-instruments.com/" />
```

### Open Graph Required Four Tags

```html
<!-- Source: https://ogp.me/ -->
<meta property="og:type" content="website" />
<meta property="og:title" content="Melody Generator App | TC-01 Tone Calculator" />
<meta property="og:image" content="https://kaini-instruments.com/assets/images/og-image.png" />
<meta property="og:url" content="https://kaini-instruments.com/" />
```

### SoftwareApplication JSON-LD

```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "TC-01 Tone Calculator",
  "applicationCategory": "MusicApplication",
  "operatingSystem": "Android, Windows, Web",
  "offers": {
    "@type": "Offer",
    "price": "0",
    "priceCurrency": "USD"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Kaini Instruments"
  }
}
```

### Favicon Minimal Modern Set

```html
<!-- Source: https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs -->
<link rel="icon" href="/assets/images/favicon.png" type="image/png" />
<link rel="apple-touch-icon" href="/assets/images/apple-touch-icon.png" />
<meta name="theme-color" content="#0f1623" />
```

### .nojekyll file

```
(empty file — zero bytes, filename only)
```

### CNAME file

```
kaini-instruments.com
```

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| Tailwind v3 JS config file | v4 CSS @theme directive | v4.0 (Jan 2025) | Config is now CSS-native; no tailwind.config.js needed |
| Many favicon sizes (16, 32, 48, 57, 72, 96, 114, 128, 144, 152, 180...) | 3-file set: favicon.ico + apple-touch-icon.png + icon.svg | 2021-present | SVG covers modern browsers; ico for legacy fallback |
| OG tags with Twitter-specific `twitter:card` | OG tags + optional Twitter Card tags | Twitter Card still exists but OG is primary | Phase 1 uses OG only; Twitter Cards are v2 (SEO-06) |
| schema.org SoftwareApplication with no `offers` | `offers.price` is Google-required for rich results | Google policy | Must include `offers` block or rich result will not be eligible |

**Deprecated/outdated:**

- `tailwind.config.js` for CDN usage: v4 CDN does not use JS config files. @theme directive in CSS is the v4 replacement.
- Multiple PNG favicons at different sizes: SVG favicon + apple-touch-icon.png covers all modern cases. Large favicon size arrays are unnecessary overhead.
- `<meta name="keywords">`: Ignored by all major search engines since ~2009. Do not include.

---

## Open Questions

1. **Exact orange hex value**
   - What we know: User wants "vibrant/electric orange" — TC-01's brand color
   - What's unclear: No hex provided; Claude to decide
   - Recommendation: `#f97316` (Tailwind's `orange-500`) is a well-established vibrant orange that works on dark navy. Alternative: `#fb923c` (orange-400) if a lighter tone is preferred. Either is safe to use; confirm with user in Phase 2 visual review.

2. **Nav sticky vs static**
   - What we know: User left this to Claude's discretion
   - What's unclear: Whether sticky nav affects Phase 2 hero section layout assumptions
   - Recommendation: Sticky nav (`position: sticky; top: 0`) is the modern SaaS default (Linear, Vercel, etc.). Implement sticky with a subtle backdrop blur (`backdrop-blur-sm bg-surface/90`) for a premium feel. Revisit if hero layout conflicts in Phase 2.

3. **Font loading — Google Fonts vs system font**
   - What we know: User mentioned Inter or DM Sans; Claude's discretion
   - What's unclear: Whether Google Fonts CDN dependency is acceptable given the no-build constraint
   - Recommendation: Use Inter via Google Fonts CDN (`<link rel="preconnect" href="https://fonts.googleapis.com">` + font stylesheet). This is standard for static sites. If user prefers no external font dep, Tailwind's `font-sans` stack (system-ui, sans-serif) is the fallback.

---

## Sources

### Primary (HIGH confidence)

- [https://tailwindcss.com/docs/installation/play-cdn](https://tailwindcss.com/docs/installation/play-cdn) — CDN script tag, @theme directive syntax, v4 Play CDN behavior
- [https://developers.google.com/search/docs/appearance/structured-data/software-app](https://developers.google.com/search/docs/appearance/structured-data/software-app) — SoftwareApplication required properties, applicationCategory examples, offers requirement
- [https://ogp.me/](https://ogp.me/) — Open Graph protocol specification, four required tags
- [https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls) — canonical tag requirements, absolute URL requirement
- [https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) — CNAME file format, GitHub Pages custom domain process
- [https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs](https://evilmartians.com/chronicles/how-to-favicon-in-2021-six-files-that-fit-most-needs) — Minimal modern favicon set (3-file approach)

### Secondary (MEDIUM confidence)

- [https://github.com/schemaorg/schemaorg/issues/635](https://github.com/schemaorg/schemaorg/issues/635) — schema.org applicationCategory is free-form text, no fixed enumeration; MusicApplication is valid
- [https://webtech.tools/can-you-use-tailwind-cdn-in-production-sites](https://webtech.tools/can-you-use-tailwind-cdn-in-production-sites) — Practical CDN production tradeoffs (~300KB bundle size)
- [https://a11ymatters.com/pattern/mobile-nav/](https://a11ymatters.com/pattern/mobile-nav/) — Accessible mobile navigation ARIA patterns
- MDN Web Docs — aria-expanded, aria-controls toggle patterns for hamburger menus

### Tertiary (LOW confidence)

- None — all key claims verified with primary or secondary sources

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — Tailwind v4 CDN tag fetched from official docs; vanilla JS is project constraint with no ambiguity
- Architecture: HIGH — HTML5 semantic structure is stable; patterns drawn from official docs
- SEO metadata: HIGH — OG spec from ogp.me; canonical from Google Search Central; JSON-LD from Google developer docs
- GitHub Pages: HIGH — CNAME and .nojekyll confirmed via GitHub official docs
- Pitfalls: MEDIUM-HIGH — Most verified; MusicApplication category limitation verified via Google docs + schema.org issue tracker

**Research date:** 2026-02-26
**Valid until:** 2026-05-26 (stable domain — 90 days; Tailwind v4 is stable release)

# Phase 1: Foundation & SEO - Context

**Gathered:** 2026-02-26
**Status:** Ready for planning

<domain>
## Phase Boundary

Deliver a valid, crawlable dark-themed HTML skeleton with complete SEO metadata, navigation with anchor links to empty section stubs, Tailwind CSS v4 via CDN, and GitHub Pages deployment files (CNAME, .nojekyll). No marketing content, no interactivity beyond nav — those are Phase 2 and Phase 3.

</domain>

<decisions>
## Implementation Decisions

### Dark palette & visual tone
- Deep navy/charcoal background — professional, polished (Linear/Vercel direction)
- Vibrant/electric orange accent color — TC-01 brand color
- Subtle gradients on hero/section backgrounds for modern SaaS feel
- Clean sans-serif font (Inter, DM Sans, or similar)
- Both TC-01 and Kaini logos will be used — TC-01 prominent, Kaini in footer
- User will provide logo files (TC-01 logo + Kaini company logo)

### Page skeleton & navigation
- Section order: Hero > Features > Gallery > Web App > Contact
- Nav displays "TC-01" text with "Tone Calculator" context so visitors understand the brand
- Real section IDs from day one (#features, #gallery, #web-app, #contact) so anchor links work
- Section stubs show heading title + short placeholder text
- Subtle background alternation between sections (slightly different dark shades) for visual structure

### Mobile navigation
- Hamburger menu on screens below 768px breakpoint
- Vanilla JS toggle for hamburger open/close (no framework, no CSS-only hack)
- Desktop: horizontal nav links visible

### SEO copy & keywords
- Primary keyword: "Melody Generator App"
- Secondary keywords: "melody creator", "song maker app"
- Title tag: "Melody Generator App | TC-01 Tone Calculator"
- Meta description: ease-focused messaging ("generate melodies in seconds, no music theory needed" direction)
- Copy tone: professional & polished
- Target audience: beginners (no music theory) AND hobbyist musicians equally
- Custom domain: kaini-instruments.com

### JSON-LD & Open Graph
- SoftwareApplication schema with category: MusicApplication
- Operating systems: Android, Windows, Web
- Pricing: freemium model — free download with optional Pro license key purchase
- OG image: placeholder path (og-image.png) to be replaced with real asset later

### Footer
- Copyright: "© 2026 Kaini Instruments. All rights reserved."
- Kaini logo in footer
- Placeholder links for Privacy Policy and Terms of Service (href="#")
- No social media links (no accounts yet)

### Favicon & browser tab
- PNG favicon (user will provide the file)
- Apple touch icon tag included (referencing apple-touch-icon.png)
- Browser theme-color: dark navy (matches page background for immersive mobile feel)
- Favicon tags set up to reference provided PNG at multiple sizes

### Claude's Discretion
- Exact accent color shade (vibrant/electric orange on navy — Claude picks the hex)
- Text color hierarchy (white headings, gray body — Claude picks exact values)
- Nav bar behavior (sticky vs static)
- Hamburger menu open animation style
- Section stub min-height (whether to enforce minimum or collapse to content)
- Footer layout and content arrangement
- Nav label text for section links
- OG image placeholder approach

</decisions>

<specifics>
## Specific Ideas

- Orange is the dominant TC-01 brand color — must be the primary accent
- "Tone Calculator" context must appear in nav so visitors understand what "TC" stands for
- Domain is kaini-instruments.com — used for canonical URL, CNAME, OG url
- Company is "Kaini Instruments" — used in copyright and JSON-LD publisher
- Page title exactly: "Melody Generator App | TC-01 Tone Calculator"
- User will share logo files (TC-01 logo, Kaini logo, favicon PNG) — set up paths/references for them

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 01-foundation-seo*
*Context gathered: 2026-02-26*

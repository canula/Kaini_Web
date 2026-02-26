# Requirements: TC-01 Melody Generator Landing Page

**Defined:** 2026-02-26
**Core Value:** Convert visitors into users and upsell Free users to Pro via Lemon Squeezy license key flow

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### SEO & Metadata

- [ ] **SEO-01**: Page has optimized `<title>` and `<meta description>` for "AI Melody Generator" and "Music Maker App"
- [ ] **SEO-02**: Semantic `<h1>` and `<h2>` tags target primary keywords
- [ ] **SEO-03**: Open Graph tags (og:title, og:description, og:image, og:url) for social sharing
- [ ] **SEO-04**: Canonical URL tag pointing to custom domain
- [ ] **SEO-05**: JSON-LD SoftwareApplication schema for Google rich results

### Hero Section

- [ ] **HERO-01**: Clean headline and subheadline explaining the value proposition
- [ ] **HERO-02**: "Launch Web App" CTA button that smooth-scrolls to web app section
- [ ] **HERO-03**: Official Google Play badge with placeholder link
- [ ] **HERO-04**: Official Microsoft Store badge with placeholder link

### Features Showcase

- [ ] **FEAT-01**: Section highlighting 6-8 key capabilities with benefits-first language
- [ ] **FEAT-02**: Visual icons or illustrations for each feature

### Gallery

- [ ] **GLRY-01**: Phone screenshots displayed inside CSS phone frame mockups
- [ ] **GLRY-02**: Desktop UI screenshot display
- [ ] **GLRY-03**: Embedded phone video (autoplay muted loop playsinline)
- [ ] **GLRY-04**: Horizontal scroll/carousel on mobile for phone mockups

### Web App UI Placeholder

- [ ] **APP-01**: Static layout with dropdowns for genre selection and a "Generate" button
- [ ] **APP-02**: Audio player placeholder element
- [ ] **APP-03**: Pro badge overlay on advanced features (Phrase Mode, MIDI Import, Custom Instruments)
- [ ] **APP-04**: `data-pro-lock` attributes on locked elements for JS targeting

### Pro Activation

- [ ] **PRO-01**: "Activate License Key" input field with Submit button
- [ ] **PRO-02**: Async placeholder function for Lemon Squeezy API license validation
- [ ] **PRO-03**: On success, set `localStorage.setItem('isPro', 'true')` and unlock Pro UI
- [ ] **PRO-04**: Loading, success, and error states on activation
- [ ] **PRO-05**: "Buy a Pro License" text link to Lemon Squeezy checkout URL

### Contact

- [ ] **CONT-01**: Contact section with centered `#tally-form-container` div and HTML comment placeholder

### Infrastructure

- [ ] **INFR-01**: Single `index.html` using Tailwind CSS v4 via CDN (no build process)
- [ ] **INFR-02**: Vanilla JavaScript only (no frameworks)
- [ ] **INFR-03**: `CNAME` file committed to repo root for custom domain
- [ ] **INFR-04**: `.nojekyll` file committed to repo root
- [ ] **INFR-05**: Dark color palette with clean modern marketing design

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### SEO Enhancements

- **SEO-06**: Twitter Card meta tags for Twitter sharing
- **SEO-07**: Structured FAQ schema for Google rich snippets

### Content

- **CONT-02**: Blog or changelog section
- **CONT-03**: Testimonials/reviews section (once reviews exist)

### Monetization

- **PRO-06**: Cloudflare Worker proxy for Lemon Squeezy API (if CORS blocks direct calls)
- **PRO-07**: TC-02, TC-03 product tiers with separate license keys

## Out of Scope

| Feature | Reason |
|---------|--------|
| Functional audio demo | Requires full app stack (Tone.js, React) — landing page is static only |
| In-app checkout cart | Breaks static constraint; Lemon Squeezy external checkout is the model |
| HTML contact form | Tally.so iframe will be pasted in later |
| Light mode theme | App is dark-only; landing page matches |
| Build tools (npm, Vite, Node.js) | Strictly static GitHub Pages constraint |
| Backend/database | localStorage only for license state |
| Cookie/GDPR banner | No tracking = no obligation |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| SEO-01 | — | Pending |
| SEO-02 | — | Pending |
| SEO-03 | — | Pending |
| SEO-04 | — | Pending |
| SEO-05 | — | Pending |
| HERO-01 | — | Pending |
| HERO-02 | — | Pending |
| HERO-03 | — | Pending |
| HERO-04 | — | Pending |
| FEAT-01 | — | Pending |
| FEAT-02 | — | Pending |
| GLRY-01 | — | Pending |
| GLRY-02 | — | Pending |
| GLRY-03 | — | Pending |
| GLRY-04 | — | Pending |
| APP-01 | — | Pending |
| APP-02 | — | Pending |
| APP-03 | — | Pending |
| APP-04 | — | Pending |
| PRO-01 | — | Pending |
| PRO-02 | — | Pending |
| PRO-03 | — | Pending |
| PRO-04 | — | Pending |
| PRO-05 | — | Pending |
| CONT-01 | — | Pending |
| INFR-01 | — | Pending |
| INFR-02 | — | Pending |
| INFR-03 | — | Pending |
| INFR-04 | — | Pending |
| INFR-05 | — | Pending |

**Coverage:**
- v1 requirements: 30 total
- Mapped to phases: 0
- Unmapped: 30 ⚠️

---
*Requirements defined: 2026-02-26*
*Last updated: 2026-02-26 after initial definition*

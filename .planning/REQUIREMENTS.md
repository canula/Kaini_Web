# Requirements: TC-01 Melody Generator Landing Page

**Defined:** 2026-02-26
**Core Value:** Convert visitors into users and upsell Free users to Pro via Lemon Squeezy license key flow

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### SEO & Metadata

- [x] **SEO-01**: Page has optimized `<title>` and `<meta description>` for "AI Melody Generator" and "Music Maker App"
- [x] **SEO-02**: Semantic `<h1>` and `<h2>` tags target primary keywords
- [x] **SEO-03**: Open Graph tags (og:title, og:description, og:image, og:url) for social sharing
- [x] **SEO-04**: Canonical URL tag pointing to custom domain
- [x] **SEO-05**: JSON-LD SoftwareApplication schema for Google rich results

### Hero Section

- [x] **HERO-01**: Clean headline and subheadline explaining the value proposition
- [x] **HERO-02**: "Launch Web App" CTA button that smooth-scrolls to web app section
- [x] **HERO-03**: Official Google Play badge with placeholder link
- [x] **HERO-04**: Official Microsoft Store badge with placeholder link

### Features Showcase

- [x] **FEAT-01**: Section highlighting 6-8 key capabilities with benefits-first language
- [x] **FEAT-02**: Visual icons or illustrations for each feature

### Gallery

- [x] **GLRY-01**: Phone screenshots displayed inside CSS phone frame mockups
- [x] **GLRY-02**: Desktop UI screenshot display
- [x] **GLRY-03**: Embedded phone video (autoplay muted loop playsinline)
- [x] **GLRY-04**: ~~Horizontal scroll/carousel on mobile for phone mockups~~ Rescoped: Superseded by Phase 3 paired feature/image rows (user-approved)

### Web App UI Placeholder

- [ ] **APP-01**: Static layout with dropdowns for genre selection and a "Generate" button
- [ ] **APP-02**: Audio player placeholder element
- [x] **APP-03**: Pro badge overlay on advanced features (Phrase Mode, MIDI Import, Custom Instruments)
- [ ] **APP-04**: `data-pro-lock` attributes on locked elements for JS targeting

### Pro Activation

- [ ] **PRO-01**: "Activate License Key" input field with Submit button
- [ ] **PRO-02**: Async placeholder function for Lemon Squeezy API license validation
- [ ] **PRO-03**: On success, set `localStorage.setItem('isPro', 'true')` and unlock Pro UI
- [ ] **PRO-04**: Loading, success, and error states on activation
- [x] **PRO-05**: "Buy a Pro License" text link to Lemon Squeezy checkout URL

### Contact

- [x] **CONT-01**: Contact section with centered `#tally-form-container` div and HTML comment placeholder

### Infrastructure

- [x] **INFR-01**: Single `index.html` using Tailwind CSS v4 via CDN (no build process)
- [x] **INFR-02**: Vanilla JavaScript only (no frameworks)
- [x] **INFR-03**: `CNAME` file committed to repo root for custom domain
- [x] **INFR-04**: `.nojekyll` file committed to repo root
- [x] **INFR-05**: Dark color palette with clean modern marketing design

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
| Functional audio demo | Requires full app stack (Tone.js, React) -- landing page is static only |
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
| SEO-01 | Phase 1 | Complete |
| SEO-02 | Phase 1 | Complete |
| SEO-03 | Phase 1 | Complete |
| SEO-04 | Phase 1 | Complete |
| SEO-05 | Phase 1 | Complete |
| HERO-01 | Phase 2 | Complete |
| HERO-02 | Phase 2 | Complete |
| HERO-03 | Phase 2 | Complete |
| HERO-04 | Phase 2 | Complete |
| FEAT-01 | Phase 2 | Complete |
| FEAT-02 | Phase 2 | Complete |
| GLRY-01 | Phase 2 | Complete |
| GLRY-02 | Phase 2 | Complete |
| GLRY-03 | Phase 2 | Complete |
| GLRY-04 | -- | Rescoped: Superseded by Phase 3 paired feature/image rows (user-approved) |
| APP-01 | -- | Rescoped: Phase 3 CONTEXT.md removed static app UI placeholder from scope |
| APP-02 | -- | Rescoped: Phase 3 CONTEXT.md removed static app UI placeholder from scope |
| APP-03 | Phase 3 | Complete |
| APP-04 | -- | Rescoped: Phase 3 CONTEXT.md removed data-pro-lock attributes from scope |
| PRO-01 | -- | Rescoped: Phase 3 CONTEXT.md — license activation handled in-app, not on landing page |
| PRO-02 | -- | Rescoped: Phase 3 CONTEXT.md — license activation handled in-app, not on landing page |
| PRO-03 | -- | Rescoped: Phase 3 CONTEXT.md — license activation handled in-app, not on landing page |
| PRO-04 | -- | Rescoped: Phase 3 CONTEXT.md — license activation handled in-app, not on landing page |
| PRO-05 | Phase 3 | Complete |
| CONT-01 | Phase 2 | Complete |
| INFR-01 | Phase 1 | Complete |
| INFR-02 | Phase 1 | Complete |
| INFR-03 | Phase 1 | Complete |
| INFR-04 | Phase 1 | Complete |
| INFR-05 | Phase 1 | Complete |

**Coverage:**
- v1 requirements: 30 total
- Mapped to phases: 22
- Rescoped: 8 (APP-01, APP-02, APP-04, PRO-01, PRO-02, PRO-03, PRO-04, GLRY-04)

---
*Requirements defined: 2026-02-26*
*Last updated: 2026-02-26 after Phase 3 rescope revision*

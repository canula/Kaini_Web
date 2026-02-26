# Roadmap: TC-01 Melody Generator Landing Page

## Overview

This roadmap delivers a static single-page landing site for TC-01 Melody Generator in three phases. Phase 1 lays the HTML skeleton, infrastructure files, SEO metadata, and Tailwind CDN setup. Phase 2 builds all visual marketing sections (hero, features, gallery, contact). Phase 3 adds the interactive web app demo placeholder and Pro license activation flow -- the core conversion mechanic. Every phase produces a page that can be opened in a browser and visually verified.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [x] **Phase 1: Foundation & SEO** - HTML skeleton, infrastructure files, SEO metadata, Tailwind CDN, dark palette baseline (completed 2026-02-26)
- [x] **Phase 2: Marketing Sections** - Hero, features showcase, gallery with phone mockups, video embed, contact placeholder (completed 2026-02-26)
- [x] **Phase 3: Web App Demo & Pro Activation** - Charcoal palette, paired feature/image layout, Free vs Pro comparison section, Buy Pro CTA (completed 2026-02-26)

## Phase Details

### Phase 1: Foundation & SEO
**Goal**: Visitors see a valid, crawlable dark-themed page skeleton with correct metadata, and GitHub Pages deployment files are in place from day one
**Depends on**: Nothing (first phase)
**Requirements**: INFR-01, INFR-02, INFR-03, INFR-04, INFR-05, SEO-01, SEO-02, SEO-03, SEO-04, SEO-05
**Success Criteria** (what must be TRUE):
  1. Opening index.html in a browser shows a dark-themed page with empty section stubs and a nav bar with anchor links
  2. Viewing page source shows complete SEO meta tags (title, description, OG tags, canonical, JSON-LD SoftwareApplication schema)
  3. CNAME file and .nojekyll file exist in the repo root
  4. Page uses Tailwind CSS v4 via CDN with no build process and no framework JS
**Plans**: 2 plans

Plans:
- [ ] 01-01-PLAN.md — Infrastructure files (CNAME, .nojekyll) + index.html with complete head metadata, SEO tags, and Tailwind v4 CDN
- [ ] 01-02-PLAN.md — Page body: sticky nav, section stubs, footer, hamburger JS, visual checkpoint

### Phase 2: Marketing Sections
**Goal**: A visitor landing on the page understands what TC-01 is, sees it in action through screenshots and video, and can navigate to app stores or scroll to the web app
**Depends on**: Phase 1
**Requirements**: HERO-01, HERO-02, HERO-03, HERO-04, FEAT-01, FEAT-02, GLRY-01, GLRY-02, GLRY-03, GLRY-04, CONT-01
**Success Criteria** (what must be TRUE):
  1. Hero section displays headline, subheadline, and three CTAs (Launch Web App smooth-scroll, Google Play badge, Microsoft Store badge)
  2. Features section presents 6-8 capabilities with icons and benefits-first copy
  3. Gallery shows phone screenshots inside CSS phone frame mockups with horizontal snap-scroll on mobile, a desktop screenshot, and an embedded autoplay-muted video
  4. Contact section contains a #tally-form-container div with an HTML comment placeholder for Tally.so
  5. Page is responsive across mobile (375px), tablet (768px), and desktop (1280px) viewports
**Plans**: 2 plans

Plans:
- [x] 02-01-PLAN.md — Hero section (split layout, gradient, 3 CTAs, store badges), features grid (6 capabilities with SVG icons), contact placeholder
- [ ] 02-02-PLAN.md — Gallery (phone screenshot snap carousel, desktop screenshot, autoplay video), visual checkpoint

### Phase 3: Web App Demo & Pro Activation
**Goal**: Visitors see features paired with visual proof, understand Free vs Pro differences, and can purchase a Pro license via Lemon Squeezy -- all on a charcoal grey themed page with updated navigation
**Depends on**: Phase 2
**Requirements**: APP-03, PRO-05 (APP-01, APP-02, APP-04, PRO-01, PRO-02, PRO-03, PRO-04 rescoped out -- see REQUIREMENTS.md traceability)
**Success Criteria** (what must be TRUE):
  1. Page uses charcoal grey palette (#1a1a1a / #2a2a2a) instead of dark navy
  2. Six features displayed in alternating two-column rows with paired screenshots/video
  3. Free vs Pro comparison section shows two columns with feature lists and Buy Pro CTA
  4. "Buy a Pro License" link opens Lemon Squeezy checkout in new tab
  5. Nav shows Features | Pricing | Contact (no Gallery or Web App links)
  6. #web-app stub section is removed; Hero CTA points to placeholder external URL
**Plans**: 2 plans

Plans:
- [ ] 03-01-PLAN.md — Charcoal palette swap, nav updates, #web-app removal, paired feature/image alternating layout
- [ ] 03-02-PLAN.md — Free vs Pro comparison section with Buy CTA, visual checkpoint

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation & SEO | 2/2 | Complete   | 2026-02-26 |
| 2. Marketing Sections | 2/2 | Complete   | 2026-02-26 |
| 3. Web App Demo & Pro Activation | 2/2 | Complete   | 2026-02-26 |

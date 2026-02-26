# Roadmap: TC-01 Melody Generator Landing Page

## Overview

This roadmap delivers a static single-page landing site for TC-01 Melody Generator in three phases. Phase 1 lays the HTML skeleton, infrastructure files, SEO metadata, and Tailwind CDN setup. Phase 2 builds all visual marketing sections (hero, features, gallery, contact). Phase 3 adds the interactive web app demo placeholder and Pro license activation flow -- the core conversion mechanic. Every phase produces a page that can be opened in a browser and visually verified.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 1: Foundation & SEO** - HTML skeleton, infrastructure files, SEO metadata, Tailwind CDN, dark palette baseline
- [ ] **Phase 2: Marketing Sections** - Hero, features showcase, gallery with phone mockups, video embed, contact placeholder
- [ ] **Phase 3: Web App Demo & Pro Activation** - Static app UI placeholder with Pro locks, license key activation flow, buy link

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
**Plans**: TBD

Plans:
- [ ] 01-01: TBD

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
**Plans**: TBD

Plans:
- [ ] 02-01: TBD

### Phase 3: Web App Demo & Pro Activation
**Goal**: Visitors can preview the app UI inline, see Pro-locked features clearly distinguished from free ones, activate a license key, and find the purchase link
**Depends on**: Phase 2
**Requirements**: APP-01, APP-02, APP-03, APP-04, PRO-01, PRO-02, PRO-03, PRO-04, PRO-05
**Success Criteria** (what must be TRUE):
  1. Static web app UI section shows dropdowns, a Generate button, and an audio player placeholder -- all non-functional but visually representative
  2. Pro-locked elements (Phrase Mode, MIDI Import, Custom Instruments) display a Pro badge overlay and have data-pro-lock attributes
  3. Entering a license key shows loading, success, or error states, and on success sets localStorage isPro flag and removes Pro lock badges
  4. A "Buy a Pro License" link points to the Lemon Squeezy checkout URL
  5. Refreshing the page after successful activation retains the unlocked Pro state via localStorage
**Plans**: TBD

Plans:
- [ ] 03-01: TBD

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Foundation & SEO | 0/0 | Not started | - |
| 2. Marketing Sections | 0/0 | Not started | - |
| 3. Web App Demo & Pro Activation | 0/0 | Not started | - |

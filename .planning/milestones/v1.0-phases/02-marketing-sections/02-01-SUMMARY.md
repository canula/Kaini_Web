---
phase: 02-marketing-sections
plan: 01
subsystem: ui
tags: [html, tailwind, tailwind-v4, hero, features, contact, svg, responsive]

# Dependency graph
requires:
  - phase: 01-foundation-seo
    provides: index.html page skeleton with nav, footer, custom Tailwind v4 color tokens, scroll-smooth

provides:
  - Hero section with split layout, gradient overlay, headline, subheadline, and 3 CTAs (Launch Web App, Google Play badge, MS Store badge)
  - Features section with 6 capability cards in responsive 2-column grid with inline Heroicons SVG icons
  - Contact section with #tally-form-container placeholder for Tally.so embed

affects: [02-02-gallery, 03-web-app-activation]

# Tech tracking
tech-stack:
  added: []
  patterns:
    - "Tailwind v4 gradient syntax: bg-linear-to-br (not bg-gradient-to-br)"
    - "Heroicons v2 inline SVG pattern: fill=none viewBox=0 0 24 24 stroke-width=1.5 stroke=currentColor aria-hidden=true"
    - "Split hero grid: md:grid-cols-2 gap-12 items-center with relative z-index stacking over absolute gradient overlay"
    - "Feature card: bg-surface rounded-xl p-6 flex gap-4 with w-8 h-8 text-accent flex-shrink-0 icon"

key-files:
  created: []
  modified:
    - index.html

key-decisions:
  - "Used bg-linear-to-br (Tailwind v4) not bg-gradient-to-br for gradient overlay — old class silently fails in v4"
  - "Phone mockup uses relative path assets/images/screenshots/phone-01.png (no leading slash) for GitHub Pages compatibility"
  - "Google Play and MS Store badge links contain TODO comments with PLACEHOLDER IDs for future replacement"
  - "Features section uses bg-surface cards on bg-surface-alt section background for contrast"
  - "Contact section uses bg-surface alternating with web-app section bg-surface-alt above it"

patterns-established:
  - "Section alternation: bg-surface and bg-surface-alt alternate through page for visual rhythm"
  - "CTA row: flex flex-wrap items-center gap-4 allows badge images to wrap naturally on narrow viewports"

requirements-completed: [HERO-01, HERO-02, HERO-03, HERO-04, FEAT-01, FEAT-02, CONT-01]

# Metrics
duration: 2min
completed: 2026-02-26
---

# Phase 2 Plan 01: Marketing Sections (Hero, Features, Contact) Summary

**Split-layout hero with gradient overlay and 3 CTAs, 6-feature card grid with Heroicons SVG icons, and Tally.so contact placeholder — replacing all stub sections in index.html**

## Performance

- **Duration:** 2 min
- **Started:** 2026-02-26T17:19:20Z
- **Completed:** 2026-02-26T17:20:46Z
- **Tasks:** 2
- **Files modified:** 1

## Accomplishments

- Hero section: split 2-column grid (text+CTAs left, phone screenshot right), Tailwind v4 gradient overlay (`bg-linear-to-br from-surface via-surface to-accent/10`), headline, subheadline, and 3 inline CTAs (Launch Web App anchor, Google Play badge, MS Store badge)
- Features section: 6 capability cards in responsive `md:grid-cols-2` grid, each with Heroicons v2 inline SVG (`text-accent w-8 h-8`), h3 title, and 2-3 sentence benefits-first copy
- Contact section: centered "Get in touch" heading, body text, and `#tally-form-container` div with HTML comment placeholder for Tally.so embed

## Task Commits

Each task was committed atomically:

1. **Task 1: Build hero section with split layout, gradient, and CTAs** - `154d7af` (feat)
2. **Task 2: Build features grid with SVG icons and contact placeholder** - `e3c6e98` (feat)

**Plan metadata:** `(pending final commit)` (docs: complete plan)

## Files Created/Modified

- `index.html` - Hero, features, and contact sections built; stubs replaced with full implementations

## Decisions Made

- Used `bg-linear-to-br` (Tailwind v4 syntax) — `bg-gradient-to-br` was renamed in v4 and silently fails
- Phone mockup uses relative path `assets/images/screenshots/phone-01.png` (no leading slash) per GitHub Pages compatibility requirement from plan
- Both store badge links contain TODO comments with placeholder IDs, per plan's explicit instruction
- Feature cards use `bg-surface` background on the `bg-surface-alt` section to create card contrast
- Contact section `bg-surface` properly alternates with the `bg-surface-alt` web-app section directly above

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Hero, Features, and Contact sections complete and ready for browser verification
- Phone screenshot asset (`assets/images/screenshots/phone-01.png`) needs to exist for hero phone mockup to render
- App store placeholder IDs in badge links need replacement before launch (Google Play package name, MS Store product ID)
- Tally.so embed code needed for #tally-form-container before Contact section is functional
- Phase 02-02 (Gallery section) can proceed independently

---
*Phase: 02-marketing-sections*
*Completed: 2026-02-26*

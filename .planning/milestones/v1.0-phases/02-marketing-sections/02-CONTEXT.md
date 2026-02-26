# Phase 2: Marketing Sections - Context

**Gathered:** 2026-02-26
**Status:** Ready for planning

<domain>
## Phase Boundary

Build all visual marketing sections into the existing page skeleton: hero with CTAs and store badges, features showcase with 6 capabilities, gallery with phone screenshots and video, and contact placeholder. A visitor landing on the page should understand what TC-01 is, see it in action, and know how to get it. The web app demo and Pro activation flow are Phase 3.

</domain>

<decisions>
## Implementation Decisions

### Hero layout & CTAs
- Gradient overlay background (dark surface to transparent with subtle accent color)
- Split layout: text/CTAs on one side, phone mockup screenshot on the other
- All three CTAs inline in a single row: "Launch Web App" button + Google Play badge + Microsoft Store badge
- On mobile, stacks vertically — stacking order (text-first vs mockup-first) is Claude's discretion

### Feature card presentation
- 2-column grid layout (3 rows of 2 on desktop, stacks to 1 column on mobile)
- SVG line icons (Heroicons or Lucide style) for each feature
- 6 features total, specifically:
  1. Record Voice
  2. MIDI Import
  3. Motion/Contour/Rhythm Presets
  4. Phrase Generation
  5. Microtonal Support
  6. Note Edit Tracker
- Each card has 2-3 sentences of benefits-first copy
- Card panel style (background vs borderless) is Claude's discretion

### Gallery & media
- 8 phone screenshots available as real assets (not placeholders)
- Screenshots displayed flat with rounded corners and subtle shadow — no CSS device frames
- Horizontal snap-scroll carousel on mobile for phone screenshots
- Desktop screenshot included — placement relative to carousel/video is Claude's discretion
- Video is a local MP4/WebM file (self-hosted, HTML `<video>` tag)
- Video is a phone screen recording (portrait aspect ratio) — displayed below the carousel
- Video plays autoplay muted loop playsinline per requirements

### Contact section
- Centered `#tally-form-container` div with HTML comment placeholder for Tally.so embed
- Minimal section — just the heading and placeholder container

### Claude's Discretion
- Mobile hero stacking order (text-first vs mockup-first)
- Feature card panel style (surface-alt cards vs borderless items)
- Desktop screenshot placement within gallery flow
- Section subheadings (1-line intros under h2s) — per section as needed
- Exact spacing, typography scale, and responsive breakpoint details
- Gallery carousel navigation indicators (dots, arrows, or scroll-only)

</decisions>

<specifics>
## Specific Ideas

- Casual & approachable copy tone — friendly, conversational (e.g., "Create melodies in seconds. No music degree required.")
- Features copy should be benefits-first, 2-3 sentences explaining what each feature does for the user
- The 6 features are specific app capabilities the user wants highlighted — copy should explain each in plain language
- Phone screenshots and video are real assets ready to drop in (not placeholders)
- Video is a screen recording of the phone app in action — shows the actual workflow

</specifics>

<deferred>
## Deferred Ideas

None — discussion stayed within phase scope

</deferred>

---

*Phase: 02-marketing-sections*
*Context gathered: 2026-02-26*

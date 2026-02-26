---
phase: 02-marketing-sections
verified: 2026-02-26T18:00:00Z
status: human_needed
score: 11/11 must-haves verified
human_verification:
  - test: "Open index.html at 1280px desktop width and confirm hero split layout renders correctly: headline and 3 CTAs on the left, phone screenshot on the right, with visible gradient tint toward bottom-right"
    expected: "Two-column layout visible; gradient overlay present; all 3 CTAs in a horizontal row"
    why_human: "CSS grid rendering and gradient visibility cannot be confirmed by grep — requires a browser"
  - test: "Resize to 375px mobile width: hero stacks vertically (text first, phone mockup below), features stack to 1 column, gallery carousel is swipeable, video plays inline (not fullscreen)"
    expected: "All sections respond correctly at mobile breakpoint; no horizontal overflow on the page body"
    why_human: "Responsive breakpoint behavior requires browser rendering"
  - test: "In the gallery section, confirm all 7 phone screenshots load and display with rounded corners and shadow; confirm the desktop screenshot spans full available width"
    expected: "All images render (no broken-image icons); rounded-2xl and shadow-lg visually present"
    why_human: "Image rendering requires browser; asset paths use relative URLs and must be confirmed against serving context"
  - test: "Confirm the HTML5 video element plays inline on mobile (does not trigger fullscreen player). Note: demo.webm is missing — only demo.mp4 is present. The browser should fall back to MP4 and play correctly."
    expected: "Video plays muted, looping, inline without user interaction; no fullscreen takeover on mobile"
    why_human: "Autoplay behavior and mobile inline playback require a real browser on a real device or emulator"
  - test: "Click the 'Launch Web App' CTA in the hero — verify it smooth-scrolls to the #web-app section"
    expected: "Page scrolls smoothly to the Web App section; no full-page jump"
    why_human: "scroll-smooth behavior requires browser interaction"
---

# Phase 2: Marketing Sections Verification Report

**Phase Goal:** A visitor landing on the page understands what TC-01 is, sees it in action through screenshots and video, and can navigate to app stores or scroll to the web app.
**Verified:** 2026-02-26T18:00:00Z
**Status:** human_needed
**Re-verification:** No — initial verification

## Goal Achievement

All 5 Success Criteria from ROADMAP.md have been verified against the actual codebase. Automated checks pass on all 11 requirements. Five items are flagged for human visual/browser verification before the phase can be fully signed off.

### Observable Truths (from ROADMAP.md Success Criteria)

| # | Truth | Status | Evidence |
|---|-------|--------|----------|
| 1 | Hero section displays headline, subheadline, and three CTAs (Launch Web App smooth-scroll, Google Play badge, Microsoft Store badge) | VERIFIED | `index.html` line 122 (h1), line 125 (p subheadline), line 130 (`href="#web-app"` CTA), line 133 (Google Play), line 137 (Microsoft Store) |
| 2 | Features section presents 6-8 capabilities with icons and benefits-first copy | VERIFIED | 6 `<div class="bg-surface rounded-xl p-6 flex gap-4">` cards (lines 160-223), 6 inline SVGs with `class="w-8 h-8 text-accent"`, all with h3 titles and descriptive copy |
| 3 | Gallery shows phone screenshots in snap-scroll carousel, a desktop screenshot, and an embedded autoplay-muted video | VERIFIED | `snap-x snap-mandatory` container (line 236), 7 phone image slots (lines 238-258), `desktop-01.png` (line 263), `<video autoplay muted loop playsinline>` (line 268) with WebM+MP4 sources |
| 4 | Contact section contains a #tally-form-container div with an HTML comment placeholder for Tally.so | VERIFIED | `index.html` line 291: `<div id="tally-form-container" class="max-w-lg mx-auto">` with `<!-- Tally.so embed goes here -->` at line 292 |
| 5 | Page is responsive across mobile, tablet, and desktop viewports | NEEDS HUMAN | `md:grid-cols-2` on hero (line 119) and features (line 157) confirmed; `md:w-56` on carousel items (lines 238-256) confirmed; actual rendering requires browser |

**Score:** 4/5 truths fully verified programmatically; 1 needs human browser confirmation

### Required Artifacts

| Artifact | Expected | Status | Details |
|----------|----------|--------|---------|
| `index.html` | Hero, Features, Gallery, Contact sections | VERIFIED | All four section IDs present: `id="hero"` (line 115), `id="features"` (line 152), `id="gallery"` (line 230), `id="contact"` (line 287). File is 345 lines — substantive. |
| `assets/images/screenshots/phone-01.png` through `phone-07.png` | 7 phone screenshots | VERIFIED | All 7 files confirmed on disk: phone-01.png through phone-07.png |
| `assets/images/screenshots/desktop-01.png` | Desktop screenshot | VERIFIED | File confirmed on disk |
| `assets/videos/demo.mp4` | Video file (MP4) | VERIFIED | File confirmed on disk |
| `assets/videos/demo.webm` | Video file (WebM) | MISSING | Only `demo.mp4` present; `demo.webm` referenced in HTML (line 271) but not on disk. Browser will fall back to MP4. Plan documented this as a "User Setup Required" item — not a blocker. |

### Key Link Verification

| From | To | Via | Status | Details |
|------|----|-----|--------|---------|
| Hero "Launch Web App" CTA | `#web-app` section | `href="#web-app"` anchor + `scroll-smooth` on `<html>` | WIRED | Line 130: `<a href="#web-app" ...>Launch Web App</a>`; `#web-app` section exists at line 279; `<html class="scroll-smooth">` at line 2 |
| Google Play badge | Google Play Store | External link with placeholder package name | WIRED (placeholder) | Line 133: `href="https://play.google.com/store/apps/details?id=YOUR_PACKAGE_NAME"` — link present with TODO comment, intentional per plan |
| Microsoft Store badge | Microsoft Store | External link with placeholder product ID | WIRED (placeholder) | Line 137: `href="https://apps.microsoft.com/detail/PLACEHOLDER"` — link present with TODO comment, intentional per plan |
| Gallery carousel container | Phone screenshot images | `overflow-x-auto snap-x snap-mandatory` | WIRED | Line 236-237: container has correct CSS classes; 7 `snap-center shrink-0` child items confirmed (grep: 7 matches) |
| Video element | Local video file | HTML5 `<video>` with `<source>` elements | WIRED (partial) | Lines 268-273: `<video autoplay muted loop playsinline>` with WebM+MP4 sources; MP4 file exists; WebM missing from disk (falls back gracefully) |

### Requirements Coverage

| Requirement | Source Plan | Description | Status | Evidence |
|-------------|-------------|-------------|--------|----------|
| HERO-01 | 02-01-PLAN.md | Clean headline and subheadline explaining value proposition | SATISFIED | `<h1>` at line 122, `<p>` subheadline at line 125-127 |
| HERO-02 | 02-01-PLAN.md | "Launch Web App" CTA smooth-scrolling to web app section | SATISFIED | `href="#web-app"` at line 130; `#web-app` section at line 279; `scroll-smooth` on `<html>` |
| HERO-03 | 02-01-PLAN.md | Official Google Play badge with placeholder link | SATISFIED | Badge image from Google CDN, link to `play.google.com` at line 133-135 |
| HERO-04 | 02-01-PLAN.md | Official Microsoft Store badge with placeholder link | SATISFIED | Badge image from `get.microsoft.com`, link to `apps.microsoft.com` at line 137-140 |
| FEAT-01 | 02-01-PLAN.md | Section highlighting 6-8 key capabilities with benefits-first language | SATISFIED | 6 cards with h3 titles and 2-3 sentence descriptions: Record Voice, MIDI Import, Motion/Contour/Rhythm Presets, Phrase Generation, Microtonal Support, Note Edit Tracker |
| FEAT-02 | 02-01-PLAN.md | Visual icons or illustrations for each feature | SATISFIED | 6 inline SVG icons (`w-8 h-8 text-accent`) — one per card, Heroicons v2 style |
| GLRY-01 | 02-02-PLAN.md | Phone screenshots displayed inside CSS phone frame mockups | SATISFIED (adapted) | Plan was updated: screenshots displayed flat with `rounded-2xl shadow-lg` per explicit user decision. REQUIREMENTS.md says "phone frame mockups" but user approved flat display — see CONT-01 note. |
| GLRY-02 | 02-02-PLAN.md | Desktop UI screenshot display | SATISFIED | `desktop-01.png` referenced at line 263, file exists on disk |
| GLRY-03 | 02-02-PLAN.md | Embedded phone video (autoplay muted loop playsinline) | SATISFIED | `<video autoplay muted loop playsinline>` at line 268; all 4 required attributes present |
| GLRY-04 | 02-02-PLAN.md | Horizontal scroll/carousel on mobile for phone mockups | SATISFIED | `overflow-x-auto snap-x snap-mandatory` container at line 236; `snap-center shrink-0` on all 7 child items |
| CONT-01 | 02-01-PLAN.md | Contact section with centered `#tally-form-container` div and HTML comment placeholder | SATISFIED | `<div id="tally-form-container" class="max-w-lg mx-auto">` at line 291 with `<!-- Tally.so embed goes here -->` |

**Note on GLRY-01:** REQUIREMENTS.md specifies "CSS phone frame mockups" but the user explicitly decided during Phase 2 execution to display screenshots flat with rounded corners and shadow instead. This was an approved deviation documented in 02-02-SUMMARY.md. The visual checkpoint task (02-02 Task 2) was approved by the user with this implementation. No gap.

**Orphaned requirements check:** All Phase 2 requirements (HERO-01 through GLRY-04, CONT-01) are claimed by plans 02-01 and 02-02. No orphaned requirements found.

### Anti-Patterns Found

| File | Line | Pattern | Severity | Impact |
|------|------|---------|----------|--------|
| `index.html` | 134 | `<!-- TODO: Replace YOUR_PACKAGE_NAME with real Google Play package name -->` | Info | Intentional placeholder — plan explicitly instructs TODO comment. No functional impact on Phase 2 goal. |
| `index.html` | 138 | `<!-- TODO: Replace PLACEHOLDER with real MS Store product ID -->` | Info | Intentional placeholder — plan explicitly instructs TODO comment. No functional impact on Phase 2 goal. |
| `index.html` | 282 | `"coming soon"` text in `#web-app` section | Info | Correct — `#web-app` is a Phase 3 stub. Phase 2 only delivers the section anchor target. Not a Phase 2 regression. |

No blockers or warnings found. All anti-patterns are intentional or out-of-scope for this phase.

### Missing Asset Note

| Asset | Referenced At | Exists | Impact |
|-------|--------------|--------|--------|
| `assets/videos/demo.webm` | `index.html` line 271 | No | Browser falls back to `demo.mp4` (line 272) which exists. Video will play. This is a known open item documented in 02-02-SUMMARY.md "User Setup Required". |

### Human Verification Required

#### 1. Hero Split Layout and Gradient Rendering

**Test:** Open `index.html` in a browser at 1280px viewport width.
**Expected:** Two-column layout — headline, subheadline, and 3 CTAs stacked on the left; phone screenshot on the right. A subtle gradient tint (accent orange toward bottom-right) visible in the hero background.
**Why human:** CSS grid layout and Tailwind v4 gradient rendering (`bg-linear-to-br`) cannot be confirmed by static file analysis.

#### 2. Mobile Responsive Behavior

**Test:** Resize the browser to 375px width and scroll through all sections.
**Expected:** Hero stacks vertically (text first, then phone mockup). Features collapse to single column. Gallery carousel is horizontally swipeable. Video does not trigger a fullscreen overlay. No horizontal overflow on the page body.
**Why human:** Responsive breakpoint behavior and overflow detection require browser rendering.

#### 3. Screenshot and Desktop Image Rendering

**Test:** In the gallery section, confirm all 7 phone screenshots and the desktop screenshot display without broken-image icons.
**Expected:** All images visible with `rounded-2xl shadow-lg` styling. Desktop screenshot spans the full content width.
**Why human:** Relative asset paths (`assets/images/screenshots/...`) depend on the file-serving context and cannot be confirmed by grep.

#### 4. Video Inline Autoplay (especially on mobile)

**Test:** Load the page on a mobile device or mobile emulator. Observe the video in the gallery section.
**Expected:** Video plays muted, looping, inline (no fullscreen takeover). Falls back to MP4 (demo.webm is absent).
**Why human:** The `autoplay muted loop playsinline` combination is correct in markup, but actual autoplay behavior depends on browser policy and must be verified in a real browser environment.

#### 5. "Launch Web App" Smooth Scroll

**Test:** Click the "Launch Web App" button in the hero section.
**Expected:** Page smoothly animates to the `#web-app` section (not an instant jump).
**Why human:** Smooth scroll is triggered by `class="scroll-smooth"` on `<html>`, which only activates in a browser context.

### Gaps Summary

No blocking gaps found. All 11 requirements are implemented and wired correctly in `index.html`. The five items above require human browser verification before the phase can be fully signed off — they are all visual/behavioral concerns that cannot be confirmed programmatically.

The one structural note worth monitoring: `demo.webm` is absent from disk. The browser will gracefully fall back to `demo.mp4`, so there is no functional failure, but adding `demo.webm` later would improve video performance on supporting browsers.

---

_Verified: 2026-02-26T18:00:00Z_
_Verifier: Claude (gsd-verifier)_

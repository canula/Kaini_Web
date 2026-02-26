# Phase 3: Web App Demo & Pro Activation - Context

**Gathered:** 2026-02-26
**Status:** Ready for planning

<domain>
## Phase Boundary

**RESCOPED from original requirements.** The web app exists as a separate product — this landing page is the purchase destination, not the app host.

This phase delivers:
1. A Free vs Pro comparison section to drive conversions
2. A "Buy Pro License" link to Lemon Squeezy checkout
3. Merged features + gallery into paired feature/image sections
4. Charcoal grey palette (replacing dark blue)
5. Removal of the empty Web App stub section
6. Navigation updates to reflect new page structure

**Not in scope:** License key activation (handled in the app), static app UI placeholder, functional demo.

**User flow:** Free app → user hits Pro lock → app directs to landing page → user reads comparison → buys via Lemon Squeezy (new tab) → gets key via email → enters key in app.

</domain>

<decisions>
## Implementation Decisions

### Free vs Pro Comparison
- Two-column comparison table (classic SaaS pricing style)
- Pro features (for now): Phrase Mode, MIDI Import, Custom Instruments
- Free features: everything else (Record Voice, Motion/Contour/Rhythm Presets, Microtonal Support, Note Edit Tracker)
- Feature list may be updated later to match the actual app — use these three as Pro-locked for now

### Purchase Flow
- "Buy Pro License" links to Lemon Squeezy checkout (placeholder URL for now)
- Opens in a new tab — user keeps the landing page open
- Lemon Squeezy handles key delivery via email/receipt — no confirmation on the landing page
- No activation input on the landing page — activation happens in the app

### Web App Launch CTA
- No standalone Web App section — remove the current `#web-app` stub entirely
- The hero's existing "Launch Web App" button is sufficient
- Hero CTA gets a placeholder URL that will be filled in before publishing (site won't go live until app is ready)
- Nav link for "Web App" should either be removed or point to the hero CTA / external app URL

### Layout Restructure — Features + Gallery Merge
- Replace separate Features grid and Gallery carousel with paired feature/image sections
- Each feature gets its own row: text on one side, relevant image/video on the other
- Alternating layout (image left/text right, then swap) — Claude decides exact pattern for best visual flow

**Feature-to-image pairings:**
| Feature | Media | Asset |
|---------|-------|-------|
| MIDI Import | Phone screenshot | phone-02.png |
| Note Edit Tracker | Phone screenshot | phone-07.png |
| Phrase Generation | Phone screenshot | phone-06.png |
| Motion / Contour / Rhythm Presets | Demo video | demo.webm / demo.mp4 |
| Record Voice | Desktop screenshot | desktop-01.png |
| Microtonal Support | Phone screenshot | any remaining (phone-03, 04, or 05) |

- No standalone carousel — all screenshots live next to their paired feature
- Unused phone screenshots (from original 7) are simply not displayed

### Color Palette
- Switch from dark blue (`#0f172a` / `#1e293b`) to charcoal grey
- Keep orange accent (`#f97316`) for buttons and highlights
- Claude picks exact charcoal values that pair well with the orange accent

### Page Structure (new order)
- Hero (with "Launch Web App" CTA + store badges)
- Features (paired with images, alternating layout)
- Free vs Pro comparison table (with Buy Pro CTA)
- Contact
- Footer

### Claude's Discretion
- Exact charcoal grey values for surface/surface-alt
- Phone screenshot sizing in paired layout (phone-scale vs larger)
- Buy button placement (inside comparison table, hero, nav, or multiple)
- Whether price is shown on page or deferred to Lemon Squeezy checkout
- Alternating layout pattern details
- Typography and spacing adjustments for the new layout

</decisions>

<specifics>
## Specific Ideas

- "Features and see-it-in-action feels flat" — user wants visual proof paired directly with each feature description
- "Not sure if we first have the image on one side and text on another then keep swapping" — alternating layout, Claude decides exact execution
- "I don't like the dark blues — switch to charcoal grey"
- Site won't be published until the web app is ready, so placeholder URLs are fine

</specifics>

<deferred>
## Deferred Ideas

- Actual Lemon Squeezy checkout URL — user will provide before publishing
- Web app URL — will be filled in when the app is ready
- Google Play and Microsoft Store real URLs — existing TODOs in hero section
- Updated Pro feature list — currently using placeholder three features, will match actual app later

</deferred>

---

*Phase: 03-web-app-demo-pro-activation*
*Context gathered: 2026-02-26*

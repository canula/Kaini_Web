# Phase 3: Web App Demo & Pro Activation - Research

**Researched:** 2026-02-26
**Domain:** Static HTML restructure — Free/Pro comparison section, paired feature/image layout, charcoal palette, Lemon Squeezy CTA
**Confidence:** HIGH

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

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

#### Free vs Pro Comparison
- Two-column comparison table (classic SaaS pricing style)
- Pro features (for now): Phrase Mode, MIDI Import, Custom Instruments
- Free features: everything else (Record Voice, Motion/Contour/Rhythm Presets, Microtonal Support, Note Edit Tracker)
- Feature list may be updated later to match the actual app — use these three as Pro-locked for now

#### Purchase Flow
- "Buy Pro License" links to Lemon Squeezy checkout (placeholder URL for now)
- Opens in a new tab — user keeps the landing page open
- Lemon Squeezy handles key delivery via email/receipt — no confirmation on the landing page
- No activation input on the landing page — activation happens in the app

#### Web App Launch CTA
- No standalone Web App section — remove the current `#web-app` stub entirely
- The hero's existing "Launch Web App" button is sufficient
- Hero CTA gets a placeholder URL that will be filled in before publishing (site won't go live until app is ready)
- Nav link for "Web App" should either be removed or point to the hero CTA / external app URL

#### Layout Restructure — Features + Gallery Merge
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

#### Color Palette
- Switch from dark blue (`#0f172a` / `#1e293b`) to charcoal grey
- Keep orange accent (`#f97316`) for buttons and highlights
- Claude picks exact charcoal values that pair well with the orange accent

#### Page Structure (new order)
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

### Deferred Ideas (OUT OF SCOPE)
- Actual Lemon Squeezy checkout URL — user will provide before publishing
- Web app URL — will be filled in when the app is ready
- Google Play and Microsoft Store real URLs — existing TODOs in hero section
- Updated Pro feature list — currently using placeholder three features, will match actual app later
</user_constraints>

<phase_requirements>
## Phase Requirements

**IMPORTANT NOTE:** The original REQUIREMENTS.md lists APP-01 through PRO-05 as Phase 3 targets, but CONTEXT.md has fully rescoped Phase 3. The CONTEXT.md supersedes the requirements document. The deliverables are entirely different from what APP-01 through PRO-05 describe. The planner should treat CONTEXT.md as the authoritative scope and map these requirement IDs to the rescoped deliverables below.

| ID | Original Description (REQUIREMENTS.md) | Rescoped Deliverable (CONTEXT.md) |
|----|----------------------------------------|----------------------------------|
| APP-01 | Static layout with dropdowns and Generate button | REPLACED: Paired feature/image section replaces the flat Features grid |
| APP-02 | Audio player placeholder element | REPLACED: Video embed (demo.webm/mp4) embedded in Motion/Contour/Rhythm paired row |
| APP-03 | Pro badge overlay on advanced features | REPLACED: Pro column in Free vs Pro comparison table marks Phrase Mode, MIDI Import, Custom Instruments |
| APP-04 | data-pro-lock attributes on locked elements | REPLACED: No JS lock UI on landing page; comparison table communicates gating visually |
| PRO-01 | License key input + Submit button | OUT OF SCOPE: Activation happens in the app, not on the landing page |
| PRO-02 | Async Lemon Squeezy API validation placeholder | OUT OF SCOPE: No API calls on landing page |
| PRO-03 | localStorage isPro flag on success | OUT OF SCOPE: localStorage handled by the app |
| PRO-04 | Loading/success/error states | OUT OF SCOPE: No activation form on landing page |
| PRO-05 | "Buy a Pro License" link to Lemon Squeezy | RETAINED: Buy Pro CTA in comparison section, opens in new tab |
</phase_requirements>

---

## Summary

Phase 3 is a static HTML restructuring and content addition exercise. No new libraries, build tools, or JavaScript frameworks are introduced. The entire phase is executed within the single `index.html` file using the existing Tailwind CSS v4 CDN setup and vanilla JS already in place.

The scope has three distinct work areas: (1) replace the two separate Features and Gallery sections with a unified paired feature/image layout; (2) add a Free vs Pro comparison section with a Lemon Squeezy buy CTA; (3) update the color palette from dark navy to charcoal grey and clean up the nav and page structure. The `#web-app` stub section is deleted and nav links updated.

No external research into new libraries or APIs is needed. The research focus is on: correct Tailwind v4 utility patterns for alternating two-column layouts, charcoal grey color values that visually pair well with `#f97316` orange, and comparison table markup patterns for SaaS pricing pages.

**Primary recommendation:** Execute as three sequential HTML edits to `index.html` — palette + theme tokens first, then paired features section, then Pro comparison section — so each is visually verifiable before proceeding.

---

## Standard Stack

### Core (unchanged from prior phases)

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Tailwind CSS v4 (CDN) | @4 (browser build) | Utility styling | Already in project — `https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4` |
| Vanilla JS | ES2020 | Interaction | Project constraint: no frameworks |
| HTML5 | — | Markup | Single `index.html` static page |

### No New Dependencies
This phase requires zero new libraries. Everything is HTML + Tailwind v4 utilities.

**Installation:** None required.

---

## Architecture Patterns

### Recommended File Structure
```
index.html   (single file — all changes here)
assets/
├── images/
│   └── screenshots/
│       ├── phone-02.png   (MIDI Import pairing)
│       ├── phone-03.png   (Microtonal Support — or 04/05)
│       ├── phone-06.png   (Phrase Generation pairing)
│       ├── phone-07.png   (Note Edit Tracker pairing)
│       └── desktop-01.png (Record Voice pairing)
└── videos/
    ├── demo.webm          (Motion/Contour/Rhythm pairing)
    └── demo.mp4           (fallback)
```

### Pattern 1: Charcoal Grey Palette — @theme token replacement

**What:** Replace the existing dark navy tokens (`#0f172a`, `#1e293b`) with charcoal grey values inside the `@theme` block. Charcoal greys that pair well with `#f97316` orange sit in the 15–25% lightness range in HSL.

**Recommended values (Claude's discretion — confirmed rationale):**
- `surface`: `#1a1a1a` (near-black charcoal, neutral warm-dark)
- `surface-alt`: `#2a2a2a` (slightly lighter charcoal for section contrast)
- `nav`: `#1a1a1a` (matches surface for navbar)
- Keep `heading: #f8fafc`, `body: #94a3b8`, `accent: #f97316`, `accent-hover: #ea580c` unchanged

**Why these values:** `#1a1a1a` and `#2a2a2a` are neutral charcoals with no blue tint. The orange accent `#f97316` reads warm against neutral dark backgrounds; against cool navies the pairing is slightly mismatched. These values improve the orange/dark contrast relationship.

**Example:**
```html
<!-- Source: Tailwind v4 @theme pattern — confirmed in project decisions [01-01] -->
<style type="text/tailwindcss">
  @theme {
    --color-accent: #f97316;
    --color-accent-hover: #ea580c;
    --color-surface: #1a1a1a;
    --color-surface-alt: #2a2a2a;
    --color-heading: #f8fafc;
    --color-body: #94a3b8;
    --color-nav: #1a1a1a;
  }
</style>
```

Also update `<meta name="theme-color" content="#1a1a1a" />` to match.

### Pattern 2: Paired Feature/Image Section — Alternating Two-Column Layout

**What:** Each feature row is a two-column grid. Text column and media column swap sides on alternating rows. On mobile, media stacks below text.

**Alternating pattern (Claude's recommendation):**
- Row 1: Text left, image right (default)
- Row 2: Image left, text right — achieved with `md:order-last` on the text div
- Repeat pattern for all 6 features

**Phone screenshot sizing:** Use `w-48 md:w-56` (same as gallery carousel, visually tested). Desktop screenshot (`desktop-01.png`) should be `w-full rounded-2xl` since it's wide format.

**Video embed:** Reuse exact pattern from Phase 2 gallery (`autoplay muted loop playsinline`) — all 4 attributes required per project decision [Phase 02-02].

**Example — single feature row:**
```html
<!-- Row: text left, image right (default pattern) -->
<div class="grid md:grid-cols-2 gap-12 items-center py-16 border-b border-white/10">
  <!-- Text column -->
  <div>
    <div class="flex items-center gap-3 mb-3">
      <!-- SVG icon (reuse from existing features section) -->
      <svg class="w-8 h-8 text-accent flex-shrink-0">...</svg>
      <h3 class="text-xl font-bold text-heading">MIDI Import</h3>
    </div>
    <p class="text-body">Bring in MIDI files from your DAW...</p>
  </div>
  <!-- Media column -->
  <div class="flex justify-center">
    <img src="assets/images/screenshots/phone-02.png"
         alt="TC-01 MIDI import screen"
         class="w-48 md:w-56 rounded-2xl shadow-lg" />
  </div>
</div>

<!-- Row: image left, text right (swap pattern) -->
<div class="grid md:grid-cols-2 gap-12 items-center py-16 border-b border-white/10">
  <!-- Text column — pushed right on desktop via order -->
  <div class="md:order-last">
    ...
  </div>
  <!-- Media column — natural left position -->
  <div class="flex justify-center">
    ...
  </div>
</div>
```

**Recommended feature order and alternating assignment:**
| Row | Feature | Side (text) | Media |
|-----|---------|-------------|-------|
| 1 | Record Voice | Left | desktop-01.png (right) |
| 2 | MIDI Import | Right | phone-02.png (left) |
| 3 | Motion / Contour / Rhythm | Left | demo video (right) |
| 4 | Phrase Generation | Right | phone-06.png (left) |
| 5 | Microtonal Support | Left | phone-03.png (right) |
| 6 | Note Edit Tracker | Right | phone-07.png (left) |

This order puts the desktop screenshot first (visually strong, wide), video in the middle (visual peak), and wraps with phone shots.

### Pattern 3: Free vs Pro Comparison Section

**What:** Two-column comparison table. Left column = Free tier. Right column = Pro tier. Rows = features. Pro column has a CTA button.

**Classic SaaS pattern for this markup:**

```html
<section id="pricing" class="bg-surface-alt py-24">
  <div class="max-w-4xl mx-auto px-6">
    <h2 class="text-3xl font-bold text-heading text-center mb-4">Free vs Pro</h2>
    <p class="text-body text-center mb-12">Start free. Unlock more when you're ready.</p>

    <div class="grid md:grid-cols-2 gap-6">

      <!-- Free column -->
      <div class="bg-surface rounded-2xl p-8 border border-white/10">
        <h3 class="text-xl font-bold text-heading mb-2">Free</h3>
        <p class="text-body text-sm mb-6">Everything you need to get started</p>
        <ul class="space-y-3 text-body text-sm">
          <li class="flex items-center gap-2">
            <!-- checkmark SVG -->
            Record Voice
          </li>
          <li class="flex items-center gap-2">
            Motion / Contour / Rhythm Presets
          </li>
          <li class="flex items-center gap-2">
            Microtonal Support
          </li>
          <li class="flex items-center gap-2">
            Note Edit Tracker
          </li>
        </ul>
      </div>

      <!-- Pro column -->
      <div class="bg-surface rounded-2xl p-8 border-2 border-accent">
        <h3 class="text-xl font-bold text-heading mb-2">Pro</h3>
        <p class="text-body text-sm mb-6">Advanced tools for serious composers</p>
        <ul class="space-y-3 text-body text-sm mb-8">
          <li class="flex items-center gap-2 text-heading">
            <!-- checkmark SVG in accent color -->
            Everything in Free, plus:
          </li>
          <li class="flex items-center gap-2 text-heading">
            Phrase Mode
          </li>
          <li class="flex items-center gap-2 text-heading">
            MIDI Import
          </li>
          <li class="flex items-center gap-2 text-heading">
            Custom Instruments
          </li>
        </ul>
        <a href="https://PLACEHOLDER.lemonsqueezy.com/checkout"
           target="_blank"
           rel="noopener noreferrer"
           class="block w-full text-center bg-accent hover:bg-accent-hover text-white font-semibold px-6 py-3 rounded-lg transition-colors">
          Buy a Pro License
        </a>
        <p class="text-body text-xs text-center mt-3">License key delivered by email · Activate in-app</p>
      </div>

    </div>
  </div>
</section>
```

**Note:** Price display is Claude's discretion. Recommend deferring price to Lemon Squeezy checkout — no price hardcoded on page. This avoids stale price copy if pricing changes.

### Pattern 4: Navigation Updates

**What:** Remove `#web-app` nav link (both desktop and mobile). Hero CTA href changes from `#web-app` to a placeholder external URL.

```html
<!-- Desktop nav: remove this line -->
<a href="#web-app" class="text-body hover:text-heading transition-colors">Web App</a>

<!-- Mobile menu: remove this line -->
<a href="#web-app" class="block text-body hover:text-heading transition-colors">Web App</a>

<!-- Hero CTA: change href -->
<a href="https://PLACEHOLDER_APP_URL" target="_blank" rel="noopener noreferrer"
   class="inline-flex items-center gap-2 bg-accent hover:bg-accent-hover text-white font-semibold px-6 py-3 rounded-lg transition-colors">
  Launch Web App
</a>
```

Also add `#pricing` link to nav for the new comparison section:
```html
<a href="#pricing" class="text-body hover:text-heading transition-colors">Pricing</a>
```

### Anti-Patterns to Avoid

- **Using `bg-gradient-to-br` instead of `bg-linear-to-br`:** Tailwind v4 renamed gradient utilities. `bg-gradient-to-br` silently fails in v4. Use `bg-linear-to-br` (confirmed project decision [02-01]).
- **Missing mobile video autoplay attributes:** The video element in the paired features layout must include all four: `autoplay muted loop playsinline`. Missing any one breaks mobile autoplay (confirmed project decision [Phase 02-02]).
- **Relative path leading slash on GitHub Pages:** Asset paths must be relative (`assets/images/...` not `/assets/images/...`) — confirmed project decision [02-01].
- **Hardcoding price:** Lemon Squeezy is the source of truth for pricing. Don't put a price on the page — it will become stale.
- **Reusing `#web-app` anchor:** The section is being deleted. Any remaining `href="#web-app"` links will 404-scroll silently. Audit all anchor references before finishing.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Comparison table layout | Custom CSS grid system | Tailwind v4 `grid md:grid-cols-2 gap-6` | Already in project, zero overhead |
| Alternating columns | JS-injected class toggling | CSS `md:order-last` on text div | Pure CSS, no JS needed |
| Pro badge / highlight | Custom component | `border-2 border-accent` on Pro card + different background | Tailwind handles visual hierarchy |
| Checkmark icons | SVG sprite system | Inline heroicon paths (same pattern as features section) | Already established in project |

**Key insight:** This phase is 100% HTML edits. Every visual need is covered by Tailwind utility classes already loaded. No JS, no libraries, no build step.

---

## Common Pitfalls

### Pitfall 1: Forgetting to Remove All `#web-app` References
**What goes wrong:** The section is deleted but nav links and the hero CTA still point to `#web-app`. Clicking them scrolls to nowhere (or top of page).
**Why it happens:** Multiple anchor references in nav (desktop + mobile) and hero CTA all point to the same target.
**How to avoid:** Search `index.html` for `#web-app` before finishing. There are exactly 3 occurrences: desktop nav, mobile nav, hero CTA. All 3 must be updated.
**Warning signs:** Dev tools > find in page for `#web-app` returns results after changes are done.

### Pitfall 2: Gradient Syntax in Tailwind v4
**What goes wrong:** Any new gradient overlays added during palette work use `bg-gradient-to-*` which silently fails in Tailwind v4 (no error, just no gradient).
**Why it happens:** Tailwind v4 renamed gradient utilities to `bg-linear-to-*`.
**How to avoid:** Use `bg-linear-to-br` (confirmed in project decisions).
**Warning signs:** Gradient appears in design but not rendered in browser.

### Pitfall 3: Desktop Screenshot Overflows Paired Layout
**What goes wrong:** `desktop-01.png` is a wide landscape image. In the two-column paired layout it may overflow its column or look tiny if constrained to phone-width sizing.
**Why it happens:** Phone screenshots are portrait, desktop is landscape — different aspect ratios.
**How to avoid:** For the Record Voice row (desktop screenshot), give the media column `w-full` instead of the `w-48 md:w-56` used for phone shots. The grid column already constrains width.
**Warning signs:** Desktop screenshot appears tiny or overflows into the text column.

### Pitfall 4: Video Element in Paired Layout Needs All 4 Attributes
**What goes wrong:** Mobile Safari won't autoplay video without `playsinline`. Omitting `muted` also blocks autoplay on Chrome.
**Why it happens:** Browser autoplay policies require muted + playsinline on mobile.
**How to avoid:** Copy the exact video element from the existing gallery section which already has all 4 attributes.
**Warning signs:** Video shows poster image but doesn't play on mobile.

### Pitfall 5: `@theme` Token Change Doesn't Cascade to `theme-color` Meta
**What goes wrong:** `theme-color` meta tag still references the old navy `#0f172a` after palette change, causing browser chrome (mobile address bar) to stay navy.
**Why it happens:** `@theme` tokens only apply to Tailwind CSS — meta tags are static HTML.
**How to avoid:** When updating `--color-surface` in `@theme`, also update `<meta name="theme-color" content="...">` to match.

---

## Code Examples

### Charcoal @theme Block (complete replacement)
```html
<!-- Source: Tailwind v4 @theme — project pattern confirmed [01-01] -->
<style type="text/tailwindcss">
  @theme {
    --color-accent: #f97316;
    --color-accent-hover: #ea580c;
    --color-surface: #1a1a1a;
    --color-surface-alt: #2a2a2a;
    --color-heading: #f8fafc;
    --color-body: #94a3b8;
    --color-nav: #1a1a1a;
  }
</style>
```

### Buy Pro License Link
```html
<!-- Source: HTML spec, Lemon Squeezy integration is external checkout -->
<a href="https://PLACEHOLDER.lemonsqueezy.com/checkout"
   target="_blank"
   rel="noopener noreferrer"
   class="block w-full text-center bg-accent hover:bg-accent-hover text-white font-semibold px-6 py-3 rounded-lg transition-colors">
  Buy a Pro License
</a>
```

### Alternating Row — Image Left (swap pattern)
```html
<!-- Source: Tailwind v4 grid utilities — confirmed working in project -->
<div class="grid md:grid-cols-2 gap-12 items-center py-16">
  <!-- Text column: pushed to right column on md via order-last -->
  <div class="md:order-last">
    <div class="flex items-center gap-3 mb-3">
      <svg class="w-8 h-8 text-accent flex-shrink-0" ...>...</svg>
      <h3 class="text-xl font-bold text-heading">Feature Name</h3>
    </div>
    <p class="text-body">Feature description text.</p>
  </div>
  <!-- Media column: natural order = left on desktop -->
  <div class="flex justify-center">
    <img src="assets/images/screenshots/phone-XX.png"
         alt="TC-01 screenshot description"
         class="w-48 md:w-56 rounded-2xl shadow-lg" />
  </div>
</div>
```

### Video Row in Paired Layout
```html
<!-- Source: Phase 02-02 gallery — all 4 mobile autoplay attributes required -->
<div class="flex justify-center">
  <video autoplay muted loop playsinline
         poster="assets/images/screenshots/phone-01.png"
         class="w-48 md:w-64 rounded-2xl shadow-lg">
    <source src="assets/videos/demo.webm" type="video/webm" />
    <source src="assets/videos/demo.mp4" type="video/mp4" />
  </video>
</div>
```

### Checkmark List Item (Free/Pro feature rows)
```html
<!-- Heroicon check-circle (consistent with existing SVG style in project) -->
<li class="flex items-center gap-2">
  <svg class="w-5 h-5 text-accent flex-shrink-0" fill="none" viewBox="0 0 24 24"
       stroke-width="1.5" stroke="currentColor" aria-hidden="true">
    <path stroke-linecap="round" stroke-linejoin="round"
          d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
  </svg>
  <span>Feature Name</span>
</li>
```

---

## State of the Art

| Old Approach | Current Approach | Reason |
|--------------|------------------|--------|
| Dark navy (`#0f172a` / `#1e293b`) | Charcoal grey (`#1a1a1a` / `#2a2a2a`) | User preference — orange accent reads better on neutral dark |
| Separate Features grid + Gallery carousel | Paired feature/image rows with alternating layout | "Features and see-it-in-action feels flat" — user wants visual proof paired directly |
| `#web-app` stub section | Removed — hero CTA is sufficient | App is a separate product; stub was empty placeholder |
| No upsell section | Free vs Pro comparison table | Core conversion goal of the landing page |

---

## Open Questions

1. **Phone screenshot sizing in paired layout**
   - What we know: Gallery uses `w-48 md:w-56` (verified working visually in Phase 2)
   - What's unclear: Whether this feels proportionate in a two-column layout vs the full-width carousel context
   - Recommendation: Use `w-48 md:w-56` as starting point. Add visual checkpoint task after implementation to review sizing.

2. **Price display on comparison table**
   - What we know: Lemon Squeezy handles checkout; price is deferred per CONTEXT.md
   - What's unclear: Whether "no price on page" feels incomplete to visitors
   - Recommendation: Omit price, add copy like "License key delivered by email · Activate in-app" below the CTA button. Price appears naturally on the Lemon Squeezy checkout page.

3. **Nav item for comparison/pricing section**
   - What we know: `#web-app` link is being removed; page structure changes
   - What's unclear: Whether to add a "Pricing" nav link, or keep nav as Features / Gallery / Contact
   - Recommendation: Add `<a href="#pricing">Pricing</a>` to both desktop and mobile nav. This is a standard SaaS landing page nav pattern and helps conversion.

---

## Sources

### Primary (HIGH confidence)
- Project `index.html` (read directly) — complete current state of markup, Tailwind tokens, existing sections
- `.planning/phases/03-web-app-demo-pro-activation/03-CONTEXT.md` — authoritative phase scope, locked decisions
- `.planning/STATE.md` — confirmed prior decisions: Tailwind v4 gradient syntax, relative paths, video attributes, color tokens

### Secondary (MEDIUM confidence)
- Tailwind v4 `@theme` token pattern — confirmed in production via project history (Phase 01-01, 02-01)
- `bg-linear-to-br` vs `bg-gradient-to-br` — confirmed in project decision log [02-01]
- Video autoplay attributes — confirmed in project decision log [Phase 02-02]

### Tertiary (LOW confidence)
- Charcoal colour values `#1a1a1a` / `#2a2a2a` — derived from colour theory reasoning (neutral dark + warm orange accent). Visual checkpoint required during implementation.

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — no new libraries, all existing patterns
- Architecture: HIGH — all patterns verified from existing codebase and project decisions
- Pitfalls: HIGH — most derived from documented prior decisions; `#web-app` reference cleanup is mechanical

**Research date:** 2026-02-26
**Valid until:** 2026-03-28 (static HTML patterns, 30 days)

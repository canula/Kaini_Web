# Phase 2: Marketing Sections - Research

**Researched:** 2026-02-26
**Domain:** Static HTML marketing sections — hero, features grid, gallery carousel, contact placeholder — using Tailwind CSS v4 CDN and vanilla JS
**Confidence:** HIGH

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions

**Hero layout & CTAs**
- Gradient overlay background (dark surface to transparent with subtle accent color)
- Split layout: text/CTAs on one side, phone mockup screenshot on the other
- All three CTAs inline in a single row: "Launch Web App" button + Google Play badge + Microsoft Store badge
- On mobile, stacks vertically — stacking order (text-first vs mockup-first) is Claude's discretion

**Feature card presentation**
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

**Gallery & media**
- 8 phone screenshots available as real assets (not placeholders)
- Screenshots displayed flat with rounded corners and subtle shadow — no CSS device frames
- Horizontal snap-scroll carousel on mobile for phone screenshots
- Desktop screenshot included — placement relative to carousel/video is Claude's discretion
- Video is a local MP4/WebM file (self-hosted, HTML `<video>` tag)
- Video is a phone screen recording (portrait aspect ratio) — displayed below the carousel
- Video plays autoplay muted loop playsinline per requirements

**Contact section**
- Centered `#tally-form-container` div with HTML comment placeholder for Tally.so embed
- Minimal section — just the heading and placeholder container

### Claude's Discretion
- Mobile hero stacking order (text-first vs mockup-first)
- Feature card panel style (surface-alt cards vs borderless items)
- Desktop screenshot placement within gallery flow
- Section subheadings (1-line intros under h2s) — per section as needed
- Exact spacing, typography scale, and responsive breakpoint details
- Gallery carousel navigation indicators (dots, arrows, or scroll-only)

### Deferred Ideas (OUT OF SCOPE)
None — discussion stayed within phase scope
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|-----------------|
| HERO-01 | Clean headline and subheadline explaining the value proposition | Hero split layout with gradient background pattern; copy tone: casual & approachable |
| HERO-02 | "Launch Web App" CTA button that smooth-scrolls to web app section | Page already has `scroll-smooth` on `<html>`; anchor href="#web-app" works natively |
| HERO-03 | Official Google Play badge with placeholder link | Official badge image at `play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png`; link to `https://play.google.com/store/apps/details?id=PACKAGE_NAME` |
| HERO-04 | Official Microsoft Store badge with placeholder link | Microsoft web component: `<ms-store-badge>` via CDN script, or plain SVG image fallback at `https://get.microsoft.com/images/en-us%20dark.svg` |
| FEAT-01 | Section highlighting 6-8 key capabilities with benefits-first language | 2-column CSS grid; 6 specific features defined in decisions; copy tone: benefits-first, 2-3 sentences |
| FEAT-02 | Visual icons or illustrations for each feature | Heroicons inline SVG (copy from heroicons.com); no build tools needed; styled via `class="text-accent w-8 h-8"` |
| GLRY-01 | Phone screenshots displayed inside CSS phone frame mockups | Decision overrides: flat display with `rounded-2xl shadow-lg`, NO CSS device frames |
| GLRY-02 | Desktop UI screenshot display | Desktop screenshot displayed in gallery; placement (above/below carousel, or in its own row) is Claude's discretion |
| GLRY-03 | Embedded phone video (autoplay muted loop playsinline) | `<video autoplay muted loop playsinline>` with `<source src>` for MP4 and WebM; poster attribute recommended |
| GLRY-04 | Horizontal scroll/carousel on mobile for phone mockups | CSS-only snap scroll: `overflow-x-auto snap-x snap-mandatory` on container; `snap-center shrink-0` on each item; hide scrollbar via `[&::-webkit-scrollbar]:hidden` in CDN mode |
| CONT-01 | Contact section with centered `#tally-form-container` div and HTML comment placeholder | `<div id="tally-form-container" class="text-center"><!-- Tally.so embed goes here --></div>` |
</phase_requirements>

---

## Summary

Phase 2 builds four content sections into the existing page skeleton (`index.html`): a hero with split layout and store badge CTAs, a features grid with inline SVG icons, a gallery combining a phone screenshot carousel with a desktop screenshot and autoplay video, and a minimal contact placeholder. The entire implementation is static HTML + Tailwind CSS v4 CDN — no build tools, no JS frameworks.

The primary constraint is the CDN-only environment. Several standard Tailwind patterns (plugins, `@utility`, `@apply`) are unavailable or restricted in the browser CDN runtime. Key workarounds have been identified and verified: gradient syntax changed from `bg-gradient-*` to `bg-linear-*` in v4; custom scrollbar hiding uses the arbitrary property syntax `[&::-webkit-scrollbar]:hidden`; the Microsoft Store badge can use either their official web component (external JS load) or a plain SVG `<img>` fallback.

The gallery carousel is pure CSS scroll snap — no JS required. Video autoplay on mobile requires all four attributes (`autoplay muted loop playsinline`) to work reliably in Safari/iOS. Feature icons are copied inline from heroicons.com as raw SVG — the correct approach when there is no build pipeline.

**Primary recommendation:** Build section by section in a single `index.html` pass, using Tailwind v4 `bg-linear-*` gradient syntax, CSS-only snap carousel, inline SVG icons, and the MS Store web component with `<noscript>` SVG fallback.

---

## Standard Stack

### Core

| Library | Version | Purpose | Why Standard |
|---------|---------|---------|--------------|
| Tailwind CSS | v4 (CDN) | Utility-first styling | Already established in Phase 1; CDN already loaded |
| Vanilla JS | ES2020 | Interactivity (hamburger; no carousel JS needed) | Project constraint: no frameworks |
| HTML5 `<video>` | Native | Autoplay muted gallery video | Self-hosted MP4/WebM; no third-party player needed |

### Supporting

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| MS Store badge web component | Latest (CDN) | Official Microsoft Store badge with auto theme | When MS Store product ID is known; loads from `get.microsoft.com` |
| Google Play badge (image) | Static PNG | Official Google Play badge | Always — Google provides a stable hosted badge image URL |
| Heroicons (inline SVG) | v2 | Feature section icons | Copy-paste from heroicons.com; no install needed |

### Alternatives Considered

| Instead of | Could Use | Tradeoff |
|------------|-----------|----------|
| MS Store web component | Plain SVG `<img>` fallback | Web component requires external JS load from `get.microsoft.com`; plain SVG is simpler and works offline; loses auto-theme detection |
| Heroicons inline SVG | Lucide CDN script | Heroicons inline requires no JS; Lucide CDN adds a script tag but avoids copy-paste repetition — for 6 icons, inline is less overhead |
| CSS-only snap carousel | JS-driven carousel (Splide, Swiper) | CSS-only has zero JS overhead and no external dependency; JS libraries add dots/arrows/keyboard nav but are overkill for a simple 8-image mobile carousel |

**Installation:** None required — all assets are static or CDN-loaded via the existing `<script>` tag.

---

## Architecture Patterns

### Recommended Section Structure in `index.html`

Each section replaces its existing stub. The page already has: nav, hero stub, features stub, gallery stub, web-app stub, contact stub, footer. Phase 2 replaces the four stubs marked Pending in REQUIREMENTS.md.

```
index.html
├── <section id="hero">       ← replace hero stub
├── <section id="features">   ← replace features stub
├── <section id="gallery">    ← replace gallery stub
├── <section id="web-app">    ← UNTOUCHED (Phase 3)
├── <section id="contact">    ← replace contact stub
```

Asset paths (relative from root, consistent with Phase 1 conventions):
```
assets/
├── images/
│   ├── screenshots/
│   │   ├── phone-01.png … phone-08.png
│   │   └── desktop-01.png
│   └── (badges can use remote URLs — no local copy needed)
└── videos/
    └── demo.mp4  (and/or demo.webm)
```

### Pattern 1: Hero — Split Layout with Gradient

**What:** Two-column flex/grid on desktop (md:grid-cols-2), stacks to single column on mobile. Left column: headline + subheadline + CTA row. Right column: phone screenshot image. Background uses a `bg-linear-to-br` from surface to a semi-transparent accent.

**When to use:** Any hero where marketing copy and a product shot share equal visual weight.

```html
<!-- Source: Tailwind CSS v4 official docs (tailwindcss.com/docs/background-image) -->
<section id="hero" class="relative overflow-hidden bg-surface pt-32 pb-24">
  <!-- Gradient overlay -->
  <div class="pointer-events-none absolute inset-0 bg-linear-to-br from-surface via-surface to-accent/10"></div>

  <div class="relative max-w-6xl mx-auto px-6 grid md:grid-cols-2 gap-12 items-center">
    <!-- Text column -->
    <div>
      <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold text-heading mb-4">
        Create melodies in seconds.<br>No music degree required.
      </h1>
      <p class="text-lg text-body mb-8 max-w-lg">
        TC-01 Tone Calculator turns your voice, presets, and ideas into real melodies — instantly.
        Free on Android, Windows, and Web.
      </p>

      <!-- CTA row: all three inline -->
      <div class="flex flex-wrap items-center gap-4">
        <a href="#web-app"
           class="inline-flex items-center gap-2 bg-accent hover:bg-accent-hover text-white font-semibold px-6 py-3 rounded-lg transition-colors">
          Launch Web App
        </a>

        <!-- Google Play badge -->
        <a href="https://play.google.com/store/apps/details?id=PACKAGE_NAME" target="_blank" rel="noopener">
          <img src="https://play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png"
               alt="Get it on Google Play" class="h-12" />
        </a>

        <!-- Microsoft Store badge (web component + plain img fallback) -->
        <script type="module" src="https://get.microsoft.com/badge/ms-store-badge.bundled.js"></script>
        <ms-store-badge productid="PRODUCT_ID" theme="dark" language="en-us"></ms-store-badge>
      </div>
    </div>

    <!-- Phone mockup column -->
    <div class="flex justify-center">
      <img src="/assets/images/screenshots/phone-01.png"
           alt="TC-01 app screenshot" class="w-64 md:w-72 rounded-2xl shadow-2xl" />
    </div>
  </div>
</section>
```

### Pattern 2: Features Grid — 2-Column with Inline SVG Icons

**What:** `grid grid-cols-1 md:grid-cols-2 gap-6` container. Each card uses either a surface-alt background (`bg-surface-alt rounded-xl p-6`) or a borderless flex row. Icon is inline SVG styled `class="w-8 h-8 text-accent flex-shrink-0"`.

**When to use:** Any 6+ feature showcase where icon + title + copy repeats uniformly.

```html
<!-- Source: heroicons.com (copy inline SVG); Tailwind CSS grid docs -->
<section id="features" class="bg-surface-alt py-24">
  <div class="max-w-6xl mx-auto px-6">
    <h2 class="text-3xl font-bold text-heading mb-3">Features</h2>
    <p class="text-body mb-12">Everything you need to create melodies your way.</p>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Feature card (repeat 6×) -->
      <div class="bg-surface rounded-xl p-6 flex gap-4">
        <!-- Heroicons inline SVG — microphone example -->
        <svg class="w-8 h-8 text-accent flex-shrink-0" fill="none" viewBox="0 0 24 24"
             stroke-width="1.5" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round"
                d="M12 18.75a6 6 0 0 0 6-6v-1.5m-6 7.5a6 6 0 0 1-6-6v-1.5
                   m6 7.5v3.75m-3.75 0h7.5M12 15.75a3 3 0 0 1-3-3V4.5a3 3 0 1 1 6 0v8.25a3 3 0 0 1-3 3Z" />
        </svg>
        <div>
          <h3 class="text-heading font-semibold mb-1">Record Voice</h3>
          <p class="text-body text-sm">Hum a melody and TC-01 transcribes it into notes instantly.
            No keyboard, no theory — just sing what's in your head.</p>
        </div>
      </div>

      <!-- ... 5 more cards ... -->
    </div>
  </div>
</section>
```

### Pattern 3: Gallery — CSS Snap Carousel + Desktop Screenshot + Video

**What:** Three sub-components in one section:
1. Horizontal snap-scroll carousel for 8 phone screenshots (mobile-first; on desktop becomes a static row or remains scrollable)
2. Desktop screenshot displayed in its own row
3. `<video>` element for the screen recording

**When to use:** Mixed media gallery with touch-friendly scrolling on mobile.

```html
<!-- Source: Tailwind CSS scroll snap docs; MDN video autoplay guide -->
<section id="gallery" class="bg-surface py-24">
  <div class="max-w-6xl mx-auto px-6">
    <h2 class="text-3xl font-bold text-heading mb-3">See it in action</h2>
    <p class="text-body mb-10">TC-01 on Android — swipe through the screenshots.</p>

    <!-- Phone screenshot snap carousel -->
    <div class="flex overflow-x-auto snap-x snap-mandatory gap-4 pb-4
                [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
      <!-- Repeat for each of 8 screenshots -->
      <div class="snap-center shrink-0 w-48 md:w-56">
        <img src="/assets/images/screenshots/phone-01.png"
             alt="TC-01 screenshot 1"
             class="rounded-2xl shadow-lg w-full" />
      </div>
      <!-- phone-02 through phone-08 ... -->
    </div>

    <!-- Desktop screenshot -->
    <div class="mt-12">
      <img src="/assets/images/screenshots/desktop-01.png"
           alt="TC-01 desktop interface"
           class="rounded-2xl shadow-lg w-full" />
    </div>

    <!-- Autoplay video -->
    <div class="mt-12 flex justify-center">
      <video
        autoplay muted loop playsinline
        poster="/assets/images/screenshots/phone-01.png"
        class="w-48 md:w-64 rounded-2xl shadow-lg">
        <source src="/assets/videos/demo.webm" type="video/webm" />
        <source src="/assets/videos/demo.mp4" type="video/mp4" />
      </video>
    </div>
  </div>
</section>
```

### Pattern 4: Contact — Minimal Placeholder

```html
<section id="contact" class="bg-surface-alt py-24">
  <div class="max-w-6xl mx-auto px-6 text-center">
    <h2 class="text-3xl font-bold text-heading mb-4">Get in touch</h2>
    <p class="text-body mb-10">Questions, feedback, or just want to say hi — we'd love to hear from you.</p>
    <div id="tally-form-container">
      <!-- Tally.so embed goes here -->
    </div>
  </div>
</section>
```

### Anti-Patterns to Avoid

- **Using `bg-gradient-to-r` in v4:** Renamed to `bg-linear-to-r`. The v3 class may silently produce no gradient in v4 CDN.
- **Using `overflow-x: hidden` on carousel container:** Makes the list completely unscrollable — use `overflow-x-auto` instead.
- **Omitting `playsinline` on video:** Video will attempt fullscreen on iOS Safari instead of playing inline.
- **Omitting `muted` on video:** Autoplay will be blocked entirely by browser policy (iOS, Chrome, Firefox all enforce this).
- **Using `@utility` or `@apply` for custom CSS in CDN mode:** These directives are unsupported in the browser CDN runtime. Use arbitrary property classes (`[&::-webkit-scrollbar]:hidden`) instead.
- **Using `@layer utilities` in CDN style block for complex pseudo-element styles:** The CDN parser has limited support. Stick to Tailwind utility classes and arbitrary properties.
- **Loading Heroicons via CDN script (Lucide) and then referencing icon names:** Adds a JS dependency and data-icon attribute pattern; inline SVG is cleaner and has zero runtime cost for 6 icons.

---

## Don't Hand-Roll

| Problem | Don't Build | Use Instead | Why |
|---------|-------------|-------------|-----|
| Horizontal carousel snap behavior | Custom JS scroll handler with touch events | CSS `snap-x snap-mandatory` + `overflow-x-auto` | Browser-native; handles touch, trackpad, and keyboard; zero JS |
| Scrollbar hiding cross-browser | Manual vendor-prefix CSS block | Tailwind arbitrary `[&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]` | Covers all browsers in one line; no custom CSS block needed |
| Google Play badge image | Self-hosted badge PNG | Google's hosted badge URL (`play.google.com/intl/en_us/badges/...`) | Official URL; always up to date; TOS requires using official assets |
| MS Store badge image + link | Static SVG manually designed | `<ms-store-badge>` web component or official SVG URL | Auto theme detection, localization, and Microsoft compliance |
| Feature icon system | Custom SVG sprite or icon font | Heroicons v2 inline SVG | 6 icons = 6 paste operations; no runtime cost, no external dependency |

**Key insight:** In a no-build, CDN-only project, the correct approach is always "fewer moving parts." Native CSS handles the carousel; remote hosted assets handle the badges; inline SVG handles the icons.

---

## Common Pitfalls

### Pitfall 1: Tailwind v4 Gradient Class Name Change

**What goes wrong:** Developer writes `bg-gradient-to-br from-surface to-accent/10` — class silently produces no gradient because `bg-gradient-to-*` was renamed in v4.
**Why it happens:** Tailwind v3 used `bg-gradient-to-{dir}`; v4 renamed it to `bg-linear-to-{dir}`. The CDN won't error — it just ignores unknown classes.
**How to avoid:** Always use `bg-linear-to-{dir}` in this project.
**Warning signs:** Hero background looks flat/solid despite gradient classes being present.

### Pitfall 2: Video Autoplay Blocked on Mobile

**What goes wrong:** Video doesn't play on iPhone/iPad or Chrome for Android.
**Why it happens:** Mobile browsers require all four attributes simultaneously: `autoplay muted loop playsinline`. Missing any one causes silent failure on at least one platform.
**How to avoid:** Use the full attribute set on every `<video>` element: `<video autoplay muted loop playsinline>`.
**Warning signs:** Video shows static poster frame and never starts; video goes fullscreen unexpectedly on iOS.

### Pitfall 3: Carousel Container with Wrong `overflow` Value

**What goes wrong:** Screenshots don't scroll horizontally; carousel appears broken.
**Why it happens:** Using `overflow-x: hidden` (or Tailwind's `overflow-x-hidden`) instead of `overflow-x-auto`. Hidden prevents all overflow including scroll.
**How to avoid:** Use `overflow-x-auto` on the scroll container, not `overflow-x-hidden`.
**Warning signs:** Horizontal content is clipped; no scrollbar appears even on desktop; touch swipe does nothing.

### Pitfall 4: snap-x Without snap-center on Children

**What goes wrong:** Horizontal scroll works but doesn't "snap" — items drift freely.
**Why it happens:** `snap-x snap-mandatory` on the container defines the snap axis, but without `snap-center` (or `snap-start`) on each child, the browser has no snap points to lock to.
**How to avoid:** Every carousel `<div>` child must have `snap-center shrink-0`.
**Warning signs:** Scroll feels free-flowing with no snapping behavior; items stop mid-image.

### Pitfall 5: CDN Custom CSS Directives Unsupported

**What goes wrong:** Developer adds `@utility no-scrollbar { &::-webkit-scrollbar { display: none; } }` inside `<style type="text/tailwindcss">` — it's silently ignored.
**Why it happens:** The Tailwind v4 browser CDN runtime has limited directive support. `@utility` and `@apply` are build-step features.
**How to avoid:** Use Tailwind's arbitrary property syntax for pseudo-element targeting: `[&::-webkit-scrollbar]:hidden`.
**Warning signs:** The custom class never appears in the rendered stylesheet; scrollbar remains visible.

### Pitfall 6: Missing Asset Files Blocking Gallery Section

**What goes wrong:** Gallery section renders but shows broken image icons.
**Why it happens:** Screenshot and video files must exist at the paths referenced in HTML before the section is live. The STATE.md blocker explicitly calls this out.
**How to avoid:** Confirm file paths before implementing gallery HTML. Use consistent naming convention: `assets/images/screenshots/phone-01.png` through `phone-08.png`, `desktop-01.png`, and `assets/videos/demo.mp4`.
**Warning signs:** Browser network tab shows 404s for image/video URLs.

### Pitfall 7: Microsoft Store Product ID Unknown

**What goes wrong:** MS Store badge links to nothing or a 404.
**Why it happens:** The `<ms-store-badge productid="...">` requires the real Store product ID. Placeholder text will cause a broken badge.
**How to avoid:** Use a clearly-labelled placeholder in the `productid` attribute (e.g., `productid="PLACEHOLDER"`) and note it in an HTML comment. Also add a placeholder `href` on the wrapping anchor. Document as a "fill in before launch" item.

---

## Code Examples

Verified patterns from official sources:

### Tailwind v4 Gradient (Hero Background)

```html
<!-- Source: https://tailwindcss.com/docs/background-image -->
<div class="bg-linear-to-br from-surface via-surface to-accent/10">
  <!-- hero content -->
</div>
```

### CSS Snap Carousel (Gallery — Phone Screenshots)

```html
<!-- Source: https://tailwindcss.com/docs/scroll-snap-type -->
<div class="flex overflow-x-auto snap-x snap-mandatory gap-4 pb-4
            [&::-webkit-scrollbar]:hidden [-ms-overflow-style:none] [scrollbar-width:none]">
  <div class="snap-center shrink-0 w-48">
    <img src="/assets/images/screenshots/phone-01.png"
         alt="Screenshot 1" class="rounded-2xl shadow-lg w-full" />
  </div>
  <!-- Repeat for all 8 screenshots -->
</div>
```

### HTML5 Autoplay Video

```html
<!-- Source: MDN Web Docs — Autoplay guide for media -->
<video autoplay muted loop playsinline
       poster="/assets/images/screenshots/phone-01.png"
       class="w-48 md:w-64 rounded-2xl shadow-lg">
  <source src="/assets/videos/demo.webm" type="video/webm" />
  <source src="/assets/videos/demo.mp4" type="video/mp4" />
</video>
```

### Google Play Badge

```html
<!-- Source: developer.android.com/distribute/marketing-tools/linking-to-google-play -->
<a href="https://play.google.com/store/apps/details?id=PACKAGE_NAME"
   target="_blank" rel="noopener noreferrer">
  <img src="https://play.google.com/intl/en_us/badges/images/generic/en_badge_web_generic.png"
       alt="Get it on Google Play" class="h-12" />
</a>
```

### Microsoft Store Badge (Web Component + Plain SVG Fallback)

```html
<!-- Source: https://github.com/microsoft/app-store-badge -->
<!-- Option A: Web component (requires external JS; auto-themes) -->
<script type="module" src="https://get.microsoft.com/badge/ms-store-badge.bundled.js"></script>
<ms-store-badge productid="PLACEHOLDER" theme="dark" language="en-us"></ms-store-badge>

<!-- Option B: Plain SVG image (no JS dependency) -->
<a href="https://apps.microsoft.com/detail/PLACEHOLDER" target="_blank" rel="noopener">
  <img src="https://get.microsoft.com/images/en-us%20dark.svg"
       alt="Get it from Microsoft" class="h-12" />
</a>
```

### Heroicons Inline SVG (Feature Icon)

```html
<!-- Source: heroicons.com — copy raw SVG, apply Tailwind classes -->
<svg class="w-8 h-8 text-accent flex-shrink-0" fill="none" viewBox="0 0 24 24"
     stroke-width="1.5" stroke="currentColor" aria-hidden="true">
  <!-- paste path data from heroicons.com here -->
</svg>
```

### Smooth Scroll (Already Implemented)

```html
<!-- Already on <html> from Phase 1 — no action needed -->
<html lang="en" class="scroll-smooth">
```

### Tally Contact Placeholder

```html
<!-- Source: REQUIREMENTS.md CONT-01 -->
<div id="tally-form-container" class="max-w-lg mx-auto">
  <!-- Tally.so embed goes here -->
</div>
```

---

## State of the Art

| Old Approach | Current Approach | When Changed | Impact |
|--------------|------------------|--------------|--------|
| `bg-gradient-to-r` | `bg-linear-to-r` | Tailwind v4.0 (Jan 2025) | Must use new class name; old class silently fails |
| `@apply` / `@layer` in CDN | Arbitrary Tailwind classes (`[&::-webkit-scrollbar]:hidden`) | Tailwind v4.0 CDN limitations | Custom CSS goes in arbitrary brackets, not directives |
| oklab color interpolation opt-in | oklab is the DEFAULT in v4 | Tailwind v4.0 | Gradients look better by default; no change needed |
| `-webkit-overflow-scrolling: touch` on iOS | Not needed in iOS 13+ | iOS 13 (2019) | Drop this deprecated property; modern iOS handles momentum scrolling natively |

**Deprecated/outdated:**
- `bg-gradient-to-{dir}`: Replaced by `bg-linear-to-{dir}` in Tailwind v4. Do not use.
- `-webkit-overflow-scrolling: touch`: Deprecated in iOS 13+, no-op in modern Safari. Do not add.
- Tailwind CDN `@utility` directive: Not supported in browser CDN runtime. Use arbitrary properties.

---

## Open Questions

1. **Asset file paths and filenames**
   - What we know: 8 phone screenshots and 1 desktop screenshot are "ready to drop in" (CONTEXT.md). A video MP4/WebM exists.
   - What's unclear: Actual filenames and directory structure not confirmed. The planning docs don't specify filenames.
   - Recommendation: Planner should include a task to inventory/confirm asset files and establish the `assets/images/screenshots/` and `assets/videos/` directory structure before building the gallery section. Use placeholder paths in HTML that match the convention, then swap in real filenames.

2. **Microsoft Store product ID**
   - What we know: REQUIREMENTS.md says "placeholder link" for both store badges. MS badge web component requires a real `productid`.
   - What's unclear: Whether the app is actually listed on the Microsoft Store yet, and if so what the product ID is.
   - Recommendation: Use `productid="PLACEHOLDER"` and wrap in an HTML comment. Plan a follow-up to swap in real IDs. For now, use Option B (plain SVG image) so the badge renders visually even with a placeholder href.

3. **Google Play package name**
   - What we know: STATE.md notes "App store listing URLs needed for badge deep links."
   - What's unclear: Actual Android package name (e.g., `com.kaini.tc01`).
   - Recommendation: Use a clearly labelled placeholder `id=YOUR_PACKAGE_NAME` in the Google Play URL. Badge image will render fine; the link will 404 until the real package name is filled in.

4. **Video file format and both source variants**
   - What we know: Video is "a local MP4/WebM file" per CONTEXT.md.
   - What's unclear: Whether both `.mp4` and `.webm` variants actually exist, or just one.
   - Recommendation: `<source>` both with WebM first (better compression), MP4 second. If only one format exists, include only that `<source>`. Browser silently skips unavailable sources.

---

## Sources

### Primary (HIGH confidence)
- [Tailwind CSS v4 — background-image docs](https://tailwindcss.com/docs/background-image) — verified `bg-linear-to-*` gradient syntax
- [Tailwind CSS v4 — scroll-snap-type docs](https://tailwindcss.com/docs/scroll-snap-type) — verified `snap-x snap-mandatory` + child `snap-center` pattern
- [Tailwind CSS v4 — adding-custom-styles docs](https://tailwindcss.com/docs/adding-custom-styles) — confirmed `@utility` not available in browser CDN runtime; `[&::-webkit-scrollbar]:hidden` is the CDN-compatible alternative
- [Microsoft app-store-badge GitHub README](https://github.com/microsoft/app-store-badge) — verified web component syntax, `theme="dark"`, plain SVG fallback URL
- [Android Developers — linking-to-google-play](https://developer.android.com/distribute/marketing-tools/linking-to-google-play) — verified Google Play badge hosted image URL
- [MDN — Autoplay guide for media](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay) — confirmed `autoplay muted loop playsinline` requirement for iOS Safari

### Secondary (MEDIUM confidence)
- [WebKit blog — New video policies for iOS](https://webkit.org/blog/6784/new-video-policies-for-ios/) — confirmed `playsinline` and `muted` are both required for inline autoplay
- Tailwind v4.0 release blog — confirmed `bg-gradient-*` → `bg-linear-*` rename (cross-verified with official docs)

### Tertiary (LOW confidence)
- Various WebSearch results on scrollbar hiding — flagged; verified via official Tailwind docs that arbitrary property approach is correct for CDN

---

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH — verified against official Tailwind v4 docs, MS badge GitHub, Android developer docs
- Architecture: HIGH — based on existing Phase 1 code (already reviewed index.html) and official docs
- Pitfalls: HIGH for CDN limitations, video autoplay, gradient rename; MEDIUM for asset path question (depends on actual files)

**Research date:** 2026-02-26
**Valid until:** 2026-08-26 (stable — Tailwind v4 CDN, HTML5 video, and badge APIs are stable; review if Tailwind v5 releases)

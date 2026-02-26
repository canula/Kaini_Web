# Feature Research

**Domain:** Music app / SaaS product landing page (static, GitHub Pages)
**Researched:** 2026-02-26
**Confidence:** MEDIUM — findings verified across multiple WebSearch sources; no single authoritative spec exists for this domain combination

---

## Feature Landscape

### Table Stakes (Users Expect These)

Features users assume exist. Missing these = product feels incomplete or untrustworthy.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Hero section with headline + subheadline | Every landing page has one; first impression determines bounce rate — "5-second rule": can they understand the product in 5 sec? | LOW | Headline should be outcome-focused, under 8 words. Subheadline expands with 1-2 lines of key benefits. |
| Clear primary CTA above the fold | Visitors need to know what action to take immediately; buried CTAs kill conversion | LOW | For this project: "Launch Web App" as primary scroll-anchor CTA. Google Play + Microsoft Store badges as secondary. |
| Google Play badge | Any Android app landing page is expected to have it; absence signals the app may be abandoned or unofficial | LOW | Must use official Google Play badge from Google's Partner Marketing Hub. Google requires their badge be same size or larger than other badges. |
| Microsoft Store badge | Windows app listing expectation; without it, Windows users don't know the app exists | LOW | Use official Microsoft Store badge image. Place near Google Play badge. |
| Feature showcase section | SaaS visitors need to understand what the product does before converting; features = trust | MEDIUM | Benefits-first language (what the user gets) not feature-list language (what the app has). Use icons or visual cards. |
| App screenshot gallery with device mockups | Users expect to see the app before downloading; raw screenshots without frames look amateurish | MEDIUM | 4-6 screenshots in phone frames for mobile, 1 desktop screenshot in browser/laptop frame. Above-the-fold hero shot = most compelling screen, not a settings page. |
| SEO meta tags (title, description, OG) | Static pages without meta tags rank poorly and share badly on social; this is baseline hygiene | LOW | title + meta description + og:title + og:description + og:image. JSON-LD SoftwareApplication schema for rich results. |
| Contact section | Users who have questions or issues need a way to reach you; missing = trust gap | LOW | Tally.so iframe placeholder div satisfies this — no custom form needed. |
| Mobile-responsive design | >60% of web traffic is mobile; a non-responsive landing page immediately disqualifies the page | MEDIUM | Tailwind's responsive prefixes handle this; badge and CTA layouts must stack gracefully on small screens. |
| Page load performance | Slow pages = high bounce; Google PageSpeed is a ranking factor | LOW | Static page with CDN Tailwind is naturally fast; main risk is unoptimized images (phone mockup PNGs). Use WebP or compressed PNG. |

### Differentiators (Competitive Advantage)

Features that set this landing page apart from generic app landing pages.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Embedded static web app UI preview | Shows the product's actual interface inline — visitors experience the aesthetic before clicking through; interactive demos lift conversions vs. static screenshots | HIGH | Per PROJECT.md: this is a static layout placeholder (dropdowns, Generate button, audio player stub) — not functional. Achieves the visual differentiator at LOW implementation cost since it's non-functional. |
| Pro feature lock badges inline | Makes the free-vs-Pro distinction immediately visible in context; converts Free users better than a separate pricing page | MEDIUM | Pro badge overlays on specific features (Phrase Mode, MIDI Import, Custom Instruments). Lock icon + "Pro" pill. Users see exactly what they're missing. |
| License key activation modal/section | Eliminates friction for users who already bought Pro via Lemon Squeezy — they can activate without leaving the page | MEDIUM | Input field + Submit button. States: idle, loading, success (unlocks UI), error (invalid key, already used, limit reached). localStorage persistence. Must handle CORS — Lemon Squeezy API called client-side. |
| Phone mockup gallery with video | Video inside a phone frame (or adjacent) demonstrates the sequencer in motion — static screenshots don't convey a music app's feel | MEDIUM | Single phone recording video embed. Autoplay muted loop preferred for ambient effect; user controls for full playback. |
| Dark marketing aesthetic borrowed from app | Creates visual continuity between landing page and app; users arrive at the app and recognize the brand immediately | LOW | Dark palette (zinc-900 range) with marketing-polish overlay. NOT retro-hardware — that's the app. Clean, modern typography over the dark base. |
| Scales/contours/transforms quantified in copy | "100 scales, 25 contours, 8 transforms" is a concrete differentiator vs. vague "AI melody" claims; numbers build credibility | LOW | Surface these numbers prominently in the features section. Competing tools use vague "AI-powered" language — specificity wins. |
| JSON-LD SoftwareApplication schema | Enables rich results in Google (star ratings, price, OS compatibility) — most indie app landing pages skip this entirely | LOW | Add structured data: applicationCategory: "MusicApplication", operatingSystem: "Android, Windows, Web", offers with price, aggregateRating once reviews exist. |

### Anti-Features (Commonly Requested, Often Problematic)

Features that seem like good ideas but create problems for a static no-build-process page.

| Feature | Why Requested | Why Problematic | Alternative |
|---------|---------------|-----------------|-------------|
| Functional audio demo (play melodies in-page) | "Show don't tell" — visitors want to hear the app | Requires Tone.js, React, actual melody generation logic — the entire app stack. Incompatible with static single-file constraint. Ships never. | Static Web App UI placeholder (dropdowns + Generate button stub). Points to the live web app for real interaction. |
| In-page checkout / payment flow | Reduce friction — buy without leaving | Requires Lemon Squeezy JS embed or hosted checkout iFrame + server-side validation. Breaks GitHub Pages static constraint. Creates PCI surface area. | External Lemon Squeezy checkout link. "Buy Pro License" text link is sufficient for this volume. |
| Blog / content marketing section | SEO via content | Requires a build pipeline (MDX, static site generator) to be maintainable. A blog maintained manually in HTML becomes abandoned. | Focus SEO on the landing page itself: keyword-rich copy, JSON-LD, og:tags. Add a blog later if/when there's a build pipeline. |
| Light mode / theme toggle | Accessibility requests | Dark-only is a deliberate brand decision. Adding a toggle doubles CSS complexity, adds JS state, and risks the design looking unpolished in light mode without significant additional design work. | Ensure sufficient contrast ratios in dark mode to satisfy WCAG AA (skip theme toggle). |
| Testimonials / social proof section | Trust-building | No user reviews exist yet (product is pre-launch). Fabricated testimonials are unethical. Placeholder "testimonials coming soon" looks worse than no section. | Launch without testimonials. Add the section once real reviews exist on Google Play / Microsoft Store. |
| Real-time download counter / user count | Social proof via numbers | Requires a backend endpoint or third-party script call. Breaks static constraint. Numbers would be inaccurate at launch. | Use specific product capability numbers instead (100 scales, 25 contours) — these are equally credible and honest. |
| Cookie consent banner | Legal compliance for EU | Adds complexity, degrades UX, requires JS. A static marketing page that sets no tracking cookies and uses no analytics has no legal obligation to show a cookie banner. | Omit analytics entirely (GitHub Pages default). If analytics are added later, add the banner then. |
| Multi-page site structure | SEO via separate pages | Multiple HTML files require navigation, consistent headers/footers — quickly becomes a maintenance burden without a build tool. | Single-page with anchor links (#features, #screenshots, #pro, #contact). Better for a single product at this stage. |

---

## Feature Dependencies

```
Hero Section
    └──contains──> Primary CTA (Launch Web App anchor)
    └──contains──> Google Play Badge
    └──contains──> Microsoft Store Badge

Screenshots Gallery
    └──requires──> Phone mockup images (pre-processed assets)
    └──requires──> Device frame CSS/HTML structure
    └──enhances──> Video embed (same gallery block)

Static Web App UI Preview
    └──requires──> Pro feature lock badges
                       └──enhances──> License Key Activation

License Key Activation
    └──requires──> Lemon Squeezy API (async fetch, client-side)
    └──requires──> localStorage for isPro persistence
    └──unlocks──> Pro badge/lock state on static UI preview

SEO Meta Tags
    └──requires──> OG image asset (screenshot or hero graphic)
    └──enhances──> JSON-LD schema (independent but complementary)

Contact Section
    └──contains──> Tally.so iframe placeholder (no implementation needed)
```

### Dependency Notes

- **License Key Activation requires Lemon Squeezy API access client-side:** The activation call goes to `https://api.lemonsqueezy.com/v1/licenses/activate` from the browser. No CORS issue — Lemon Squeezy's License API accepts browser-originated requests. Store the returned `instance_id` and `license_key` in localStorage for session persistence.
- **Static Web App UI Preview requires Pro feature lock badges:** They are visually coupled — the placeholder UI is the surface where Pro locks appear. Build them together in one phase.
- **Screenshots Gallery depends on pre-processed assets:** Phone mockup PNGs must be compressed/WebP-converted before implementation. This is a content dependency, not a code dependency — resolve it in the content preparation step.
- **OG image is required for effective SEO:** A generic page without `og:image` shares badly on social. The OG image should be a 1200x630px graphic (can reuse a desktop screenshot with branding overlay).

---

## MVP Definition

### Launch With (v1)

Minimum viable: visitors understand the product, can download it, can activate Pro, can contact you.

- [ ] Hero section with headline, subheadline, 3 CTAs (web app anchor, Google Play badge, Microsoft Store badge) — without this, no conversions
- [ ] Features showcase section (6-8 key capabilities, benefits-first language) — without this, visitors don't know what TC-01 does
- [ ] Screenshot gallery (4-6 phone mockups in frames + 1 desktop screenshot) — without this, mobile app feels unreal
- [ ] SEO meta tags + JSON-LD SoftwareApplication schema — without this, the page is invisible to search and shares badly
- [ ] Static Web App UI placeholder with Pro feature lock badges — establishes the free-vs-Pro distinction before any upsell copy
- [ ] Pro Activation section (license key input + submit + success/error feedback + localStorage unlock) — the monetization loop depends on this
- [ ] "Buy Pro License" external Lemon Squeezy link — without this, there's no path to purchase
- [ ] Contact section with Tally.so placeholder div — even a placeholder communicates accessibility

### Add After Validation (v1.x)

- [ ] Video embed — add once phone recording is finalized; video encoding/compression takes time and can block launch if made a hard dependency
- [ ] Testimonials section — add once Google Play and Microsoft Store reviews exist (trigger: first 5 reviews collected)
- [ ] JSON-LD aggregateRating — add once reviews exist to populate real data

### Future Consideration (v2+)

- [ ] Blog / content marketing — defer until there's a build pipeline; don't maintain HTML manually
- [ ] Analytics integration — defer until traffic volume justifies the cookie complexity
- [ ] Multi-product landing sections for TC-02, TC-03 — defer until those products ship

---

## Feature Prioritization Matrix

| Feature | User Value | Implementation Cost | Priority |
|---------|------------|---------------------|----------|
| Hero section + 3 CTAs | HIGH | LOW | P1 |
| Google Play + Microsoft Store badges | HIGH | LOW | P1 |
| SEO meta tags + OG tags | HIGH | LOW | P1 |
| Features showcase section | HIGH | LOW | P1 |
| Screenshot gallery with phone frames | HIGH | MEDIUM | P1 |
| Pro Activation (license key input) | HIGH | MEDIUM | P1 |
| Static Web App UI placeholder | MEDIUM | MEDIUM | P1 |
| Pro feature lock badges | MEDIUM | LOW | P1 |
| Contact section (Tally placeholder) | MEDIUM | LOW | P1 |
| JSON-LD SoftwareApplication schema | MEDIUM | LOW | P1 |
| Video embed | MEDIUM | LOW | P2 |
| Testimonials section | HIGH | LOW | P3 (blocked on reviews) |
| Blog | LOW | HIGH | P3 |

**Priority key:**
- P1: Must have for launch
- P2: Should have, add when possible
- P3: Nice to have, future consideration or blocked on external dependency

---

## Competitor Feature Analysis

| Feature | Typical Indie Music App Page | Large Music SaaS (e.g., SOUNDRAW) | TC-01 Approach |
|---------|------------------------------|-----------------------------------|----------------|
| Hero CTA | Single "Download" button | Multiple CTAs, email capture | 3 CTAs: Web App + Google Play + Microsoft Store (no email capture — keeps it simple) |
| Screenshot presentation | Raw screenshots or no screenshots | Polished device mockups, animations | Phone frame mockups (static HTML/CSS) — professional without animation complexity |
| Pro/Free distinction | Often unclear; separate pricing page | Feature comparison table | Inline Pro lock badges on the UI placeholder — visible in context |
| License activation | Usually redirects to account portal | Subscription management dashboard | Inline key input on the landing page — no separate portal needed |
| Contact | Often just an email address | Full support portal | Tally.so iframe — lightweight, no maintenance |
| SEO | Usually weak (missing og:tags, no schema) | Full schema, blog content | JSON-LD + full meta tags — achievable on static page |
| Video | Rare on indie app pages | Produced product demo videos | Phone recording embed — authentic, lower production cost |

---

## Sources

- [LandingPageFlow: CTA Placement Strategies 2026](https://www.landingpageflow.com/post/best-cta-placement-strategies-for-landing-pages) — MEDIUM confidence (WebSearch, marketing blog)
- [SaaS Hero Section Best Practices](https://www.wearetenet.com/blog/saas-hero-section-best-practices) — MEDIUM confidence (WebSearch, marketing blog)
- [Screenhance: How to Display Screenshots on SaaS Landing Pages](https://screenhance.com/blog/saas-landing-page-screenshots) — MEDIUM confidence (WebFetch verified)
- [Lemon Squeezy: Validating License Keys](https://docs.lemonsqueezy.com/guides/tutorials/license-keys) — HIGH confidence (official docs, WebFetch verified)
- [Lemon Squeezy: Activate a License Key API](https://docs.lemonsqueezy.com/api/license-api/activate-license-key) — HIGH confidence (official API docs)
- [Google Play Badge Guidelines](https://partnermarketinghub.withgoogle.com/brands/google-play/visual-identity/badge-guidelines/) — HIGH confidence (official Google docs)
- [Apple App Store Badge Guidelines](https://developer.apple.com/app-store/marketing/guidelines/) — HIGH confidence (official Apple docs)
- [618media: App Store Badge Utilization](https://618media.com/en/blog/app-store-badge-utilization/) — MEDIUM confidence (WebSearch, marketing blog)
- [Userpilot: SaaS Landing Page Best Practices](https://userpilot.com/blog/saas-landing-page-best-practices/) — MEDIUM confidence (WebSearch, verified by multiple similar sources)
- [Eleken: Input Field Design](https://www.eleken.co/blog-posts/input-field-design) — MEDIUM confidence (WebSearch)
- [Open Graph Protocol](https://ogp.me/) — HIGH confidence (official OGP spec)
- [ASO Mobile: Screenshots for App Store and Google Play 2025](https://asomobile.net/en/blog/screenshots-for-app-store-and-google-play-in-2025-a-complete-guide/) — MEDIUM confidence (WebSearch)

---

*Feature research for: Music app SaaS landing page (TC-01 Melody Generator by Kaini Instruments)*
*Researched: 2026-02-26*

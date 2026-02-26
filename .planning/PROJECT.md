# TC-01 Melody Generator Landing Page

## What This Is

A single-page static marketing website (index.html) for **TC-01 Melody Generator** by **Kaini Instruments** — a web-based melodic sequencer that generates melodies through an 8-transform control system. The page showcases the product through paired feature/image rows, drives downloads (Google Play, Microsoft Store), and upsells Free users to Pro via a Lemon Squeezy checkout link. Hosted on GitHub Pages with a custom domain.

## Core Value

Convert visitors into users — whether through the web app, Google Play, or Microsoft Store — and upsell Free users to Pro via Lemon Squeezy.

## Requirements

### Validated

- ✓ SEO-optimized metadata (title, meta description, h1/h2, OG tags, JSON-LD schema) — v1.0
- ✓ Hero section with headline, subheadline, 3 CTAs (Web App, Google Play, Microsoft Store) — v1.0
- ✓ Features showcase with 6 paired feature/image alternating rows — v1.0
- ✓ Phone screenshots and desktop screenshot display — v1.0
- ✓ Video embed (autoplay muted loop) — v1.0
- ✓ Free vs Pro comparison section with Buy Pro CTA — v1.0
- ✓ "Buy a Pro License" link to Lemon Squeezy checkout — v1.0
- ✓ Contact section with #tally-form-container placeholder — v1.0
- ✓ Tailwind CSS v4 via CDN, vanilla JS only, no build process — v1.0
- ✓ GitHub Pages compatible (CNAME, .nojekyll) — v1.0
- ✓ Charcoal grey dark palette with clean modern marketing design — v1.0
- ✓ Official Google Play and Microsoft Store badge images — v1.0
- ✓ Responsive layout (mobile, tablet, desktop) with hamburger nav — v1.0

### Active

- [ ] Replace placeholder URLs (app URL, Lemon Squeezy checkout, Google Play, Microsoft Store)
- [ ] Add brand image assets (favicon, apple-touch-icon, logos, og-image)
- [ ] Insert Tally.so embed code into contact form container
- [ ] Add demo.webm for better video compression (MP4 fallback works)

### Out of Scope

- Build tools (no npm, no Node.js, no Vite) — strictly static
- Backend/database — localStorage only
- Actual audio generation or playback — static page only
- In-app checkout cart — Lemon Squeezy external checkout is the model
- HTML contact form — Tally.so iframe will be pasted in later
- Light mode theme — app is dark-only; landing page matches
- Static web app UI placeholder — removed during v1.0 (license activation handled in-app)
- License key activation flow on landing page — handled in-app instead
- Horizontal carousel for phone screenshots — superseded by paired feature/image rows

## Context

Shipped v1.0 with 400 LOC in a single index.html file.
Tech stack: Tailwind CSS v4 (CDN), vanilla JavaScript, GitHub Pages.
Charcoal grey palette (#1a1a1a / #2a2a2a) with accent orange #f97316.
6 paired feature/image alternating rows replaced original grid + carousel approach.
8 requirements rescoped during v1.0: license activation and static app UI deferred to in-app; carousel superseded by paired rows.
11 tech debt items remain (mostly placeholder URLs and missing brand assets) — see pre-launch checklist in audit.

## Constraints

- **Tech stack**: Single index.html + Tailwind CDN + vanilla JS — no build process
- **Hosting**: GitHub Pages (static files only, no server-side)
- **Payments**: No in-app cart — Lemon Squeezy external checkout + license key activation only
- **Contact form**: Tally.so iframe (not coded — placeholder div only)

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Clean marketing design over retro-hardware | Landing page targets new users; clean design converts better | ✓ Good — charcoal palette with modern layout |
| Static web app placeholder (not functional demo) | Reduces complexity; app itself is the real demo | ✓ Good — further simplified by removing placeholder entirely |
| Feature-based Pro locking (not genre-based) | Locks advanced features like Phrase Mode, MIDI Import — clearer value prop | ✓ Good — Free vs Pro comparison section |
| Phone screenshots flat (no CSS device frames) | User preferred flat display with rounded corners | ✓ Good — simpler, cleaner |
| Official store badges (not custom buttons) | Trust and recognition from established badge designs | ✓ Good |
| Tailwind v4 via CDN (browser build) | No build tooling needed; strictly static constraint | ✓ Good — bg-linear-to-br syntax required |
| Charcoal grey over dark navy palette | Better contrast, more professional marketing feel | ✓ Good — applied in Phase 3 |
| Paired feature/image rows over grid + gallery | Shows features with visual proof side-by-side | ✓ Good — superseded carousel requirement |
| License activation in-app (not landing page) | Landing page is marketing-only; activation belongs in app | ✓ Good — simplified scope significantly |
| Lemon Squeezy external checkout (no price on page) | Price lives on checkout page; avoids sync issues | ✓ Good |

---
*Last updated: 2026-02-26 after v1.0 milestone*

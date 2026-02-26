# TC-01 Melody Generator Landing Page

## What This Is

A single-page static website (index.html) for **TC-01 Melody Generator** by **Kaini Instruments** — a web-based melodic sequencer that generates melodies through an 8-transform control system. The landing page serves as both a marketing page and a lightweight web app placeholder, hosted on GitHub Pages with a custom domain. It showcases the product, drives downloads (Google Play, Microsoft Store), and handles Pro license activation via Lemon Squeezy.

## Core Value

Convert visitors into users — whether through the web app, Google Play, or Microsoft Store — and upsell Free users to Pro via a Lemon Squeezy license key flow.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] SEO-optimized metadata (title, meta description, h1, h2 for "AI Melody Generator", "Music Maker App")
- [ ] Hero section with headline, subheadline, and 3 CTAs (Launch Web App scroll, Google Play badge, Microsoft Store badge)
- [ ] Features showcase section highlighting key capabilities (100 scales, 25 contours, 8 transforms, MIDI import, etc.)
- [ ] Screenshots/gallery section with phone mockups (phone screenshots displayed inside phone frames) and one desktop screenshot
- [ ] Video embed from phone recording
- [ ] Web App UI placeholder (static layout — dropdowns, Generate button, audio player placeholder)
- [ ] Pro badge locking on advanced features (Phrase Mode, MIDI Import, Custom Instruments, etc.)
- [ ] Pro Activation section/modal with "Activate License Key" input + Submit button
- [ ] Vanilla JS license key validation (async placeholder for Lemon Squeezy API, localStorage isPro flag, UI unlock)
- [ ] "Buy a Pro License" text link pointing to Lemon Squeezy checkout
- [ ] Contact section with `#tally-form-container` div and HTML comment placeholder for Tally.so iframe
- [ ] Tailwind CSS via CDN (no build process)
- [ ] Vanilla JavaScript only (no frameworks)
- [ ] GitHub Pages compatible (strictly static)
- [ ] Clean, modern marketing design (not retro-hardware — that's the app's aesthetic)
- [ ] Dark color palette inspired by the app but with polished marketing feel
- [ ] Official Google Play and Microsoft Store badge images

### Out of Scope

- Build tools (no npm, no Node.js, no Vite) — strictly static
- Backend/database — localStorage only
- Actual audio generation or playback in the demo — static placeholder only
- In-app checkout cart — only a license key input and external Lemon Squeezy link
- HTML contact form — Tally.so iframe will be pasted in later
- Light mode theme
- The actual TC-01 application code — this is the landing page only

## Context

- **Product:** TC-01 is the first in the "Tone Calculator" product range by Kaini Instruments. Future products (TC-02, TC-03) will be integrated into the same app, unlocked via license key upgrades.
- **Actual app tech:** React 19 + TypeScript + Vite + Tone.js (irrelevant to landing page — landing page is pure HTML/CSS/JS)
- **App aesthetic:** Dark retro-hardware/industrial (zinc-900, Audiowide font, LED glows). Landing page should be clean modern marketing but can borrow the dark palette.
- **Monetization:** SaaS loop via Lemon Squeezy to comply with Android App Store policies. Free tier with locked Pro features.
- **Media assets:** User has phone screenshots (to display in phone frame mockups), 1 desktop UI screenshot, and 1 phone video.
- **Hosting:** GitHub Pages with custom domain connection.
- **PRD location:** `C:\Users\fudge\Desktop\MyProjects\Sequencer\PRD.md` — full product spec for content reference.

## Constraints

- **Tech stack**: Single index.html + Tailwind CDN + vanilla JS — no build process
- **Hosting**: GitHub Pages (static files only, no server-side)
- **Payments**: No in-app cart — Lemon Squeezy external checkout + license key activation only
- **Contact form**: Tally.so iframe (not coded — placeholder div only)

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Clean marketing design over retro-hardware | Landing page targets new users who haven't seen the app; clean design converts better | — Pending |
| Static web app placeholder (not functional demo) | Reduces complexity; app itself is the real demo | — Pending |
| Feature-based Pro locking (not genre-based) | Locks advanced features like Phrase Mode, MIDI Import — clearer value prop | — Pending |
| Phone frame mockups for mobile screenshots | Professional presentation of mobile app screenshots | — Pending |
| Official store badges (not custom buttons) | Trust and recognition from established badge designs | — Pending |

---
*Last updated: 2026-02-26 after initialization*

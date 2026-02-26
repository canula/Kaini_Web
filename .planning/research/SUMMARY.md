# Project Research Summary

**Project:** TC-01 Melody Generator — Static Landing Page
**Domain:** Static single-page marketing site (music app / SaaS, GitHub Pages, no build process)
**Researched:** 2026-02-26
**Confidence:** MEDIUM-HIGH (stack and architecture HIGH; features and pitfalls MEDIUM)

## Executive Summary

TC-01 Melody Generator requires a static single-page marketing and activation site hosted on GitHub Pages. The project has a firm no-build-process constraint, which drives all technology decisions: Tailwind CSS is delivered via CDN (Play CDN v4), all JavaScript is vanilla ES2022+, and the entire page lives in a single `index.html`. This is a well-understood architectural pattern with a clear, prescriptive implementation path. The recommended approach is to build in 8 ordered phases following the documented dependency chain: HTML skeleton and configuration first, then static sections, then the asset-dependent gallery, then the interactive license key activation flow, and finally deployment configuration.

The key differentiator for this landing page is the inline static web app UI preview with Pro feature lock badges, paired with a license key activation modal that persists state via localStorage. This free-vs-Pro UX distinction — visible in context rather than on a separate pricing page — is the central conversion mechanic and should be treated as a first-class feature, not an afterthought. The Lemon Squeezy License API call is the single point of external integration risk: there are documented CORS issues when calling it from browser JS, and the localStorage `isPro` flag is trivially bypassable in DevTools. Both are accepted risks for a landing page where real payment enforcement lives inside the TC-01 app itself, not on this page.

The main build risks are deployment-specific: the `CNAME` file must be committed to the repo root from day one or the custom domain will be silently wiped on future pushes, and GitHub Pages HTTPS provisioning requires DNS precision against four specific GitHub IP addresses. These are low-complexity issues if addressed upfront but disproportionately disruptive if discovered late. The Tailwind CDN decision should be made explicitly in Phase 1 and documented in a code comment — the CDN is acceptable for this traffic profile, but it adds 100-300KB to first load and is officially "not for production." If Core Web Vitals matter later, the escape hatch is running Tailwind CLI once and committing the purged CSS file.

---

## Key Findings

### Recommended Stack

The stack is intentionally minimal. HTML5 provides semantic structure, Tailwind CSS v4 via CDN handles all styling with zero build tooling, and vanilla ES2022+ JavaScript covers the only two interactive requirements: localStorage state persistence and an async fetch call to the Lemon Squeezy License API. GitHub Pages provides free HTTPS hosting with custom domain support. No JavaScript framework, no bundler, no package manager, no CI/CD pipeline.

See `.planning/research/STACK.md` for exact CDN URLs, SEO meta tag block, phone mockup HTML pattern, store badge URLs, and the full license key localStorage abstraction.

**Core technologies:**
- HTML5 (semantic): Page structure, SEO semantics, single-file entry point — zero dependencies
- Tailwind CSS v4 CDN (`@tailwindcss/browser@4` via jsDelivr): Utility-first styling, JIT in-browser — eliminates build process at the cost of ~100-300KB first-load overhead
- Vanilla JavaScript (ES2022+): localStorage, async fetch, DOM manipulation — no framework overhead warranted
- GitHub Pages (free tier): Static hosting with HTTPS via Let's Encrypt, custom domain via committed `CNAME` file

**Critical version note:** Use `https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4` — do NOT use the legacy `https://cdn.tailwindcss.com` which serves Tailwind v3 and will conflict with v4 documentation.

### Expected Features

The page must achieve one goal per visitor type: (1) new visitors understand the product and can download it, (2) returning buyers can activate their Pro license without leaving the page. Everything else is secondary.

See `.planning/research/FEATURES.md` for the full prioritization matrix, feature dependency graph, competitor analysis, and MVP checklist.

**Must have (table stakes) — launch blockers:**
- Hero section with outcome-focused headline and 3 CTAs (web app anchor, Google Play badge, Microsoft Store badge)
- Features showcase section with benefits-first language (6-8 capabilities)
- Screenshot gallery with phone frame mockups (4-6 screens) — raw screenshots without frames look amateur
- SEO meta tags + JSON-LD SoftwareApplication schema — invisible to search without this
- Static Web App UI placeholder with inline Pro feature lock badges — establishes free-vs-Pro distinction in context
- Pro Activation section (license key input, submit, success/error states, localStorage unlock)
- "Buy Pro License" external Lemon Squeezy checkout link
- Contact section with Tally.so iframe placeholder

**Should have (competitive differentiators):**
- Phone recording video embed — music apps require motion to convey their feel; add once video is finalized post-launch
- Dark marketing aesthetic continuous with the app's design language — visual brand continuity reduces bounce at app handoff

**Defer (v2+):**
- Testimonials — blocked until real Google Play / Microsoft Store reviews exist; placeholder looks worse than nothing
- Blog / content marketing — requires a build pipeline to maintain; do not write HTML manually
- Analytics — no obligation to add a cookie banner until analytics are introduced; defer to reduce complexity
- Multi-product sections for TC-02/TC-03 — defer until those products ship

### Architecture Approach

The page is a flat single-file DOM with six semantic sections in a conversion-optimized order (hero → features → gallery → demo → pricing → contact), backed by three JavaScript modules implemented inline: a `licenseState` module that owns all localStorage reads/writes and the Lemon Squeezy API call, a modal controller using the native `<dialog>` element, and a page-init function that fires once on `DOMContentLoaded`. All section-to-section navigation is CSS anchor links with `scroll-behavior: smooth` on `<html>` — zero JS needed. Pro lock/unlock state is communicated through `data-pro-lock` and `data-pro-badge` HTML attributes, decoupling the license module from section markup.

See `.planning/research/ARCHITECTURE.md` for the full system diagram, data flow diagrams, recommended build order, all anti-patterns, and external integration points.

**Major components:**
1. `#hero` — First impression, 3 CTAs; full-viewport section, headline + store badge row
2. `#features` — Feature grid with CSS Pro badge overlays on locked items; creates desire before showing price
3. `#gallery` — Phone mockup carousel (CSS frames, no external library); snap-x scrollable on mobile
4. `#demo` — Static web app UI placeholder; Pro locks appear here in context
5. `#pricing` — Free/Pro card layout, Lemon Squeezy buy link, license modal trigger
6. `#contact` — Tally.so div placeholder with HTML comment
7. `licenseState` module — `readLicenseState()`, `saveLicenseState()`, `activateLicenseKey()`, `applyProUI()`; wraps all localStorage access in try/catch for private-browsing safety
8. `<dialog id="license-modal">` — Native browser dialog; handles focus trapping, backdrop, and Escape key dismissal without custom JS

### Critical Pitfalls

1. **Tailwind CDN in production** — The Play CDN is officially "not for production." For this project, accept the tradeoff consciously and document it with a code comment at the CDN script tag. If Lighthouse flags render-blocking or LCP degrades on 3G, the escape hatch is: run `npx tailwindcss` once to generate a purged CSS file and commit it. Do not discover this retroactively after writing hundreds of utility classes.

2. **CNAME file missing from repo root** — If the `CNAME` file is not committed alongside `index.html`, any future push that replaces files will wipe the custom domain setting. GitHub will silently revert the site to the `username.github.io` URL. Fix: commit `CNAME` containing the domain on a single line in Phase 1, before writing any other content.

3. **GitHub Pages HTTPS certificate never issues** — Caused by conflicting SSL from the domain registrar, wrong A record IPs, or CNAME pointing to apex instead of `username.github.io`. Fix: use exactly the four GitHub IPs, point `www` CNAME to `yourusername.github.io`, disable registrar-level SSL, and wait 24 hours before diagnosing. Remove and re-add the custom domain in GitHub Settings to force re-provisioning if needed.

4. **localStorage `isPro` is bypassable** — Any user can set `localStorage.setItem('tc01_is_pro', 'true')` in DevTools. This is acceptable: the landing page Pro lock is decorative UX friction, not a security gate. Real enforcement is in the Play Store and Microsoft Store billing. Document this explicitly in a code comment so future maintainers do not treat it as a security boundary. Additionally: validate `meta.store_id` and `meta.product_id` in the Lemon Squeezy API response to prevent cross-product license key abuse.

5. **JS global scope pollution and event listener accumulation** — Without a module bundler, all JS runs in global scope. Event listeners re-attached on each modal open will fire multiple times per click. Fix: wrap all JS in a single `DOMContentLoaded` block, attach all listeners once at init, never re-attach on modal open. Use `<script type="module">` if ES module scoping is preferred.

---

## Implications for Roadmap

Based on the architecture's documented build order dependency chain, combined with pitfall phase mappings, a clear 8-phase structure emerges. Phases 1-2 are configuration and must come before any content work. Phases 3-5 are static HTML/CSS and can be validated visually before adding interactivity. Phase 6 depends on data-attribute markup laid down in Phase 5. Phases 7-8 are cleanup and deployment.

### Phase 1: Foundation and Configuration
**Rationale:** Every subsequent phase targets DOM IDs, CDN assets, and file paths. These must exist and be correct before writing any content. The two most common deployment disasters (CNAME wipe, Jekyll processing) are fixed here at zero cost.
**Delivers:** `index.html` with complete `<head>` block (SEO meta tags, OG tags, JSON-LD schema, Tailwind CDN script), `CNAME` file, `.nojekyll` file, `assets/` directory structure, `<body>` containing empty section stubs with correct IDs (`#hero`, `#features`, `#gallery`, `#demo`, `#pricing`, `#contact`), and sticky nav with anchor links.
**Addresses:** Hero skeleton, SEO meta tags, JSON-LD SoftwareApplication schema
**Avoids:** CNAME wipe pitfall, HTTPS provisioning pitfall, SEO failures pitfall, Tailwind CDN decision made explicitly with code comment

### Phase 2: Static Sections (Hero + Features)
**Rationale:** No interactivity required. Validates the dark aesthetic, typography, and responsive layout before adding complexity. These sections are the highest conversion impact but lowest implementation complexity.
**Delivers:** Fully styled `#hero` section (headline, subheadline, 3 CTAs with official store badges), `#features` grid with Pro badge lock overlays in place, dark zinc palette established, responsive stacking on mobile.
**Uses:** Tailwind v4 arbitrary values, CSS phone mockup patterns, official Google Play and Microsoft Store badge image URLs
**Avoids:** Badge policy violations pitfall (use official URLs from research); badge minimum width 135px

### Phase 3: Gallery Section
**Rationale:** Asset-dependent phase — screenshot files must exist before this phase begins. CSS phone frames and snap-scroll gallery pattern are well-documented but require mobile testing across four viewport widths (375px, 390px, 768px, 1280px).
**Delivers:** `#gallery` section with 4-6 phone mockup frames in a snap-x scrollable row, screenshots clipped correctly inside frames, responsive behavior validated.
**Implements:** Phone mockup CSS pattern (Tailwind arbitrary border/radius values), snap-x scroll gallery
**Avoids:** Phone mockup responsive breakage pitfall; unoptimized image pitfall (WebP, <100KB per image)

### Phase 4: Static Web App UI Demo Section
**Rationale:** The `#demo` section's Pro lock badges must be built before the license JS in Phase 5, because Phase 5's `applyProUI()` targets `data-pro-lock` and `data-pro-badge` attributes that this phase creates. Build the surface, then wire the behavior.
**Delivers:** `#demo` section with static faux-app UI (non-functional dropdowns, Generate button, audio player stub), Pro lock overlays with `data-pro-lock` and `data-pro-badge` attributes on appropriate elements, visual Pro/free distinction clear to a first-time visitor.
**Addresses:** Static Web App UI placeholder feature, Pro feature lock badges feature

### Phase 5: License Key Activation (Interactivity + JS)
**Rationale:** All `data-pro-lock` attributes from Phase 4 must exist before this phase can be tested. This phase introduces the only JavaScript in the project and the only external API dependency.
**Delivers:** `<dialog id="license-modal">` with key input, submit button, loading/success/error feedback states; `licenseState` JS module (readLicenseState, saveLicenseState, activateLicenseKey with CORS-aware error handling, applyProUI); `DOMContentLoaded` init function; localStorage `isPro` state restoration on page load.
**Uses:** Native `<dialog>` element, `async/await` fetch to Lemon Squeezy License API, localStorage
**Avoids:** Global scope pollution pitfall (IIFE or module), event listener accumulation pitfall (attach once at init), security: validate `meta.store_id` and `meta.product_id` in API response; document that localStorage is bypassable by design

### Phase 6: Pricing Section
**Rationale:** Depends on the modal trigger from Phase 5 working correctly. The "Activate License" button in this section calls `dialog.showModal()` — it cannot be properly tested until the modal and license JS exist.
**Delivers:** `#pricing` section with free/pro card layout, external Lemon Squeezy checkout link, "Activate License" button wired to modal, visual hierarchy that converts while desire from gallery and demo is still high.
**Addresses:** "Buy Pro License" external link feature

### Phase 7: Contact Section
**Rationale:** No dependencies on any prior phase — a `<div>` placeholder with an HTML comment. Placed last because it is trivial and has zero risk.
**Delivers:** `#contact` section with `div#tally-form-container` and an HTML comment explaining the Tally.so iframe paste format.
**Addresses:** Contact section feature

### Phase 8: GitHub Pages Deployment
**Rationale:** Deploy after all content is complete. DNS provisioning can run in parallel with content work once the `CNAME` file is committed (Phase 1). This phase is about verification and enabling HTTPS enforcement.
**Delivers:** GitHub Pages configured with custom domain, HTTPS enforced, DNS A records verified against GitHub IPs, `.nojekyll` confirmed, Lighthouse SEO score >90, store badge links verified to deep-link to specific app listings (not platform home pages).
**Avoids:** HTTPS certificate never issues pitfall (registrar SSL disabled, correct A records, 24h patience)

### Phase Ordering Rationale

- Configuration files (`CNAME`, `.nojekyll`, `<head>` SEO block) are in Phase 1 because they are zero-cost to add at the start and high-cost to discover missing at the end.
- Static sections (Phases 2-4) come before interactive JS (Phase 5) because the JS targets DOM attributes that must exist before event listeners can be wired.
- Gallery (Phase 3) is separated from hero/features (Phase 2) because it has a content dependency on screenshot asset files that may not be available on the same day as development begins.
- Pricing section (Phase 6) is placed after license JS (Phase 5) because its primary CTA triggers the modal, and clicking a non-functional trigger during development creates misleading test results.
- Contact (Phase 7) has no dependencies and is last because it is a one-line placeholder with no risk.

### Research Flags

Phases likely needing validation during implementation:
- **Phase 5 (License Key Activation):** The Lemon Squeezy CORS behavior is the only MEDIUM-confidence finding in the research. STACK.md and FEATURES.md give conflicting signals — STACK.md cites evidence of CORS errors in browser JS, while FEATURES.md notes the API "accepts browser-originated requests." Test the actual API call from the static page before finalizing. If CORS blocks the request in production, the fallback is a Cloudflare Worker as a thin proxy (zero backend maintenance, free tier) — the frontend localStorage interface does not change.
- **Phase 8 (Deployment):** GitHub Pages HTTPS provisioning is well-documented to have edge cases with certain domain registrars (Namecheap, Strato auto-provision SSL that conflicts). Verify registrar-level SSL is disabled before enabling GitHub Pages HTTPS.

Phases with standard patterns (deep research not needed):
- **Phase 1 (Foundation):** SEO meta tag patterns and GitHub Pages configuration are well-established with official documentation. The exact head block is in STACK.md — copy it directly.
- **Phase 2 (Hero + Features):** Tailwind utility class patterns and store badge URLs are fully documented in STACK.md with exact CDN paths.
- **Phase 3 (Gallery):** Flowbite phone mockup pattern is documented in ARCHITECTURE.md with full HTML. The snap-x scroll gallery is a standard Tailwind pattern.
- **Phase 7 (Contact):** No implementation required — a `<div>` and an HTML comment.

---

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Core technologies verified against official Tailwind, GitHub, MDN, and Lemon Squeezy documentation. Tailwind CDN URL and video autoplay pattern are confirmed. CORS behavior for Lemon Squeezy is the one MEDIUM-confidence item. |
| Features | MEDIUM | Feature prioritization is derived from marketing blog research (multiple sources agree) and official badge guidelines. No single authoritative spec for this exact domain combination. MVP scope is clear and well-reasoned. |
| Architecture | HIGH | Section ordering, data flow, and component boundaries all follow documented conversion optimization research. localStorage pattern and `<dialog>` usage are verified against MDN official documentation. Build order dependency chain is internally consistent. |
| Pitfalls | MEDIUM | Most pitfalls verified against official docs (GitHub, Tailwind, Google Play, Microsoft) or community discussions with multiple corroborating reports. Lemon Squeezy CORS behavior has one primary source (Figma community forum). |

**Overall confidence:** MEDIUM-HIGH

### Gaps to Address

- **Lemon Squeezy CORS in production:** Research found conflicting signals about whether the License API accepts browser-originated requests. Test this empirically in Phase 5 before writing the final activation flow. If blocked: implement a Cloudflare Worker proxy as documented in STACK.md (no frontend changes required).
- **OG image asset:** SEO requires a real 1200x630px `og:image` file. This is a content dependency, not a code dependency. The image should be created before Phase 1 completes (or at minimum before the site is shared anywhere). A desktop screenshot with a branding overlay is sufficient.
- **Screenshot assets:** Phase 3 (Gallery) is blocked until 4-6 phone screenshot files exist and have been compressed to WebP at <100KB each. Coordinate asset preparation in parallel with Phases 1-2.
- **Lemon Squeezy product IDs:** Phase 5 requires the actual `store_id` and `product_id` values from the Lemon Squeezy product setup to validate API responses correctly. These must be hardcoded in the JS. Verify these values exist before Phase 5 begins.
- **App store listing URLs:** Phase 2 requires the exact Google Play package name (`id=YOUR_PACKAGE_NAME`) and Microsoft Store app ID for the badge deep links. Confirm these before committing the hero section.

---

## Sources

### Primary (HIGH confidence)
- `https://tailwindcss.com/docs/installation/play-cdn` — Tailwind v4 Play CDN script tag, production warning
- `https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site` — GitHub Pages DNS setup, HTTPS enforcement
- `https://docs.lemonsqueezy.com/api/license-api/activate-license-key` — License API endpoint, request/response format
- `https://docs.lemonsqueezy.com/guides/tutorials/license-keys` — License key validation guide
- `https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API` — localStorage patterns
- `https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay` — Video autoplay policy (muted/playsinline)
- `https://developer.x.com/en/docs/x-for-websites/cards/guides/getting-started` — Twitter Card meta tag requirements
- `https://ogp.me/` — Open Graph Protocol specification
- `https://partnermarketinghub.withgoogle.com/brands/google-play/visual-identity/badge-guidelines/` — Google Play badge requirements
- `https://learn.microsoft.com/en-us/windows/apps/publish/microsoft-store-app-badge` — Microsoft Store badge requirements
- `https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages` — HTTPS provisioning troubleshooting

### Secondary (MEDIUM confidence)
- `https://flowbite.com/docs/components/device-mockups/` — Tailwind phone mockup CSS pattern
- `https://brandedagency.com/blog/the-anatomy-of-a-high-converting-landing-page-14-powerful-elements-you-must-use-in-2026` — Section ordering and CTA placement
- `https://userpilot.com/blog/saas-landing-page-best-practices/` — SaaS landing page conventions
- `https://screenhance.com/blog/saas-landing-page-screenshots` — Screenshot presentation best practices
- `https://github.com/orgs/community/discussions/48422` — GitHub Pages CNAME wipe on deploy (community-verified)
- `https://github.com/orgs/community/discussions/44160` — GitHub Pages HTTPS provisioning issues (community-verified)
- `https://forum.figma.com/ask-the-community-7/cors-issues-with-create-figma-plugin-using-lemonsqueezy-for-license-management-37919` — Lemon Squeezy CORS evidence

---
*Research completed: 2026-02-26*
*Ready for roadmap: yes*

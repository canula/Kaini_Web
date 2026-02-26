# Pitfalls Research

**Domain:** Static landing page — music app marketing (GitHub Pages, Tailwind CDN, vanilla JS, Lemon Squeezy)
**Researched:** 2026-02-26
**Confidence:** MEDIUM (WebSearch verified against official docs where possible)

---

## Critical Pitfalls

### Pitfall 1: Tailwind Play CDN Is Explicitly Not for Production

**What goes wrong:**
The Tailwind Play CDN (`@tailwindcss/browser`) performs JIT compilation in the browser at runtime. Every visitor's device downloads a large JavaScript bundle and burns CPU cycles compiling CSS that could have been prebuilt static bytes. The official Tailwind docs explicitly state: "The Play CDN is designed for development purposes only, and is not intended for production." The v3 CDN was ~3MB uncompressed. The v4 browser bundle is similarly heavy.

**Why it happens:**
The project deliberately prohibits a build process (no npm, no Node.js, no Vite). This constraint makes the CDN the only apparent option. Developers reach for it without recognizing that "no build process" and "production-ready Tailwind" are incompatible goals unless you pre-generate the CSS file once via CLI and commit it.

**How to avoid:**
Two acceptable paths:
1. Use Tailwind CLI locally once to generate a purged/minified CSS file, commit that file, and serve it statically. No runtime build process needed, no npm in deployment.
2. Accept the CDN for this landing page specifically because it is a single static page with no ongoing CI/CD pipeline — the tradeoff (heavier page load) is acceptable for a low-traffic marketing page, but document this decision explicitly so it is not inherited by future pages.

If the CDN is chosen, add `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>` and understand the page will be 100-300KB heavier on first load than an optimized build.

**Warning signs:**
- PageSpeed Insights shows CSS/JS blocking render from cdn.jsdelivr.net
- Lighthouse flags "Eliminate render-blocking resources" for the Tailwind CDN script
- Page takes >2s to show styled content on slow 3G

**Phase to address:**
Phase 1 (Project setup / HTML skeleton) — decide CDN vs. committed CSS file before writing any markup. Retroactively changing this after hundreds of utility classes are written is feasible but tedious.

---

### Pitfall 2: localStorage isPro Flag Is Trivially Bypassable

**What goes wrong:**
Storing `isPro: true` in localStorage and reading it to unlock UI features provides zero security. Any user can open DevTools console and type `localStorage.setItem('isPro', 'true')` to unlock Pro features without purchasing. The UI unlocking is purely cosmetic security theater.

**Why it happens:**
The project spec deliberately calls for client-side only validation (no backend). The intent is SaaS loop compliance with Android App Store policies, not hard DRM. Developers mistake "license key input flow" for "security."

**How to avoid:**
Accept the limitation explicitly. The Lemon Squeezy validation API (`POST https://api.lemonsqueezy.com/v1/licenses/validate`) must be called from the browser (no server), which means the license key itself travels over a network request visible in DevTools. The `isPro` localStorage flag is just a convenience persistence layer after a successful API call.

The mitigation is: do not gate anything truly valuable behind this lock. The web app on this landing page is a static placeholder. The real app (Google Play, Microsoft Store) has its own payment enforcement. The landing page Pro lock is a demonstration of the Pro tier's existence, not hard enforcement. Document this in code comments so future maintainers do not treat it as a security boundary.

Additionally: validate the Lemon Squeezy response fields server-side if security ever matters. Specifically, validate `store_id` and `product_id` in the response to prevent someone using a valid license key from a different Lemon Squeezy product to unlock your app.

**Warning signs:**
- Code has a comment like "// TODO: add real validation later" without a plan for what "real" means
- The `isPro` check gates features in the actual TC-01 web app (not just this landing page placeholder)
- No check of `store_id`/`product_id` in the Lemon Squeezy API response

**Phase to address:**
Phase with license key UI implementation. Add a code comment block at the top of the JS license validation function documenting the security model explicitly.

---

### Pitfall 3: GitHub Pages Custom Domain Wiped on Every Deployment

**What goes wrong:**
If the site is ever redeployed by pushing files directly (e.g., via a script or GitHub Actions that rebuilds the branch), the `CNAME` file is deleted and the custom domain setting in GitHub Pages is cleared. The site reverts to `username.github.io/repo-name` with broken internal links.

**Why it happens:**
GitHub Pages stores the custom domain in a `CNAME` file in the root of the deployed branch. Deployment workflows that do force-pushes or clean-branch replacements overwrite this file. This is a well-documented and persistent issue in the GitHub community (multiple active discussions as of 2025-2026).

**How to avoid:**
Include a `CNAME` file in the repo root containing only the custom domain (e.g., `kainidashboard.com`) as a committed file alongside `index.html`. Since this project deploys by pushing `index.html` directly to `main` (no build process), the `CNAME` file stays in place automatically. This is only a risk if a deployment script is added later.

**Warning signs:**
- Site works on `username.github.io/repo` but not on custom domain
- GitHub Pages settings show "Custom domain" field is empty after a push
- Browser shows the default GitHub 404 page on the custom domain

**Phase to address:**
Phase 1 (Project setup / repo initialization) — commit `CNAME` file immediately alongside `index.html`.

---

### Pitfall 4: HTTPS Certificate Not Provisioned for Custom Domain

**What goes wrong:**
GitHub Pages provisions a Let's Encrypt TLS certificate automatically, but the "Enforce HTTPS" checkbox remains grayed out for hours or permanently if DNS is misconfigured. Multiple GitHub community threads (active through 2026) show users stuck with DNS check passed but certificate never issued.

**Why it happens:**
Common causes: (1) apex domain points to wrong A records (must be GitHub's four IPs: 185.199.108-111.153); (2) www CNAME points to apex domain instead of `username.github.io`; (3) conflicting SSL certificate from DNS provider (some registrars like Namecheap, Strato auto-provision SSL that blocks GitHub's certificate request); (4) domain name exceeds 64 characters (RFC3280 limit for certificate common name).

**How to avoid:**
- Set apex A records to exactly: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Set `www` CNAME to `username.github.io` (not to the apex domain)
- Disable any "free SSL" feature in the domain registrar before enabling GitHub Pages HTTPS
- After DNS propagation, wait up to 24h before concluding something is broken
- If certificate never issues: remove the custom domain in GitHub Pages settings, save, wait 5 minutes, re-enter it

**Warning signs:**
- "Enforce HTTPS" checkbox is grayed out
- GitHub Pages settings shows "Your site is published at http://" (no S)
- Browser shows "Not Secure" on the custom domain

**Phase to address:**
Phase with deployment / hosting setup.

---

## Moderate Pitfalls

### Pitfall 5: Google Play and Microsoft Store Badge Policy Violations

**What goes wrong:**
Using custom-styled download buttons instead of the official badges, or using the official badge images in ways that violate the respective brand guidelines (resizing incorrectly, altering colors, using wrong localized version, using old/deprecated badge versions).

**Why it happens:**
Developers find badge images via Google Image Search or random blog posts and use whatever PNG they find. Older Google Play badges ("Available on Google Play") have been replaced with "Get it on Google Play." Microsoft has updated badge designs multiple times.

**How to avoid:**

**Google Play:**
- Download badges only from the official Google Play Partner Marketing Hub: `https://partnermarketinghub.withgoogle.com/brands/google-play/visual-identity/visual-identity/`
- Use the horizontal lockup (preferred for most marketing contexts)
- Use localized badge matching page language
- Maintain clear space equal to 1/4 of badge height around the badge
- Never resize below legibility threshold
- Never alter badge colors or rearrange elements

**Microsoft Store:**
- Use the official badge generator at `https://apps.microsoft.com/badge`
- Alternatively download from Microsoft Learn: `https://learn.microsoft.com/en-us/windows/apps/publish/microsoft-store-app-badge`
- Microsoft provides a web component (`<ms-store-badge>`) for embedding
- Add a campaign ID to badge links to track traffic in Partner Center
- Never alter badge colors or text

**Warning signs:**
- Badge images sourced from a third-party website or blog post
- Badge says "Available on Google Play" instead of "Get it on Google Play"
- Badge images are pixelated or blurry (low-resolution source)
- Badges are different heights when placed side by side

**Phase to address:**
Phase with hero section / CTA implementation.

---

### Pitfall 6: SEO Failures on a Single-Page Static Site

**What goes wrong:**
Missing or duplicate title tags, missing meta descriptions, no Open Graph tags, no structured data (JSON-LD), missing canonical tag, h1 used multiple times or not used at all, and missing `alt` attributes on images. For a music app landing page targeting "AI melody generator" and "music maker app," these omissions directly reduce search ranking.

**Why it happens:**
Single-page sites feel simple, so SEO is treated as an afterthought. Structured data is often skipped because it is invisible. Open Graph tags are skipped because they only appear in social previews.

**How to avoid:**
- Title: unique, under 60 characters, includes primary keyword ("TC-01 Melody Generator — AI Music Sequencer")
- Meta description: 150-160 characters, includes secondary keywords, written as human-readable sentence
- Canonical: `<link rel="canonical" href="https://yourdomain.com/">` — prevents duplicate content if GitHub Pages URL coexists with custom domain
- Open Graph: `og:title`, `og:description`, `og:image` (1200x630px), `og:url`
- Structured data: JSON-LD `SoftwareApplication` schema for the app listing
- h1: exactly one, includes "TC-01 Melody Generator" or primary keyword
- All `<img>` tags: descriptive `alt` attributes
- All phone mockup images: `alt="TC-01 melody generator mobile app screenshot"` pattern

**Warning signs:**
- Lighthouse SEO score below 90
- Google Search Console shows "Page cannot be indexed" errors
- Social media share shows blank preview image (missing og:image)
- Browser tab shows default "index.html" title

**Phase to address:**
Phase 1 (HTML skeleton) — embed SEO metadata in `<head>` before writing any content.

---

### Pitfall 7: Phone Mockup Device Frames Break on Real Devices

**What goes wrong:**
CSS phone frames built with fixed pixel dimensions or absolute positioning look correct on the developer's desktop at a specific viewport width but overflow, crop, or collapse on actual mobile viewports. Screenshots inside the frame are either too small (letterboxed) or clip outside the frame border.

**Why it happens:**
Phone mockup CSS relies on precise aspect ratios and nested dimensions. When using Tailwind utility classes, getting the frame-to-screenshot ratio right requires careful use of relative units (`aspect-ratio`, percentage heights, `object-fit: contain`). Developers test only on desktop Chrome.

**How to avoid:**
- Use `aspect-ratio` CSS property on the outer frame container, not fixed `height`
- Use `object-fit: cover` or `object-fit: contain` on the screenshot image inside the frame
- Use Flowbite's Tailwind device mockup component as a reference: `https://flowbite.com/docs/components/device-mockups/` — it handles responsive sizing
- Test at 375px (iPhone SE), 390px (iPhone 14), 430px (iPhone 14 Pro Max), and 768px (tablet) viewports
- On mobile viewports, stack mockups vertically rather than displaying them side by side

**Warning signs:**
- Screenshot image extends beyond the phone frame border
- Phone frame collapses to zero height on mobile
- Frame looks correct at 1440px desktop but breaks at 768px

**Phase to address:**
Phase with screenshots/gallery section.

---

### Pitfall 8: Vanilla JS Global Scope Pollution and Event Listener Leaks

**What goes wrong:**
All JavaScript in a single `index.html` file ends up in the global scope. Functions, variables, and state attached to `window` conflict with browser globals or with each other. Event listeners added inside conditional blocks or modal open/close handlers accumulate on each re-open, causing doubled/tripled behavior.

**Why it happens:**
Without a module bundler, there is no natural encapsulation boundary. Developers write functions directly at the top level. The license activation modal is opened and closed multiple times per session, and each open may re-attach `click` event listeners to the submit button.

**How to avoid:**
- Wrap all JS in an IIFE (`(function() { ... })()`) or a single `DOMContentLoaded` block
- Alternatively, use ES modules (`<script type="module">`) which are automatically scoped
- For the license modal: attach event listeners once on `DOMContentLoaded`, not on each modal open
- Use `removeEventListener` before `addEventListener` if listeners must be re-attached
- Use `const` and `let` exclusively — never `var`
- Name all functions descriptively; avoid anonymous functions stored on `window`

**Warning signs:**
- Submitting the license key form twice triggers two API calls
- Console shows functions or variables defined on `window` that you didn't put there
- `click` handler fires multiple times per click after modal is opened and closed

**Phase to address:**
Phase with JS interactivity / license key activation.

---

## Technical Debt Patterns

| Shortcut | Immediate Benefit | Long-term Cost | When Acceptable |
|----------|-------------------|----------------|-----------------|
| Tailwind CDN (no build) | Zero setup, matches project constraints | 100-300KB runtime JS bundle; JIT compile on every page load; "not for production" per official docs | Acceptable for this project given explicit no-build constraint — document the decision |
| `localStorage` isPro flag | Trivially simple persistence | Bypassable by any user in DevTools; not a security boundary | Acceptable for this landing page — the real payment enforcement lives in Play Store / App Store |
| Inline `<script>` in index.html | Single file deployment | Hard to test, no type checking, no linting | Acceptable for MVP; becomes painful if JS grows beyond ~150 lines |
| No 404.html custom page | Nothing to build | GitHub's default 404 page shows on bad URLs (not branded) | Acceptable — this is a single-page site with no internal routing |

---

## Integration Gotchas

| Integration | Common Mistake | Correct Approach |
|-------------|----------------|------------------|
| Lemon Squeezy License API | Calling API without checking `store_id` and `product_id` in response | Always validate that the response `meta.store_id` and `meta.product_id` match your product — prevents cross-product license key abuse |
| Lemon Squeezy License API | Treating a `valid: true` response as permanent — not re-validating | For a landing page placeholder this is low risk, but note that licenses can be revoked; the flag stored in localStorage will remain `true` indefinitely |
| GitHub Pages CNAME | Setting custom domain in UI without a committed CNAME file | Commit a `CNAME` file to repo root containing only the domain; do not rely on GitHub UI alone |
| GitHub Pages HTTPS | Trying to enforce HTTPS before DNS fully propagates | Wait 24h after DNS changes before assuming HTTPS provisioning failed |
| Google Play badge | Linking badge to Play Store home instead of specific app listing | Badge `href` must deep-link to your specific app: `https://play.google.com/store/apps/details?id=YOUR_PACKAGE_NAME` |
| Microsoft Store badge | Using a generic Store URL | Badge `href` must deep-link: `https://apps.microsoft.com/store/detail/YOUR_APP_ID` |
| Tally.so iframe | Embedding iframe before Tally script loads | The placeholder `div#tally-form-container` approach in the spec is correct — add HTML comment noting Tally's embed snippet format so future maintainer knows what to paste |

---

## Performance Traps

| Trap | Symptoms | Prevention | When It Breaks |
|------|----------|------------|----------------|
| Large uncompressed phone screenshot images | LCP >4s; PageSpeed flags image optimization | Export screenshots at 2x retina (750px wide for phone frames) as WebP; compress to <100KB each | Day 1 if images are exported at full resolution from phone |
| Unoptimized video embed (raw .mp4) | Page load stall; video blocks rendering | Use `<video>` with `preload="none"` and `poster` image; or host on YouTube and embed via iframe | Day 1 if raw video file is committed to repo |
| Tailwind CDN blocking first paint | Unstyled content flash (FOUC) on slow connections | Accept the CDN tradeoff or switch to committed CSS file; at minimum add `<noscript>` fallback | First page load on 3G connections |
| Font loading flash | Text renders in system font then jumps to brand font | Use `font-display: swap` in `@font-face`; preload critical fonts with `<link rel="preload">` | First load on any connection speed |

---

## Security Mistakes

| Mistake | Risk | Prevention |
|---------|------|------------|
| Storing license key itself in localStorage (not just the `isPro` flag) | License key is visible to any script on the page; XSS can exfiltrate it | Store only the `isPro: true` boolean; never persist the raw license key string after validation |
| Calling Lemon Squeezy API without validating `store_id`/`product_id` in response | A valid license key from any other Lemon Squeezy seller unlocks your Pro tier | Read `meta.store_id` and `meta.product_id` from the API response and compare to hardcoded expected values |
| Mixed HTTP/HTTPS content | Browser blocks resources; security warning in address bar | Use only `https://` URLs throughout index.html for all CDN links, API calls, and badge images |
| Inline onclick handlers | Hard to audit; pollutes global scope | Use `addEventListener` in the script block, not `onclick=""` attributes in HTML |

---

## UX Pitfalls

| Pitfall | User Impact | Better Approach |
|---------|-------------|-----------------|
| License activation with no loading state | User clicks "Activate" and sees nothing for 2+ seconds while API call completes; clicks again, sends duplicate requests | Show spinner or "Validating..." text immediately on submit; disable the button during the async call |
| License activation error message is generic | User types a wrong key and sees "Invalid license" with no guidance | Include error message: "License key not found. Check your Lemon Squeezy purchase email." with link to Lemon Squeezy support |
| Pro badge locks are confusing without tooltip | Users see a lock icon but don't know what Pro is or how to get it | Each locked feature badge should be clickable, scrolling to the Pro Activation section or linking to Lemon Squeezy checkout |
| Video autoplay on mobile | iOS and Android block autoplay of unmuted video; broken video element | Use `autoplay muted playsinline` attributes together; test on actual mobile device |
| Store badges sized smaller than readable | Badge text is illegible at <120px wide | Minimum badge width: 135px (Google Play guideline); use `min-width` not just responsive sizing |

---

## "Looks Done But Isn't" Checklist

- [ ] **SEO:** `<title>` tag contains the primary keyword ("AI Melody Generator" or "TC-01") — verify it is not left as "Landing Page" or empty
- [ ] **SEO:** `og:image` points to a real 1200x630 image that exists in the repo — not a placeholder path
- [ ] **Canonical:** `<link rel="canonical">` points to `https://` custom domain, not the github.io URL
- [ ] **CNAME:** File exists at repo root with exactly the custom domain on a single line, no trailing newline issues
- [ ] **HTTPS:** GitHub Pages "Enforce HTTPS" checkbox is checked, not just "Your site is published at https://"
- [ ] **Google Play badge:** Links to specific app listing, not to `play.google.com` home
- [ ] **Microsoft Store badge:** Links to specific app listing, not to `apps.microsoft.com` home
- [ ] **License validation:** Lemon Squeezy API response `store_id` and `product_id` are checked against hardcoded values
- [ ] **Phone mockups:** Screenshots render inside the frame at all viewports (375px, 390px, 768px, 1280px)
- [ ] **Video:** `poster` attribute set so video area is not black while loading
- [ ] **Tally placeholder:** `#tally-form-container` div exists with HTML comment explaining what to paste

---

## Recovery Strategies

| Pitfall | Recovery Cost | Recovery Steps |
|---------|---------------|----------------|
| Tailwind CDN in production (decided to switch) | MEDIUM | Run `npx tailwindcss -i ./input.css -o ./styles.css --minify` once; replace CDN script tag with `<link rel="stylesheet" href="styles.css">`; commit the generated file |
| Custom domain wiped from GitHub Pages | LOW | Re-enter domain in GitHub Pages settings; ensure `CNAME` file is committed to repo root |
| HTTPS certificate not provisioning | MEDIUM | Check DNS records against GitHub's required IPs; disable registrar SSL; wait 24h; try removing and re-adding custom domain in GitHub Pages settings |
| localStorage Pro bypass discovered | LOW (for landing page) | No code change required — the landing page Pro lock is decorative; actual enforcement is in Play Store/App Store billing |
| Wrong store badges committed | LOW | Replace image files; the URLs for official badges are stable |
| Phone mockups broken on mobile | MEDIUM | Refactor mockup CSS to use `aspect-ratio` + `object-fit`; requires visual testing across viewport widths |

---

## Pitfall-to-Phase Mapping

| Pitfall | Prevention Phase | Verification |
|---------|------------------|--------------|
| Tailwind CDN decision | Phase 1: Project setup | Document CDN choice explicitly in code comment; check Lighthouse performance score |
| CNAME file missing | Phase 1: Project setup | `ls` repo root includes `CNAME` file before any other work begins |
| GitHub Pages HTTPS setup | Deployment phase | GitHub Pages settings shows "Enforce HTTPS" checked and green |
| SEO metadata missing | Phase 1: HTML skeleton | Validate with `<meta>` tag checker; Lighthouse SEO > 90 |
| Canonical tag missing | Phase 1: HTML skeleton | View page source; confirm `<link rel="canonical">` present |
| Official store badges | Hero / CTA phase | Badges downloaded from official sources (links in this doc); visual inspection at 1x and 2x |
| Phone mockup responsive | Screenshots/gallery phase | Test at 375px, 768px, 1280px viewports; screenshots are contained within frames |
| localStorage security | License key phase | Code comment documents security model; raw key not persisted; `store_id` validated |
| Lemon Squeezy API response validation | License key phase | Code checks `meta.store_id` === expected value before setting isPro |
| JS global scope pollution | License key / interactivity phase | All JS wrapped in IIFE or `DOMContentLoaded`; no globals except intentional |
| Loading state on license submit | License key phase | Manual test: submit form and verify button is disabled during API call |
| Video/image optimization | Content / media phase | Lighthouse flags no image/video size warnings; images < 100KB each |

---

## Sources

- Tailwind CSS Play CDN official docs: https://tailwindcss.com/docs/installation/play-cdn (HIGH confidence — official)
- Tailwind CDN file size issue (v3 ~2.9MB): https://github.com/tailwindlabs/tailwindcss/issues/5573 (MEDIUM confidence — community-verified)
- GitHub Pages HTTPS provisioning problems: https://github.com/orgs/community/discussions/44160 and https://github.com/orgs/community/discussions/184514 (MEDIUM confidence — multiple active community threads)
- GitHub Pages custom domain wiped on deploy: https://github.com/orgs/community/discussions/48422 (MEDIUM confidence — community-verified, multiple duplicates)
- GitHub Pages troubleshooting official docs: https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages (HIGH confidence — official)
- Lemon Squeezy license validation API: https://docs.lemonsqueezy.com/api/license-api/validate-license-key (HIGH confidence — official)
- Lemon Squeezy license key guide: https://docs.lemonsqueezy.com/guides/tutorials/license-keys (HIGH confidence — official)
- Google Play badge guidelines: https://partnermarketinghub.withgoogle.com/brands/google-play/visual-identity/badge-guidelines/ (HIGH confidence — official)
- Microsoft Store badge: https://learn.microsoft.com/en-us/windows/apps/publish/microsoft-store-app-badge (HIGH confidence — official)
- localStorage security risks: https://www.trevorlasn.com/blog/the-problem-with-local-storage (MEDIUM confidence — widely cited)
- OWASP HTML5 Security Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/HTML5_Security_Cheat_Sheet.html (HIGH confidence — official)
- Flowbite Tailwind device mockups: https://flowbite.com/docs/components/device-mockups/ (MEDIUM confidence — official component library)

---
*Pitfalls research for: TC-01 Melody Generator Landing Page (static site, GitHub Pages, Tailwind CDN, vanilla JS)*
*Researched: 2026-02-26*

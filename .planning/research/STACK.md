# Stack Research

**Domain:** Static music app landing page (single-page, no build process)
**Researched:** 2026-02-26
**Confidence:** HIGH for core stack; MEDIUM for Lemon Squeezy CORS behavior

## Context

This is a strictly static site: one `index.html` file, Tailwind CSS via CDN, vanilla JS, hosted on GitHub Pages with a custom domain. No build pipeline. The downstream consumer note asks for prescriptive specifics, so each section includes exact CDN URLs, code patterns, and implementation notes.

---

## Recommended Stack

### Core Technologies

| Technology | Version / Source | Purpose | Why Recommended |
|------------|-----------------|---------|-----------------|
| HTML5 | N/A — browser standard | Page structure, SEO semantics | Single file, zero deps; semantic elements (`<main>`, `<section>`, `<article>`) feed search engine structured reading |
| Tailwind CSS Browser CDN | `@tailwindcss/browser@4` via jsDelivr | Utility-first styling | Tailwind v4 is the current major; the Play CDN does JIT compilation in-browser so zero build required; fits the no-build constraint exactly |
| Vanilla JavaScript (ES2022+) | N/A — browser standard | Interactivity, localStorage, async fetch | No framework overhead; `async/await`, optional chaining, and nullish coalescing are universally supported in 2026 target browsers |
| GitHub Pages | Free static hosting tier | Deployment | Native GitHub integration, free HTTPS via Let's Encrypt, custom domain support, zero server maintenance |

### CDN Script Tag (Tailwind v4)

```html
<!-- In <head> — Tailwind v4 Play CDN -->
<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
```

**Important note on production use:** Tailwind officially marks the Play CDN as "not intended for production" because it downloads ~100KB of JS and runs JIT compilation in the browser per page load. For a marketing landing page with low traffic variability, the tradeoff is acceptable: zero build complexity, no CI/CD setup, no Node.js dependency. If the page later needs Core Web Vitals optimization (LCP < 2.5s on low-end devices), migrating to Tailwind CLI + a prebuilt CSS file is the natural escape hatch. **Confidence: HIGH** (verified via official Tailwind docs at `tailwindcss.com/docs/installation/play-cdn`).

**v3 CDN (legacy fallback only):**
```html
<!-- Do NOT use — v3 Play CDN, kept only as reference -->
<script src="https://cdn.tailwindcss.com"></script>
```

### Supporting Libraries

| Library | Version / Source | Purpose | When to Use |
|---------|-----------------|---------|-------------|
| None — intentionally | — | — | This project explicitly rules out libraries |

No JS libraries are needed. The project requirements (localStorage, async fetch, DOM manipulation) are all within native browser APIs.

---

## GitHub Pages: Hosting Configuration

### Files Required

| File | Location | Purpose |
|------|----------|---------|
| `CNAME` | Repo root | Tells GitHub Pages the custom domain (e.g., `tc01.kainiinstruments.com`) |
| `index.html` | Repo root | Entry point; GitHub Pages serves root `index.html` by default |

### Custom Domain DNS Setup

For an **apex domain** (e.g., `kainiinstruments.com`), create four `A` records pointing to GitHub's IPs:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

For a **subdomain** (e.g., `tc01.kainiinstruments.com`), create one `CNAME` record:

```
tc01.kainiinstruments.com  CNAME  yourusername.github.io
```

Enable "Enforce HTTPS" in GitHub repo Settings > Pages after DNS check passes (green checkmark). HTTPS certificate via Let's Encrypt provisions automatically — may take up to 1 hour after DNS check. **Confidence: HIGH** (verified via GitHub Docs).

---

## SEO Meta Tags

### Complete Head Block

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <!-- Primary SEO -->
  <title>TC-01 Melody Generator — AI Melody Sequencer by Kaini Instruments</title>
  <meta name="description" content="Generate unique melodies instantly with TC-01. 100 scales, 25 contours, 8 transforms. Free web app + Android + Windows." />
  <meta name="keywords" content="AI melody generator, music maker app, melody sequencer, MIDI generator, Kaini Instruments" />
  <link rel="canonical" href="https://your-domain.com/" />

  <!-- Open Graph (Facebook, LinkedIn, Discord, Slack) -->
  <meta property="og:type" content="website" />
  <meta property="og:title" content="TC-01 Melody Generator — AI Melody Sequencer" />
  <meta property="og:description" content="Generate unique melodies instantly. 100 scales, 25 contours, 8 transforms. Free web app + Android + Windows." />
  <meta property="og:url" content="https://your-domain.com/" />
  <meta property="og:image" content="https://your-domain.com/assets/og-image.png" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="627" />
  <meta property="og:image:alt" content="TC-01 Melody Generator interface — dark industrial design with 8 transform controls" />
  <meta property="og:site_name" content="Kaini Instruments" />

  <!-- Twitter / X Card -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:title" content="TC-01 Melody Generator — AI Melody Sequencer" />
  <meta name="twitter:description" content="Generate unique melodies instantly. 100 scales, 25 contours, 8 transforms. Free web app + Android + Windows." />
  <meta name="twitter:image" content="https://your-domain.com/assets/og-image.png" />

  <!-- Favicon -->
  <link rel="icon" type="image/png" href="/assets/favicon.png" />
  <link rel="apple-touch-icon" href="/assets/apple-touch-icon.png" />

  <!-- Tailwind v4 CDN -->
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
</head>
```

**Key rules (verified via Open Graph guide and Twitter/X developer docs):**
- `og:*` uses `property=` attribute; `twitter:*` uses `name=` attribute — do not mix them
- Twitter falls back to `og:` tags if `twitter:*` equivalents are missing — minimize duplication
- OG image: 1200x627px, max 5MB, main subject centered (auto-cropped on some platforms)
- `og:title` truncated at ~70 chars by Twitter/X
- `link rel="canonical"` prevents duplicate-content penalties if the domain has www/non-www variants

**Confidence: MEDIUM** (multiple sources agree; verified against official X developer docs).

---

## Phone Mockup: CSS Pattern

Use Tailwind CSS utility classes to render a phone shell around screenshot images — no external library needed, no SVG import. This approach is well-documented in Themesberg/Flowbite guides.

### Structure

```html
<!-- Phone mockup wrapper -->
<div class="relative mx-auto border-gray-800 dark:border-gray-800 bg-gray-800 border-[14px] rounded-[2.5rem] h-[600px] w-[300px] shadow-xl">
  <!-- Side button details (decorative) -->
  <div class="h-[32px] w-[3px] bg-gray-800 dark:bg-gray-800 absolute -left-[17px] top-[72px] rounded-l-lg"></div>
  <div class="h-[46px] w-[3px] bg-gray-800 dark:bg-gray-800 absolute -left-[17px] top-[124px] rounded-l-lg"></div>
  <div class="h-[46px] w-[3px] bg-gray-800 dark:bg-gray-800 absolute -left-[17px] top-[178px] rounded-l-lg"></div>
  <div class="h-[64px] w-[3px] bg-gray-800 dark:bg-gray-800 absolute -right-[17px] top-[142px] rounded-r-lg"></div>

  <!-- Screen area -->
  <div class="rounded-[2rem] overflow-hidden w-[272px] h-[572px] bg-white dark:bg-gray-800">
    <img
      src="assets/screenshots/screen-1.png"
      alt="TC-01 melody generator main screen"
      class="w-full h-full object-cover"
    />
  </div>
</div>
```

**Tailwind v4 custom value syntax** — v4 uses CSS variable-based theming, but the bracket notation `[14px]` for arbitrary values is unchanged from v3. This pattern is safe to copy as-is. **Confidence: HIGH** (Flowbite documentation and Tailwind arbitrary-value docs both confirm bracket syntax works in v4).

---

## Store Badges

### Google Play Badge

**Official source:** Google's Partner Marketing Hub (`partnermarketinghub.withgoogle.com/brands/google-play/visual-identity/badge-guidelines/`)

For direct img embed, use the SVG hosted on the official steverichey mirror (most widely used community resource, English locale):

```html
<a href="https://play.google.com/store/apps/details?id=YOUR_APP_ID" target="_blank" rel="noopener">
  <img
    src="https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png"
    alt="Get it on Google Play"
    height="60"
    class="h-[60px] w-auto"
  />
</a>
```

**Official PNG URL (Google CDN — confirmed working as of 2025):**
```
https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png
```

This is served directly from `play.google.com` and is the URL used in Google's own documentation examples. **Confidence: MEDIUM** (URL confirmed in widespread use; official badge guidelines page requires login to access downloads, but this static path is publicly referenced).

### Microsoft Store Badge

Microsoft provides two implementation paths. Use the **static image** for this no-JS-required context:

**Static badge image (no script required):**
```html
<a href="https://apps.microsoft.com/store/detail/YOUR_APP_ID" target="_blank" rel="noopener">
  <img
    src="https://get.microsoft.com/images/en-us%20dark.svg"
    alt="Get it from Microsoft Store"
    height="60"
    class="h-[60px] w-auto"
  />
</a>
```

**Web component badge (uses JS, more features):**
```html
<script src="https://getbadgecdn.azureedge.net/ms-store-badge.bundled.js" type="module"></script>
<ms-store-badge productid="YOUR_PRODUCT_ID" theme="dark"></ms-store-badge>
```

The web component auto-detects user locale and shows localized text. For simplicity and reliability, the static SVG is recommended — it has no JS dependency and renders consistently. Badge generator page: `apps.microsoft.com/badge?hl=en-US&gl=US`. **Confidence: MEDIUM** (badge script URL confirmed via search; static image URL pattern inferred from Microsoft CDN — verify on badge generator page before launch).

---

## localStorage: License Key Pattern

### Architecture Note: CORS Limitation

The Lemon Squeezy License API endpoint (`https://api.lemonsqueezy.com/v1/licenses/activate`) **does not reliably support browser-side CORS**. Community reports confirm "blocked by CORS policy" errors when calling it directly from browser JS. This means direct `fetch()` from the static page may fail in production.

**Recommended pattern for a no-backend constraint:**

Use `async/await` with a graceful fallback. Structure the code to work even when the real API call is unavailable, so the UI can be built and tested without a backend. When the project is ready to wire up real validation, a lightweight proxy can be added (Cloudflare Worker, Netlify Function, or GitHub Actions webhook) without changing the frontend interface.

### localStorage Abstraction Pattern

```javascript
// license.js — inline in index.html or in a <script> block

const LICENSE_KEY = 'tc01_license';
const INSTANCE_KEY = 'tc01_instance_id';
const IS_PRO_KEY = 'tc01_is_pro';

/**
 * Read the stored pro status synchronously.
 * Returns true if the user has previously activated a valid license.
 */
function getIsProFromStorage() {
  return localStorage.getItem(IS_PRO_KEY) === 'true';
}

/**
 * Persist pro status after successful activation.
 */
function setProStatus(isActive, licenseKey, instanceId) {
  localStorage.setItem(IS_PRO_KEY, String(isActive));
  if (licenseKey) localStorage.setItem(LICENSE_KEY, licenseKey);
  if (instanceId) localStorage.setItem(INSTANCE_KEY, instanceId);
}

/**
 * Attempt to activate a license key via Lemon Squeezy License API.
 * NOTE: This endpoint has documented CORS issues when called from a browser.
 * Replace the fetch URL with a CORS proxy endpoint before going live.
 *
 * @param {string} licenseKey - Raw key string from user input
 * @returns {{ success: boolean, error: string | null }}
 */
async function activateLicense(licenseKey) {
  const instanceName = 'web-' + navigator.userAgent.slice(0, 20);

  try {
    // PLACEHOLDER: Replace with a CORS-enabled proxy URL in production
    // e.g. 'https://your-worker.workers.dev/activate'
    const response = await fetch('https://api.lemonsqueezy.com/v1/licenses/activate', {
      method: 'POST',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: new URLSearchParams({
        license_key: licenseKey,
        instance_name: instanceName,
      }),
    });

    const data = await response.json();

    if (data.activated === true) {
      setProStatus(true, licenseKey, data.instance?.id ?? null);
      return { success: true, error: null };
    } else {
      return { success: false, error: data.error ?? 'Activation failed.' };
    }
  } catch (err) {
    console.warn('License activation fetch failed (likely CORS):', err.message);
    return { success: false, error: 'Could not reach activation server. Try again later.' };
  }
}

/**
 * Apply pro/free UI state on page load.
 * Call this once at DOMContentLoaded.
 */
function applyProUI() {
  const isPro = getIsProFromStorage();
  document.querySelectorAll('[data-pro-locked]').forEach(el => {
    el.classList.toggle('opacity-40', !isPro);
    el.classList.toggle('pointer-events-none', !isPro);
  });
  document.querySelectorAll('[data-pro-badge]').forEach(el => {
    el.classList.toggle('hidden', isPro);
  });
}

document.addEventListener('DOMContentLoaded', applyProUI);
```

**Data attribute convention for Pro locking:**
- `data-pro-locked` on any element that should be visually locked for free users
- `data-pro-badge` on the "PRO" lock badge overlay element itself

**Security note:** Client-side localStorage is not tamper-proof. A user can set `localStorage.setItem('tc01_is_pro', 'true')` manually. For a web-only app without server-side enforcement, this is an acceptable risk given the project's SaaS loop runs through the Android/Windows apps with server validation. The landing page is a placeholder UI — the actual app enforces the license. **Confidence: HIGH for the pattern; MEDIUM for CORS behavior** (CORS issue confirmed via Figma forum post with direct evidence of the error).

---

## HTML5 Video Embed

For the phone recording showcase, use native `<video>` with `autoplay muted loop playsinline` — this is the canonical pattern for marketing loop videos.

```html
<video
  autoplay
  muted
  loop
  playsinline
  poster="assets/video-poster.jpg"
  class="w-full rounded-[2rem] object-cover"
  style="max-height: 600px;"
>
  <source src="assets/demo-video.mp4" type="video/mp4" />
  <!-- Fallback for browsers without video support -->
  <img src="assets/video-poster.jpg" alt="TC-01 demo" />
</video>
```

**Key attributes:**
- `muted` is required for `autoplay` to work on all modern browsers (Chrome/Safari/Firefox policy since 2018)
- `playsinline` is required on iOS to prevent fullscreen takeover
- `poster` shows a static frame before the video loads — use a screenshot from the video
- `loop` keeps it playing as ambient marketing content

**Performance note:** Encode the video at 720p (1280x720), H.264 codec, targeting 3–5 MB for a 10-second clip. For GitHub Pages, assets are served from GitHub's CDN — no separate video hosting needed for this size. **Confidence: HIGH** (MDN Autoplay guide and multiple sources confirm the four-attribute pattern).

---

## Alternatives Considered

| Recommended | Alternative | When to Use Alternative |
|-------------|-------------|-------------------------|
| Tailwind CDN v4 (no build) | Tailwind CLI + prebuilt CSS | If Core Web Vitals become critical; page grows beyond one file |
| Vanilla JS `fetch` + localStorage | React/Preact with state | Never — overkill for a static marketing page |
| `<video autoplay muted loop>` | YouTube embed (iframe) | Only if video > 20 MB and self-hosting bandwidth matters |
| Direct HTML `<img>` for store badges | Microsoft web component `<ms-store-badge>` | If locale-aware badge localization is needed |
| Static site on GitHub Pages | Netlify / Vercel | If serverless functions are needed for CORS proxy |

---

## What NOT to Use

| Avoid | Why | Use Instead |
|-------|-----|-------------|
| `https://cdn.tailwindcss.com` (v3 CDN) | Serves Tailwind v3; v4 is current and has breaking changes if you mix docs | `https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4` |
| Calling Lemon Squeezy API directly from browser JS | CORS policy blocks browser origin in practice | Route through a CORS proxy (Cloudflare Worker) or accept failure gracefully and prompt user to contact support |
| jQuery or Alpine.js | Adds KB for no benefit; vanilla JS handles all requirements | Native `addEventListener`, `fetch`, `querySelector` |
| `<iframe>` for Google Maps or analytics embeds | GitHub Pages HTTPS strict headers; increases attack surface | Only include confirmed HTTPS-only embeds (Tally.so uses HTTPS) |
| Inline `style=""` for theming | Overrides Tailwind's utility cascade; hard to maintain | Use Tailwind arbitrary values `[value]` or `<style type="text/tailwindcss">` blocks |

---

## Stack Patterns by Variant

**If Lemon Squeezy CORS blocks license activation in production:**
- Add a Cloudflare Worker (free tier, zero backend maintenance) as a thin CORS proxy
- Frontend calls `https://your-worker.workers.dev/activate?key=...`
- Worker calls `https://api.lemonsqueezy.com/v1/licenses/activate` server-to-server
- No change to the localStorage pattern above — only the fetch URL changes

**If the phone mockup CSS needs a notch/Dynamic Island:**
- Add a `before:` pseudo-element via Tailwind's `before:content-['']` with `absolute top-0 left-1/2 -translate-x-1/2 w-[30%] h-[25px] bg-gray-800 rounded-b-xl` classes
- This is cosmetic only — screenshots will still display correctly regardless

**If video file exceeds 5 MB GitHub push limit:**
- Use Git LFS (`git lfs track "*.mp4"`) — GitHub LFS has a 1 GB free storage quota
- Alternatively host on Cloudflare R2 (free tier) and reference via HTTPS URL

---

## Version Compatibility

| Package | Version | Notes |
|---------|---------|-------|
| `@tailwindcss/browser` | `@4` (tracks latest v4.x) | v4 targets Safari 16.4+, Chrome 111+, Firefox 128+; adequate for 2026 user base |
| GitHub Pages | N/A | Requires repo to be public, or GitHub Pro for private repo hosting |
| Lemon Squeezy License API | v1 (`/v1/licenses/`) | Separate from the main Lemon Squeezy REST API; no API key required for License API |

---

## Sources

- `https://tailwindcss.com/docs/installation/play-cdn` — Tailwind v4 Play CDN script tag, production warning (HIGH confidence)
- `https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site` — GitHub Pages DNS setup, HTTPS enforcement (HIGH confidence)
- `https://docs.lemonsqueezy.com/api/license-api/activate-license-key` — License API endpoint, request/response format (HIGH confidence for endpoint; MEDIUM for CORS)
- `https://forum.figma.com/ask-the-community-7/cors-issues-with-create-figma-plugin-using-lemonsqueezy-for-license-management-37919` — CORS evidence for Lemon Squeezy browser calls (MEDIUM confidence)
- `https://apps.microsoft.com/badge?hl=en-US&gl=US` — Microsoft Store badge generator page (MEDIUM confidence for static SVG URL pattern)
- `https://play.google.com/intl/en_us/badges/` — Google Play badge guidelines (MEDIUM confidence for static PNG URL)
- `https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Autoplay` — Video autoplay policy and `muted`/`playsinline` requirements (HIGH confidence)
- `https://flowbite.com/docs/components/device-mockups/` — Tailwind phone mockup HTML/CSS pattern (HIGH confidence)
- `https://share-preview.com/blog/og-tags-complete-guide.html` — Open Graph tag specs (MEDIUM confidence)
- `https://developer.x.com/en/docs/x-for-websites/cards/guides/getting-started` — Twitter Card meta tag requirements (HIGH confidence)

---
*Stack research for: TC-01 Melody Generator static landing page*
*Researched: 2026-02-26*

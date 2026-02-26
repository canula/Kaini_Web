# Architecture Research

**Domain:** Static single-page marketing/landing site with vanilla JS interactivity
**Researched:** 2026-02-26
**Confidence:** HIGH

## Standard Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    index.html (single file)                  │
├────────────────────────────┬────────────────────────────────┤
│         Sections (DOM)     │      JavaScript Modules        │
├────────────────────────────┼────────────────────────────────┤
│  #hero                     │  licenseManager.js             │
│  #features                 │    - readState()               │
│  #gallery                  │    - activateKey()             │
│  #demo (web app UI)        │    - persistState()            │
│  #pro / #pricing           │    - applyUIState()            │
│  #contact                  │                                │
├────────────────────────────┼────────────────────────────────┤
│       Overlay Layer        │  modalController.js            │
│  .modal-backdrop           │    - open(modalId)             │
│  #license-modal            │    - close()                   │
│                            │    - trapFocus()               │
├────────────────────────────┴────────────────────────────────┤
│                    Persistent State Layer                    │
│              localStorage: { isPro, instanceId }            │
├─────────────────────────────────────────────────────────────┤
│                    External Integrations                     │
│  Lemon Squeezy License API  |  Tally.so iframe  |  CDNs     │
└─────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| `#hero` | First impression, 3 CTAs (web app, Play, Store) | Full-viewport section, headline + badge row |
| `#features` | Feature grid, Pro badge overlays on locked items | CSS grid with lock icon overlays |
| `#gallery` | Phone mockup carousel/grid with screenshots | CSS phone frames + `overflow: hidden` image containers |
| `#demo` | Static web app UI placeholder showing product | Faux dropdown/button layout, Pro locks in place |
| `#pricing` | Buy Pro CTA + license key activation trigger | Two-column free/pro card layout |
| `#contact` | Tally.so embed placeholder | `<div id="tally-form-container">` with HTML comment |
| `#license-modal` | License key input + submit + feedback states | `<dialog>` or `div.modal` overlay, form-like layout |
| `licenseState` | Read/write `isPro` + `instanceId` to localStorage | Module-pattern JS, called on DOMContentLoaded |
| `nav` | Sticky header with smooth-scroll anchor links | `position: sticky`, `scroll-behavior: smooth` on `<html>` |

## Recommended Project Structure

```
Sequencer-Landing/
├── index.html              # Entire page — all sections, all markup
├── assets/
│   ├── images/
│   │   ├── phone-screenshots/   # Raw screenshot PNGs
│   │   ├── desktop-screenshot.png
│   │   ├── google-play-badge.png
│   │   └── microsoft-store-badge.png
│   └── video/
│       └── demo.mp4             # Phone recording
├── CNAME                   # Custom domain for GitHub Pages (e.g., kainiinstruments.com)
├── .nojekyll               # Disable Jekyll processing on GitHub Pages
└── .planning/              # Project planning (not served)
```

### Structure Rationale

- **Single index.html:** The entire page is one file because there is no build process. Tailwind comes from CDN. All JS is inline `<script>` tags or a single linked `app.js`. No module bundler means no import/export — either inline or a single concatenated script file.
- **assets/images/:** Referenced directly from HTML with relative paths. GitHub Pages serves the repo root — relative paths like `./assets/images/foo.png` resolve correctly.
- **CNAME:** A single line containing the apex domain (e.g., `kainiinstruments.com`). GitHub Pages reads this to configure the custom domain. Must be committed to the repo root.
- **.nojekyll:** An empty file that tells GitHub Pages not to run Jekyll processing. Required if any directory starts with `_` or if you use custom file paths that Jekyll might ignore.

## Architectural Patterns

### Pattern 1: Section-First DOM Structure

**What:** Every page section gets a semantic `<section id="slug">` element with an `aria-label`. Navigation `<a href="#slug">` links scroll directly to them using CSS `scroll-behavior: smooth` on the `<html>` element.

**When to use:** Always — this is the correct pattern for single-page anchor navigation.

**Trade-offs:** Pure CSS smooth scroll has no timing/easing control. If a sticky nav header exists, target sections need `scroll-margin-top` to prevent content hiding behind the nav.

**Example:**
```html
<!-- In <html> or <body> via CSS -->
html { scroll-behavior: smooth; }
section { scroll-margin-top: 64px; } /* match nav height */

<!-- Markup -->
<nav>
  <a href="#features">Features</a>
  <a href="#gallery">Gallery</a>
</nav>

<section id="features" aria-label="Features">
  ...
</section>
```

---

### Pattern 2: localStorage License State Module

**What:** On `DOMContentLoaded`, read `isPro` from localStorage. If true, call `applyProUI()` to remove lock overlays and show Pro-unlocked states. When the user submits a license key, call the Lemon Squeezy activate endpoint, and on success write `isPro: true` and `instanceId` to localStorage, then call `applyProUI()`.

**When to use:** This is the only state that needs persistence on a static site — there is no server session.

**Trade-offs:** localStorage is client-side only — a user can manually set `isPro: true` without a real key. This is acceptable for a landing-page-level gate; the real enforcement happens inside the actual TC-01 app. Never trust localStorage to gate real functionality.

**Example:**
```javascript
const LICENSE_KEY = 'tc01_license';

function readLicenseState() {
  try {
    const raw = localStorage.getItem(LICENSE_KEY);
    return raw ? JSON.parse(raw) : { isPro: false, instanceId: null };
  } catch {
    return { isPro: false, instanceId: null };
  }
}

function saveLicenseState(state) {
  try {
    localStorage.setItem(LICENSE_KEY, JSON.stringify(state));
  } catch {
    // Quota exceeded or private browsing — degrade silently
  }
}

async function activateLicenseKey(key) {
  const body = new URLSearchParams({
    license_key: key,
    instance_name: 'TC-01 Web'
  });
  const res = await fetch('https://api.lemonsqueezy.com/v1/licenses/activate', {
    method: 'POST',
    headers: { Accept: 'application/json' },
    body
  });
  const data = await res.json();
  if (data.activated) {
    saveLicenseState({ isPro: true, instanceId: data.instance.id });
    applyProUI();
    return { success: true };
  }
  return { success: false, error: data.error };
}

function applyProUI() {
  document.querySelectorAll('[data-pro-lock]').forEach(el => {
    el.classList.remove('pro-locked');
  });
  document.querySelectorAll('[data-pro-badge]').forEach(el => {
    el.style.display = 'none';
  });
}

// Init on load
document.addEventListener('DOMContentLoaded', () => {
  const { isPro } = readLicenseState();
  if (isPro) applyProUI();
});
```

---

### Pattern 3: Vanilla JS Modal (dialog element)

**What:** Use the native HTML `<dialog>` element backed by `.showModal()` / `.close()` for the license key activation overlay. The `<dialog>` element handles focus trapping, backdrop rendering, and `Escape` key dismissal natively without custom JS.

**When to use:** Any overlay that needs to be dismissable, accessible, and focused — exactly the license key input flow.

**Trade-offs:** `<dialog>` has full browser support as of 2022+ (Chromium, Firefox 98+, Safari 15.4+). No IE support — but the project targets modern browsers. The `::backdrop` pseudo-element styles the overlay backdrop in CSS.

**Example:**
```html
<dialog id="license-modal" aria-label="Activate Pro License">
  <h2>Activate License Key</h2>
  <p>Enter your Lemon Squeezy license key below.</p>
  <input type="text" id="license-key-input" placeholder="XXXX-XXXX-XXXX-XXXX" />
  <div id="license-feedback" role="status" aria-live="polite"></div>
  <button id="license-submit">Activate</button>
  <button id="license-close" onclick="document.getElementById('license-modal').close()">
    Cancel
  </button>
</dialog>
```

```javascript
// Trigger
document.getElementById('open-license-modal').addEventListener('click', () => {
  document.getElementById('license-modal').showModal();
});

// Submit handler
document.getElementById('license-submit').addEventListener('click', async () => {
  const key = document.getElementById('license-key-input').value.trim();
  const feedback = document.getElementById('license-feedback');
  feedback.textContent = 'Validating...';
  const result = await activateLicenseKey(key);
  if (result.success) {
    feedback.textContent = 'Pro activated! Enjoy your features.';
    setTimeout(() => document.getElementById('license-modal').close(), 1500);
  } else {
    feedback.textContent = result.error || 'Invalid key. Please try again.';
  }
});
```

```css
dialog::backdrop {
  background: rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(4px);
}
```

---

### Pattern 4: Phone Mockup Frame (Tailwind CSS)

**What:** Wrap each screenshot in a pure-CSS phone frame using Tailwind arbitrary values for border width and border-radius. The image sits inside an `overflow: hidden` rounded inner container that clips it to the screen area.

**When to use:** Displaying mobile app screenshots in the gallery section. No external mockup image required — the frame is pure CSS.

**Trade-offs:** CSS frames look clean but are not photorealistic. For a marketing site targeting music/audio users, clean CSS frames are appropriate and load faster than PNG mockup assets.

**Example:**
```html
<!-- Tailwind CSS phone frame (adapted from Flowbite device-mockups) -->
<div class="relative border-[14px] border-zinc-800 rounded-[2.5rem] h-[600px] w-[300px] shadow-xl">
  <!-- Side buttons (decorative) -->
  <div class="absolute -start-[17px] top-[72px] h-[32px] w-[3px] bg-zinc-700 rounded-s-lg"></div>
  <div class="absolute -start-[17px] top-[124px] h-[46px] w-[3px] bg-zinc-700 rounded-s-lg"></div>
  <div class="absolute -end-[17px] top-[142px] h-[64px] w-[3px] bg-zinc-700 rounded-e-lg"></div>
  <!-- Screen area -->
  <div class="rounded-[2rem] overflow-hidden w-full h-full bg-zinc-900">
    <img src="./assets/images/phone-screenshots/screen1.png"
         alt="TC-01 melody sequencer main screen"
         class="w-full h-full object-cover" />
  </div>
</div>
```

For a gallery of multiple screenshots, wrap frames in a flex row with horizontal scroll on mobile:
```html
<div class="flex gap-8 overflow-x-auto pb-4 snap-x snap-mandatory">
  <!-- each phone frame gets snap-center -->
  <div class="snap-center flex-shrink-0 ...phone frame markup..."></div>
</div>
```

## Data Flow

### Page Load State Restoration

```
DOMContentLoaded
    ↓
readLicenseState() → localStorage.getItem('tc01_license')
    ↓
{ isPro: true/false, instanceId: string|null }
    ↓
isPro === true → applyProUI()
    ↓              ↓
    |        remove [data-pro-lock] classes
    |        hide [data-pro-badge] elements
    ↓
isPro === false → render locked UI (default state, no action needed)
```

### License Activation Flow

```
User clicks "Activate License" button
    ↓
#license-modal.showModal()
    ↓
User enters key → clicks "Activate"
    ↓
fetch POST https://api.lemonsqueezy.com/v1/licenses/activate
  body: { license_key, instance_name }
    ↓
Response: { activated: true, instance: { id }, ... }
    ↓
saveLicenseState({ isPro: true, instanceId })  →  localStorage
    ↓
applyProUI()  →  DOM updates (remove locks)
    ↓
#license-modal.close()
```

### Section-to-Section Cross-Links

```
#hero CTA ("Launch Web App")  →  scroll to #demo
#hero CTA ("Get Pro")         →  scroll to #pricing
#features Pro badges          →  open #license-modal
#pricing "Activate" button    →  open #license-modal
#pricing "Buy" link           →  external Lemon Squeezy checkout URL
```

### Key Data Flows

1. **License state init:** localStorage → JS `readLicenseState()` → DOM class mutations — runs once on every page load
2. **License activation:** User input → Lemon Squeezy API → localStorage write → DOM class mutations — runs on form submit
3. **Navigation:** Anchor `href="#id"` → browser native scroll → CSS `scroll-behavior: smooth` — no JS needed
4. **Modal open/close:** JS `dialog.showModal()` / `dialog.close()` → browser-native focus trap + backdrop

## Recommended Section Ordering (Conversion-Optimized)

Based on conversion research, the section order for a product landing page should:
1. Hook immediately above the fold
2. Build credibility through features and social proof
3. Show the product in action (gallery + demo)
4. Present the value exchange (pricing/Pro)
5. Exit with low-friction contact

**Recommended order:**

| Position | Section ID | Purpose | CTA Present |
|----------|-----------|---------|-------------|
| 1 | `#hero` | Headline + sub + 3 CTAs (web, Play, Store) | Yes — primary |
| 2 | `#features` | Feature grid with Pro badges to create desire | No |
| 3 | `#gallery` | Phone mockups + video — proof it exists | No |
| 4 | `#demo` | Static web app UI — product feel placeholder | Yes — "Launch" |
| 5 | `#pricing` | Free/Pro card + buy link + activate modal trigger | Yes — conversion |
| 6 | `#contact` | Tally.so placeholder | No |

**Why this order:** Hero hooks → features create desire → gallery proves quality → demo creates tangible product understanding → pricing converts while desire is high → contact captures latecomers.

## Recommended Build Order (Phase Dependencies)

Build in this sequence because each phase provides the scaffolding the next depends on:

| Build Order | Phase | Why This Order |
|-------------|-------|----------------|
| 1 | HTML skeleton + nav + all section anchors | All JS and CSS targets depend on correct IDs existing |
| 2 | CSS foundation: Tailwind CDN, dark palette, typography | Layout must exist before content can be dropped in |
| 3 | Static sections: hero, features grid | No interactivity — pure HTML/CSS, fast to validate |
| 4 | Gallery section + phone mockup frames | Asset-dependent; needs screenshot files in place |
| 5 | Demo section (static web app UI placeholder) | Needs features list decided to know what to show locked |
| 6 | Modal + license JS + localStorage state | Depends on `[data-pro-lock]` attributes added in step 5 |
| 7 | Pricing section | Depends on modal trigger working correctly |
| 8 | Contact section placeholder | No dependencies; last section, trivial |
| 9 | GitHub Pages deployment (CNAME + .nojekyll) | Deploy after content complete; DNS can propagate in parallel |

## GitHub Pages Deployment Structure

**Required files in repo root:**

```
/CNAME          - One line: your apex domain (e.g., kainiinstruments.com)
/.nojekyll      - Empty file; disables Jekyll processing
/index.html     - The page (GitHub Pages serves index.html as default)
```

**DNS for apex domain (required at registrar):**
- 4x `A` records pointing to GitHub's IPs: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
- Add CNAME first in GitHub settings BEFORE configuring DNS to prevent subdomain takeover risk

**For subdomain (www):**
- Single `CNAME` record: `www` → `<username>.github.io`

**Workflow:** push to `main` branch → GitHub Pages auto-deploys from root → live within ~30 seconds for subsequent pushes, ~10 minutes for DNS propagation on first setup.

## Anti-Patterns

### Anti-Pattern 1: Inline Script per Section

**What people do:** Add `<script>` tags scattered throughout the HTML file next to each section they affect.

**Why it's wrong:** Script ordering becomes fragile — early scripts run before later DOM elements exist. Debugging requires scanning the entire file. Event listeners added before DOMContentLoaded fire against null elements.

**Do this instead:** One `<script>` tag at the bottom of `<body>` (or a single linked `app.js` file). All event listeners registered inside a single `DOMContentLoaded` handler. All license/modal logic in named functions, called from one init function.

---

### Anti-Pattern 2: Storing License Validation Logic in Frontend

**What people do:** Attempt to validate the Lemon Squeezy key client-side with hashing or format checks, treating a valid-format key as "pro."

**Why it's wrong:** Any client-side check is trivially bypassable. The localStorage `isPro` flag can be set manually in DevTools regardless. This gives users false confidence that the check is secure.

**Do this instead:** Always call the Lemon Squeezy API to validate. Accept that localStorage can be spoofed for the landing page's cosmetic locks — the real enforcement gate is inside the TC-01 app itself. The landing page gate is UX friction, not security.

---

### Anti-Pattern 3: Using `scroll-behavior: smooth` Without `scroll-margin-top`

**What people do:** Set `html { scroll-behavior: smooth }` and wire up anchor links, but skip the offset for a sticky nav bar.

**Why it's wrong:** The target section scrolls to the correct position in the document but the top of the section is hidden behind the fixed/sticky navigation bar.

**Do this instead:**
```css
html { scroll-behavior: smooth; }
section[id] { scroll-margin-top: 64px; } /* match actual nav height */
```

---

### Anti-Pattern 4: Phone Mockups with Fixed Pixel Sizes

**What people do:** Build phone frames with fixed `width: 300px; height: 600px` and no responsive scaling.

**Why it's wrong:** On mobile viewports the phone frames either overflow horizontally or become too small to read. A gallery of fixed-size frames breaks at ~375px viewport width.

**Do this instead:** Use a horizontally scrollable flex row with `snap-x snap-mandatory` (Tailwind) so frames can be swiped through on mobile. Each frame keeps its fixed intrinsic size but the gallery container scrolls instead of breaking layout.

---

### Anti-Pattern 5: Jekyll Processing on GitHub Pages Without `.nojekyll`

**What people do:** Deploy a static HTML/CSS/JS site to GitHub Pages without adding `.nojekyll`, then wonder why directories starting with `_` or files in uncommon paths 404.

**Why it's wrong:** GitHub Pages runs Jekyll by default, which transforms (and sometimes omits) files it doesn't recognize, silently breaking asset paths.

**Do this instead:** Commit an empty `.nojekyll` file to the repo root. This disables Jekyll entirely and tells GitHub Pages to serve files exactly as committed.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| Lemon Squeezy License API | `fetch` POST from browser JS to `https://api.lemonsqueezy.com/v1/licenses/activate` | No API key required for activate/validate endpoints — public-facing license API. Validate `data.activated` in response, store `data.instance.id`. |
| Lemon Squeezy Checkout | External link `<a href="https://store.lemonsqueezy.com/..." target="_blank">` | No embed needed — redirect to checkout page |
| Tally.so Contact Form | `<div id="tally-form-container">` placeholder with HTML comment | User pastes Tally iframe code later; no JS required at build time |
| Google Play Badge | `<img src="./assets/images/google-play-badge.png">` inside `<a href="...play.google.com...">` | Official badge image required; cannot recreate from CSS |
| Microsoft Store Badge | `<img src="./assets/images/microsoft-store-badge.png">` inside `<a href="...microsoft.com/store...">` | Same as above |
| Tailwind CSS | CDN `<script src="https://cdn.tailwindcss.com">` in `<head>` | No build step. Inline `tailwind.config` object for custom colors if needed |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| `licenseState` module ↔ DOM sections | `applyProUI()` queries `[data-pro-lock]` and `[data-pro-badge]` attributes | Decoupled via data attributes — sections don't need to know about license logic |
| Modal controller ↔ `licenseState` module | Direct function call: `activateLicenseKey(key)` returns Promise | Modal calls license module; license module owns all API and storage logic |
| Nav ↔ sections | CSS anchor links (`href="#id"`) — zero JS | Browser native; no event listeners needed |
| Pricing CTA ↔ modal | `onclick="document.getElementById('license-modal').showModal()"` | Minimal inline handler acceptable here; modal open is trivial |

## Sources

- [Flowbite Tailwind Device Mockups](https://flowbite.com/docs/components/device-mockups/) — Tailwind phone frame class patterns (MEDIUM confidence — official Flowbite docs)
- [MDN: Using the Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API) — localStorage patterns and feature detection (HIGH confidence — MDN official)
- [Lemon Squeezy: Activate a License Key](https://docs.lemonsqueezy.com/api/license-api/activate-license-key) — API endpoint, request/response shape (HIGH confidence — official docs)
- [MDN: scroll-behavior](https://developer.mozilla.org/en-US/docs/Web/CSS/scroll-behavior) — CSS smooth scroll property (HIGH confidence — MDN official)
- [GitHub Docs: Managing a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site) — CNAME file and DNS setup (HIGH confidence — official GitHub docs)
- [Brandedagency: Anatomy of a High-Converting Landing Page 2026](https://www.brandedagency.com/blog/the-anatomy-of-a-high-converting-landing-page-14-powerful-elements-you-must-use-in-2026) — Section ordering and CTA placement patterns (MEDIUM confidence — industry blog, verified against multiple sources)
- [CSS-Tricks: Smooth Scrolling](https://css-tricks.com/snippets/jquery/smooth-scrolling/) — anchor navigation patterns (MEDIUM confidence — widely referenced source)
- [W3Schools: CSS Devices](https://www.w3schools.com/howto/howto_css_devices.asp) — CSS phone mockup implementation (MEDIUM confidence)

---
*Architecture research for: TC-01 Melody Generator Landing Page (static single-page marketing site)*
*Researched: 2026-02-26*

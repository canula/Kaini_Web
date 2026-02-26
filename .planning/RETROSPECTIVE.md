# Project Retrospective

*A living document updated after each milestone. Lessons feed forward into future planning.*

## Milestone: v1.0 — MVP

**Shipped:** 2026-02-26
**Phases:** 3 | **Plans:** 6 | **Sessions:** 1

### What Was Built
- Complete single-page marketing site for TC-01 Melody Generator (400 LOC HTML)
- SEO-optimized with JSON-LD schema, OG tags, canonical URL
- 6 paired feature/image alternating rows with charcoal grey palette
- Free vs Pro comparison section with Lemon Squeezy checkout CTA
- Responsive layout with sticky nav, hamburger menu, snap-scroll gallery

### What Worked
- Single-day execution (12:13 → 20:25) for full 3-phase landing page
- Yolo mode kept velocity high — no unnecessary confirmation gates
- Phase 3 scope simplification (removing static app UI, moving license activation to in-app) reduced complexity significantly
- Audit → re-audit flow caught GLRY-04 gap and resolved it via rescope

### What Was Inefficient
- Original scope included 8 requirements that were rescoped out — could have been caught during requirements definition
- Gallery approach changed mid-execution (carousel → paired rows) — the Phase 2 gallery work was partially reworked in Phase 3
- Summary files missing one_liner fields — prevented automated accomplishment extraction

### Patterns Established
- Tailwind v4 CDN syntax differs from v3 (bg-linear-to-br not bg-gradient-to-br)
- Flat phone screenshots with rounded-2xl shadow-lg preferred over CSS device frames
- HTML5 video requires all 4 mobile autoplay attributes: autoplay muted loop playsinline
- Placeholder URLs use PLACEHOLDER prefix for easy grep-and-replace before launch

### Key Lessons
1. Scope landing pages conservatively — static pages don't need license activation flows
2. Paired feature/image rows are more effective than grid + separate gallery for marketing
3. Run milestone audit before completion — it caught a real gap (GLRY-04)

### Cost Observations
- Model mix: balanced profile (opus/sonnet/haiku mix per GSD config)
- Sessions: 1
- Notable: Entire v1.0 shipped in ~8 hours wall time including planning, execution, and auditing

---

## Cross-Milestone Trends

### Process Evolution

| Milestone | Sessions | Phases | Key Change |
|-----------|----------|--------|------------|
| v1.0 | 1 | 3 | Initial project — established GSD workflow for static sites |

### Top Lessons (Verified Across Milestones)

1. (Awaiting second milestone for cross-validation)

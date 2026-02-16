# Adaptech Design Company — PLANS.md (ExecPlan Register)

This document contains structured execution plans (ExecPlans) for complex features,
architectural changes, large UI systems, or multi-file refactors.

Every significant change must begin with an ExecPlan before implementation.

---

# ExecPlan Template

## 1. Plan Metadata

- Plan ID:
- Title:
- Author:
- Date:
- Related TODO Section:
- Estimated Scope (S / M / L / XL):
- Risk Level (Low / Medium / High):

---

## 2. Objective

Clearly define:

- What problem this solves.
- Why it is necessary.
- The intended measurable outcome.

Be precise. Avoid vague goals.

---

## 3. Constraints

Document non-negotiables:

- Performance constraints
- Accessibility requirements
- SEO requirements
- Animation standards
- CMS compatibility requirements
- Design system rules
- Technical stack limitations

---

## 4. Current State Analysis

Describe:

- Current architecture relevant to this feature.
- Existing components involved.
- Technical debt or edge cases.
- Known blockers.

Include file paths where applicable.

---

## 5. Proposed Architecture

Provide:

### A. File Structure

Example:

```

web/src/components/hero/
Hero.tsx
HeroBackground.tsx
HeroMotion.ts
web/src/lib/scroll/
useScrollTimeline.ts

```

### B. Component Hierarchy

Describe component tree:

```

HomePage
├── HeroSection
│    ├── MotionLayer
│    ├── TextOverlay
│    └── CTAGroup
├── CaseStudyPreviewGrid
└── Footer

```

### C. Data Flow

- Where data originates.
- How it moves through the system.
- Server vs Client boundaries.
- CMS integration points.

---

## 6. Interaction & Motion Design

Document:

- Animation framework used (Framer / GSAP / CSS)
- Trigger behavior (scroll, hover, load, route transition)
- Reduced motion fallback
- Performance considerations (avoid layout thrashing, GPU layers, etc.)
- Exit strategy if animation causes instability

---

## 7. Accessibility Plan

Define:

- Keyboard navigation
- Focus order
- ARIA attributes
- Color contrast
- Motion reduction behavior

---

## 8. Performance Plan

Specify:

- Lazy loading strategy
- Code splitting
- Dynamic imports
- Image optimization
- Lighthouse performance targets

Target:
- LCP < 2.5s
- CLS < 0.1
- No long main-thread blocking

---

## 9. Implementation Steps (Atomic)

List precise steps in order:

1.
2.
3.
4.

Each step must be verifiable independently.

No vague grouped tasks.

---

## 10. Verification Commands

List exact commands required:

```

pnpm -C web lint
pnpm -C web typecheck
pnpm -C web build
pnpm -C web dev

```

Manual checks:
- Visual inspection checklist
- Keyboard-only test
- Mobile viewport test
- Reduced-motion test

---

## 11. Rollback Plan

If implementation fails:

- Files to revert
- Git strategy (branch, revert, reset)
- Feature flag fallback (if applicable)

Never leave the repo in an unstable intermediate state.

---

## 12. Future Extensions

Optional but recommended:

- What can be layered on top later?
- Hooks for animation expansion?
- CMS scaling implications?
- Theming implications?

---

# ExecPlan Quality Rules

Before implementing:

- The plan must describe file structure.
- The plan must describe data flow.
- The plan must include verification steps.
- The plan must define rollback strategy.

If any section is incomplete, do not proceed to implementation.

---

# Plan Status Tracking

Each plan must end with:

- Status: Draft / Approved / In Progress / Completed
- Commit reference (when complete)

---

# Example Entry Marker

---

# Plan: Hero Scroll-Driven Immersive Section

Status: Draft

(Full plan goes here following the template.)

---

# Plan ID: ADP-001

## 1. Plan Metadata

- Plan ID: ADP-001
- Title: Foundation and Phase 1 Build Plan for Adaptech Website
- Author: Codex
- Date: 2026-02-15
- Related TODO Section: 0 through 5 (Decisions, Setup, IA, Content model baseline, Design system, Global layout)
- Estimated Scope (S / M / L / XL): XL
- Risk Level (Low / Medium / High): Medium

---

## 2. Objective

Build a production-ready foundation for a premium, motion-forward Next.js site by replacing the starter app with:
- clear architecture and route skeletons,
- reusable design primitives and layout shell,
- motion and accessibility guardrails,
- CMS-ready typed data boundaries.

Measurable outcome:
- All primary routes exist and render.
- Shared theme tokens and UI primitives are in place.
- Lint/typecheck/build pass.
- Reduced-motion behavior is implemented for all baseline animations.

---

## 3. Constraints

- Performance: keep Core Web Vitals targets viable (no heavy blocking JS, lazy media by default).
- Accessibility: WCAG AA intent, keyboard-first nav, visible focus states, reduced-motion support.
- SEO: metadata baseline for all public routes.
- Animation: Framer Motion by default; GSAP only for scroll storytelling sections.
- CMS: typed integration boundary; no hard dependency that blocks local development.
- Stack: Next.js App Router + TypeScript + Tailwind v4.

---

## 4. Current State Analysis

- `web/` is a clean starter scaffold with only:
  - `src/app/layout.tsx`
  - `src/app/page.tsx`
  - `src/app/globals.css`
- No route architecture beyond `/`.
- No component system under `src/components`.
- No CMS or data model code.
- No typecheck script in `web/package.json`.
- Branding, content structure, and SEO metadata are placeholders.

Key blocker:
- TODO assumes a mature baseline, but the current implementation is still at starter state. Foundation work is required before advanced motion/CMS sections.

---

## 5. Proposed Architecture

### A. File Structure

```
web/src/app/
  (site)/
    layout.tsx
    page.tsx
    about/page.tsx
    services/page.tsx
    case-studies/page.tsx
    case-studies/[slug]/page.tsx
    blog/page.tsx
    blog/[slug]/page.tsx
    careers/page.tsx
    careers/[slug]/page.tsx
    contact/page.tsx
    privacy/page.tsx
    terms/page.tsx
web/src/components/
  ui/
    Button.tsx
    Card.tsx
    Input.tsx
    Textarea.tsx
    Badge.tsx
  layout/
    Header.tsx
    Footer.tsx
    MenuOverlay.tsx
web/src/lib/
  motion/
    motion-config.ts
    useReducedMotionSafe.ts
  seo/
    metadata.ts
  cms/
    types.ts
    client.ts
web/src/styles/
  tokens.css
```

### B. Component Hierarchy

```
RootLayout
└── SiteLayout
    ├── Header
    │   └── MenuOverlay
    ├── PageContent (route pages)
    └── Footer
```

### C. Data Flow

- Static/local content used first to unblock UI development.
- Route pages consume typed content adapters from `src/lib/cms/types.ts`.
- CMS client is initialized behind environment checks; pages gracefully fallback to local mock content if CMS is not configured.
- Metadata is generated at page boundary through shared helpers in `src/lib/seo/metadata.ts`.

---

## 6. Interaction & Motion Design

- Framer Motion:
  - menu open/close,
  - route section reveals,
  - card hover/press states.
- GSAP/ScrollTrigger:
  - deferred to Home hero storytelling phase only after baseline is stable.
- Reduced motion:
  - centralized guard in `useReducedMotionSafe`.
  - animation variants downgrade to simple opacity transitions.
- Performance:
  - animate transform/opacity only,
  - avoid forced layout and scroll-linked expensive effects in initial phase.

---

## 7. Accessibility Plan

- Header/menu keyboard navigation with Escape close and focus trap.
- Focus-visible styles enforced in primitives.
- Semantic landmarks (`header`, `main`, `footer`, proper heading order).
- Form components expose labels, error text mapping, and `aria-describedby`.
- Reduced-motion preference respected globally.

---

## 8. Performance Plan

- Use server components by default for route pages.
- Client components only for interactive UI and animation islands.
- Use Next image optimization for all non-decorative images.
- Defer heavy motion libraries to route-level dynamic imports where possible.
- Initial target:
  - LCP < 2.5s on key marketing pages
  - CLS < 0.1
  - no animation-driven long tasks.

---

## 9. Implementation Steps (Atomic)

1. Foundation scripts and config
- Add `typecheck` script and verify lint/build/typecheck command set.
- Normalize metadata defaults and global CSS token entry points.

2. Route skeleton and layout shell
- Create all required App Router paths with minimal content.
- Implement shared site layout with header/footer placeholders.

3. Design tokens and UI primitives
- Add `tokens.css` and wire into globals.
- Implement first-pass primitives: Button, Card, Input, Textarea, Badge.

4. Navigation and overlay behavior
- Implement sticky header and full-screen menu overlay.
- Add keyboard controls and focus management.

5. Motion baseline with reduced-motion safety
- Add Framer Motion setup for overlay and section reveals.
- Implement reduced-motion fallback path.

6. CMS-ready typed boundary
- Add CMS type definitions and guarded client initialization.
- Keep page data local until credentials exist.

7. SEO baseline
- Add shared metadata helpers.
- Ensure each route emits meaningful title/description defaults.

8. Verify and stabilize
- Run lint/typecheck/build.
- Run manual dev smoke test for route navigation, keyboard flows, and reduced-motion.

---

## 10. Verification Commands

```bash
pnpm -C web lint
pnpm -C web typecheck
pnpm -C web build
pnpm -C web dev
```

Manual checks:
- Navigate all planned routes with keyboard only.
- Confirm menu opens/closes via keyboard and Escape.
- Enable reduced motion in OS/browser and verify minimal animation behavior.
- Validate responsive layout on mobile and desktop breakpoints.

---

## 11. Rollback Plan

- Use atomic commits per implementation step.
- If a step destabilizes the app:
  - revert only files introduced by that step,
  - keep prior verified steps intact.
- Keep GSAP integration behind an isolated module so it can be disabled without touching route markup.
- If CMS integration fails, keep local content adapters as fallback and ship without live CMS.

---

## 12. Future Extensions

- Add Sanity schemas and preview URLs once content model is approved.
- Add case-study filtering/search and blog tagging.
- Add OpenGraph image generation route.
- Add analytics and error tracking instrumentation.

---

- Status: Draft
- Commit reference (when complete): N/A

---

## Navigation Status
- Single source of truth: `web/src/content/nav.ts`
- Route typing enforced via `SiteRoute` in `web/src/content/routes.ts`
- Active resolution unified in `web/src/lib/nav.ts`
- Header style contract centralized in `web/src/components/site/headerStyles.ts`
- Overlay accessibility in place:
  - focus trap
  - Escape close
  - focus restore to opener
  - `aria-modal` semantics

Next Phase:
- Hero typography dominance refinement
- CMS field mapping pass for home/nav content objects
- Performance pass for home scrolling/observer behavior under low-end devices



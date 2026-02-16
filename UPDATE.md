## [2026-02-16]

### Navigation System Consolidation
- Replaced legacy `NAV_LINKS` usage with content-driven `PRIMARY_NAV` in `web/src/content/nav.ts`.
- Removed legacy `web/src/components/layout/Header.tsx` and `web/src/components/layout/nav-links.ts`.
- Desktop header and overlay now consume the same nav array from `SiteHeader`.
- Active-state resolution unified via `resolveNavActive` in `web/src/lib/nav.ts`.

### Accessibility Hardening
- Added `aria-current="page"` support through `web/src/components/site/UnderlineLink.tsx`.
- Added skip-to-content link as first focusable element in `web/src/components/site/SiteHeader.tsx`.
- Added stable main landmark target in `web/src/app/(site)/layout.tsx`:
  - `id="main"`
  - `tabIndex={-1}`
- Overlay now restores focus to the menu button on close and traps focus when open.
- Overlay sets `aria-hidden="true"` on main content while open for modal behavior.

### Scroll Behavior
- Added deterministic scrolled-header state via `web/src/lib/useHeaderScrolled.ts`.
- Added motion-safe shell transitions through `web/src/components/site/headerStyles.ts`.
- Added shared header metrics helper in `web/src/lib/headerMetrics.ts` and unified threshold math.

### Homepage Section Awareness
- Added `useActiveSection` IntersectionObserver logic in `web/src/lib/useActiveSection.ts`.
- Added anchor-based section mapping in `web/src/content/home.ts` (`anchorId` on sections + CTA).
- Added route-aware + section-aware active resolver order on `/`:
  - hash first,
  - then observer-derived anchor.
- Added anchor-safe landing offsets using `scroll-mt-[calc(var(--header-h)+16px)]`.

### Header Style Contract
- Added `web/src/components/site/headerStyles.ts` as canonical source for:
  - shell states (top/scrolled)
  - desktop nav states
  - overlay nav states
  - shared focusable class funnel.
- Replaced duplicated class strings in `SiteHeader` and `MenuOverlay` with style constants.

### Validation and Hygiene
- Final sanity pass completed:
  - `pnpm -C web lint`
  - `pnpm -C web typecheck`
  - `pnpm -C web build` (ran unsandboxed after sandbox `spawn EPERM`)
- Dev artifact scan:
  - no `console.log` in `web/src`
  - no `TODO` in `web/src`
  - no `FIXME` in `web/src`
- `.env.local` check:
  - `web/.env.local` is not present and not tracked.
- Dependency check:
  - `pnpm -C web outdated` partially resolved before network-restricted metadata failures.
  - Noted newer versions detected for `react`, `react-dom`, `@types/node`, and `eslint`.


# Navigation Architecture Snapshot

## Content Sources
- Routes: `web/src/content/routes.ts`
- Primary nav items: `web/src/content/nav.ts`
- Home section anchors/content: `web/src/content/home.ts`

## Active Resolution Flow
- Shared resolver: `web/src/lib/nav.ts` (`resolveNavActive`)
- Non-home routes (`pathname !== "/"`):
  - active from pathname route matching
- Home route (`pathname === "/"`):
  - hash anchor wins if present
  - otherwise observer-driven `activeAnchorId` from section visibility

## Scroll Header Logic
- Hook: `web/src/lib/useHeaderScrolled.ts`
- Header metrics source: CSS token `--header-h` in `web/src/app/globals.css`
- Threshold uses derived value from `web/src/lib/headerMetrics.ts`
- Header shell state classes:
  - `HEADER_SHELL_TOP`
  - `HEADER_SHELL_SCROLLED`
  - centralized in `web/src/components/site/headerStyles.ts`

## Overlay Focus Behavior
- Component: `web/src/components/layout/MenuOverlay.tsx`
- On open:
  - focus moves to first interactive element in overlay
  - background scroll locked
- Keyboard:
  - `Escape` closes
  - `Tab` and `Shift+Tab` are trapped within overlay focusables
- On close:
  - focus restored to menu opener button via callback from `SiteHeader`
- Modal semantics:
  - `role="dialog"`
  - `aria-modal="true"`
  - main content `#main` gets `aria-hidden="true"` while open

## Style Contract
- Canonical class constants: `web/src/components/site/headerStyles.ts`
- Shared interactive link base:
  - `UnderlineLink` for desktop header links
  - overlay links consume `OVERLAY_*` constants

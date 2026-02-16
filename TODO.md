# TODO.md — Adaptech Design Company (NYC) Next-Level Website

> Target: Fortune 500 + celebrities. Bold, experimental UI with premium usability, performance, SEO, and accessibility.

## 0) Decisions and Ground Rules (lock first)

- [ ] Choose stack baseline:
  - Next.js 15 (App Router) + React + TypeScript
  - Tailwind CSS + CSS variables for theming
  - Framer Motion for UI transitions and component-level motion
  - GSAP + ScrollTrigger for scroll-driven storytelling and reveals :contentReference[oaicite:0]{index=0}
  - Headless CMS: Sanity (preferred) or Contentful fallback :contentReference[oaicite:1]{index=1}
- [ ] Decide whether to enable Next.js `viewTransition` experimental flag for route transitions (default: OFF until stable) :contentReference[oaicite:2]{index=2}
- [ ] Define accessibility baseline: WCAG AA + reduced motion support via prefers-reduced-motion (required) :contentReference[oaicite:3]{index=3}
- [ ] Define performance baseline: Core Web Vitals green, no scroll-jank, media optimized, lazy-loading.

## 1) Repo Setup and Tooling

- [ ] Initialize repo: `pnpm create next-app` (TS, ESLint, App Router).
- [ ] Add dependencies:
  - [ ] `tailwindcss postcss autoprefixer`
  - [ ] `framer-motion`
  - [ ] `gsap`
  - [ ] `@sanity/client next-sanity` (if Sanity)
  - [ ] `zod` (schema validation)
  - [ ] `react-hook-form` (forms)
- [ ] Add dev tooling:
  - [ ] Prettier + sort imports
  - [ ] Husky + lint-staged (optional)
- [ ] Set env templates:
  - [ ] `.env.example` with CMS keys, form endpoint, analytics.
- [ ] Configure CI (GitHub Actions):
  - [ ] lint, typecheck, build.

## 2) Information Architecture and Routes

Create routes in `app/`:

- [ ] `/` Home
- [ ] `/about`
- [ ] `/services`
- [ ] `/case-studies` (index)
- [ ] `/case-studies/[slug]` (detail)
- [ ] `/blog` (index)
- [ ] `/blog/[slug]` (detail)
- [ ] `/careers` (index)
- [ ] `/careers/[slug]` (job detail)
- [ ] `/contact`
- [ ] `/privacy` + `/terms`

## 3) Content Model (Headless CMS)

Sanity schemas (or equivalent):

- [ ] `siteSettings` (nav links, footer, socials, office info, SEO defaults)
- [ ] `caseStudy`:
  - title, slug, client, industry, services[], heroMedia, gallery[], metrics[], testimonial, body blocks
- [ ] `post`:
  - title, slug, excerpt, coverImage, author, publishDate, tags[], body blocks
- [ ] `job`:
  - title, slug, location, type, department, intro, responsibilities, requirements, applyUrl, status
- [ ] `service`:
  - title, slug, shortDesc, icon, longDesc, featuredWorkRefs[]
- [ ] Implement CMS preview URLs for case studies and posts.

## 4) Design System and Visual Language (bold and experimental)

- [ ] Implement theme tokens with CSS variables:
  - [ ] colors (dark, light, accent, gradient stops)
  - [ ] typography scale (display, headline, body)
  - [ ] spacing, radii, shadows, blur/glow presets
- [ ] Dark mode support:
  - [ ] system preference + manual toggle
  - [ ] persist in localStorage
- [ ] Typography:
  - [ ] choose premium pairing (display + body)
  - [ ] implement fluid typography (clamp)
- [ ] Build UI primitives (in `components/ui/`):
  - [ ] Button (variants)
  - [ ] Link
  - [ ] Input / Textarea
  - [ ] Select
  - [ ] Card
  - [ ] Badge / Tag
  - [ ] Modal / Drawer
  - [ ] Toast / Notice
  - [ ] Tooltip
- [ ] Accessibility baked into primitives (focus rings, ARIA patterns).

## 5) Global Layout Components

- [ ] Header (sticky / auto-hide on scroll down, reveal on scroll up)
- [ ] Experimental Menu Overlay:
  - [ ] full-screen overlay nav
  - [ ] hover previews (image/video snippets per section)
  - [ ] keyboard nav and escape close
- [ ] Footer:
  - [ ] bold CTA strip ("Start a project")
  - [ ] multi-column links + NYC office details + social
  - [ ] newsletter signup (optional)
- [ ] Page shell and transitions:
  - [ ] route-level transitions via Framer Motion (default)
  - [ ] optional Next view transitions behind feature flag :contentReference[oaicite:4]{index=4}

## 6) Motion System (serious, performance-safe)

Define motion rules:

- [ ] Motion tiers:
  - [ ] Tier 0: reduced motion (fade only)
  - [ ] Tier 1: standard (micro-interactions + reveals)
  - [ ] Tier 2: premium (scroll story sections + pinned panels)
- [ ] Reduced motion support using `prefers-reduced-motion` and `gsap.matchMedia()` :contentReference[oaicite:5]{index=5}
- [ ] GSAP ScrollTrigger setup:
  - [ ] reveal animations (stagger, opacity, y transform)
  - [ ] pinned hero sequences on Home
  - [ ] case study image reveal timelines :contentReference[oaicite:6]{index=6}
- [ ] Framer Motion usage:
  - [ ] menu overlay open/close
  - [ ] page content entrance/exit
  - [ ] hover/press interactions for cards and buttons

## 7) Page Builds (UI/UX spec as code)

### Home (`/`)

- [ ] Hero: full-bleed cinematic media + kinetic headline
- [ ] Scroll-driven story sections:
  - [ ] Mission / positioning
  - [ ] Services preview grid (interactive)
  - [ ] Featured case studies (editorial gallery)
  - [ ] Client trust section (logos + short proof points)
  - [ ] CTA: contact / project intake
- [ ] Implement “editorial” layout with bold typography, asymmetry, controlled whitespace.
- [ ] Optional: lightweight WebGL accent (only if performance budget allows).

### About (`/about`)

- [ ] Story blocks + philosophy
- [ ] Team grid (hover reveal bios)
- [ ] Awards / credibility section
- [ ] CTA to contact

### Services (`/services`)

- [ ] Services list + detail modules
- [ ] Each service ties to related case studies
- [ ] “Capabilities” matrix component (interactive)

### Case Studies Index (`/case-studies`)

- [ ] Gallery modes:
  - [ ] Grid masonry
  - [ ] Horizontal editorial carousel option
- [ ] Filters by service/industry
- [ ] Hover preview motion

### Case Study Detail (`/case-studies/[slug]`)

- [ ] Hero media
- [ ] Project overview (role, scope, metrics)
- [ ] Story sections (problem → approach → results)
- [ ] Media gallery blocks (image/video)
- [ ] Next/Prev navigation with transition

### Blog Index + Post

- [ ] Clean reading layout (typography and spacing optimized)
- [ ] Tags, search (optional)
- [ ] Social share meta and OG images

### Careers (`/careers`)

- [ ] Culture section
- [ ] Jobs list with filters (dept, location, type)
- [ ] Job detail pages with apply CTA

### Contact (`/contact`)

- [ ] High-conversion form UX:
  - [ ] multi-step intake
  - [ ] validation, spam protection (honeypot + rate limit)
- [ ] NYC map + direct contact details
- [ ] Submission success state with motion

## 8) Media Pipeline (premium visuals, fast load)

- [ ] Use Next Image optimization everywhere possible
- [ ] Generate responsive sizes for all images
- [ ] Use modern formats (AVIF/WebP)
- [ ] Video:
  - [ ] short loops, muted, autoplay only when in view
  - [ ] fallback poster images
- [ ] Lazy-load below-the-fold media
- [ ] Preload critical hero assets carefully

## 9) SEO, Schema, and Social

- [ ] SEO defaults in `siteSettings`
- [ ] Per-page metadata:
  - [ ] title, description, canonical, OG, Twitter card
- [ ] Structured data:
  - [ ] Organization
  - [ ] Article (blog)
  - [ ] JobPosting (careers)
- [ ] XML sitemap + robots.txt
- [ ] OpenGraph image generation route (optional)

## 10) Accessibility and UX Quality Gates

- [ ] Keyboard navigable menu and modals
- [ ] Visible focus states everywhere
- [ ] Proper headings structure
- [ ] Color contrast checks (AA)
- [ ] Reduced motion support enforced :contentReference[oaicite:7]{index=7}
- [ ] Forms: labels, errors, aria-describedby
- [ ] Lighthouse CI thresholds (performance, a11y, SEO)

## 11) Analytics and Observability

- [ ] Privacy-friendly analytics (Plausible or GA4)
- [ ] Event tracking:
  - [ ] menu opens
  - [ ] case study clicks
  - [ ] contact form steps and completion
- [ ] Error tracking (Sentry optional)

## 12) Deployment

- [ ] Deploy to Vercel (preferred for Next) or Netlify
- [ ] Configure preview deployments per PR
- [ ] Configure CMS webhooks to revalidate pages (ISR or on-demand revalidation)
- [ ] Set caching headers for static assets

## 13) Final Polish Pass

- [ ] Motion tuning: reduce jank, ensure 60fps targets
- [ ] Copy polish: short, confident, premium tone
- [ ] Cross-device QA: desktop, mobile, tablet
- [ ] Reduced-motion QA and keyboard-only QA
- [ ] Final performance pass, ship

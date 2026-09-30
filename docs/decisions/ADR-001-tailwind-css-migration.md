# ADR-001: Tailwind CSS as the CSS Framework for TripBoard

**Date:** 2026-09-17
**Status:** Accepted
**Deciders:** Arpita Shelke, TripBoard Development Team

---

## Context

TripBoard's current frontend is a static multi-page HTML application styled via a single monolithic [style.css](file:///c:/projects/TripBoard/style.css) file of 6,077 lines. This file:

- Contains duplicated class definitions across all 10 pages with no shared design tokens.
- Has no CSS custom properties (variables) for colors, spacing, or typography.
- References design patterns (cards, badges, status indicators, avatars) that are repeated in long, page-specific blocks.
- Is maintained manually by two developers in parallel, increasing the risk of style drift and merge conflicts.

Additionally, the codebase contains an isolated Next.js + TypeScript + Supabase authentication module (`login_export/`) that is being evaluated for full integration into the platform. Any CSS framework selection must be forward-compatible with this Next.js architecture.

The team requires a framework that:
1. Eliminates duplication while remaining compatible with existing static HTML pages.
2. Maps directly onto TripBoard's established visual design (navy palette, ice backgrounds, gold accents, rounded cards).
3. Provides a clear migration path into React/Next.js components when the backend integration occurs.
4. Is manageable by two developers without heavyweight build tooling during the early HTML prototype phase.

---

## Decision

**Adopt Tailwind CSS (v3) as the sole CSS framework for TripBoard.**

Tailwind CSS will replace the monolithic `style.css` progressively, page by page, using Tailwind's CDN Play script for the static HTML phase and the full CLI/PostCSS pipeline when migrating into Next.js.

---

## Design Token Mapping

The following TripBoard brand tokens will be registered in `tailwind.config.js` under `theme.extend.colors`:

| Token Name | Hex Value | Usage |
|---|---|---|
| `primary-dark` | `#071D3A` | Header/hero dark overlay, deep navy |
| `primary-blue` | `#172B4D` | Body text, headings, navbar base |
| `brand-blue` | `#2E5BFF` | Buttons, active states, links |
| `bg-ice` | `#EAF5FC` | Page background, section backgrounds |
| `accent-gold` | `#D4AF37` | Card accents, decorative lines, highlights |
| `status-green` | `#2ECC71` | Verified, approved, success states |
| `status-orange` | `#F39C12` | Pending, in-progress, warning states |

**Border Radius Convention:**
- Standard cards: `rounded-xl` (12px)
- Elevated containers: `rounded-2xl` (16px)

**Shadow Convention:**
- Default card elevation: `shadow-[0_4px_20px_rgba(0,0,0,0.08)]`
- Navbar/header: `shadow-[0_3px_15px_rgba(0,0,0,0.25)]`

---

## Migration Approach

### Phase 1 — Foundation (Current Milestone)
- Document ADR and configure design tokens (this file).
- Use Tailwind CDN Play script for HTML prototyping without a build step.

### Phase 2 — Component Extraction
- Migrate global shared components first: Navbar, Header, Footer, Buttons, Status Badges.
- Migrate landing page (`index.html`, `login.html`).

### Phase 3 — Dashboard Pages
- Migrate Admin Dashboard (`admin.html`), Employee Dashboard (`employee-dashboard.html`).

### Phase 4 — Management Pages
- Migrate `employees.html`, `trips.html`, `create-trip.html`, `add-employee.html`, `employee-details.html`, `trip-details.html`, `documents.html`.

### Phase 5 — Legacy Deprecation & Next.js Integration
- Deprecate `style.css` entirely.
- Transition to Next.js App Router pages using Tailwind CSS with `tailwind.config.ts` + PostCSS.
- Integrate `login_export/` styled under the same Tailwind token system.

---

## Alternatives Considered

| Framework | Reason Rejected |
|---|---|
| **Bootstrap 5** | Large bundle size, requires heavy overrides to achieve TripBoard's custom card/color design, poor fit with Next.js without additional configuration. |
| **CSS Modules + SCSS** | Maintains manual class authoring effort, no utility-first benefit, still risks duplication across modules, no built-in design token standardization. |
| **Vanilla CSS with Custom Properties** | Incremental improvement over the current state but no ecosystem, no component patterns, no build-time optimization, no forward path into React. |

---

## Consequences

### Positive
- **Eliminates 6,000+ lines of CSS debt** progressively page by page.
- **Design tokens in `tailwind.config.js`** ensure visual consistency without manual color lookups.
- **Two-developer parallel work** becomes safer — Tailwind utilities are localized to their HTML/JSX elements, reducing merge conflict surface area.
- **Seamless transition into Next.js** — Tailwind is the default standard in Next.js App Router projects.
- **PurgeCSS included** — production builds ship only used utility classes, achieving minimal bundle sizes.

### Negative / Risks
- Initial learning investment for developers unfamiliar with utility-first styling.
- During the transition phase, `style.css` and Tailwind classes coexist, requiring discipline to not re-add legacy classes.

### Mitigation
- Migrate one page at a time per milestone, keeping `style.css` active until full replacement.
- Use Tailwind CDN Play for zero-install HTML prototyping before committing to the build pipeline.

---

## References
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Next.js + Tailwind CSS Setup Guide](https://tailwindcss.com/docs/guides/nextjs)
- [TripBoard AGENTS.md — Rule 19](file:///c:/projects/TripBoard/AGENTS.md)

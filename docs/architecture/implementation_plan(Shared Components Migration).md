# Implementation Plan: Shared Components Migration (Tailwind CSS)

A structured implementation plan to migrate shared layout components (Header, Navbar, Footer, and Buttons) across all 12 TripBoard HTML pages, matching the approved reference design in [DESIGNS/shared-components-preview.html](file:///c:/projects/TripBoard/DESIGNS/shared-components-preview.html).

---

## 1. Background & Scope

In the Foundation stage (ADR-001), Tailwind CSS was adopted as the design framework. Visual and layout patterns were consolidated in [shared-components-preview.html](file:///c:/projects/TripBoard/DESIGNS/shared-components-preview.html) using clean vector assets (`Assets/logo_svg.svg`) and high-resolution globe backgrounds (`Assets/bg2.png`), resolving header crowding and text collisions.

This implementation plan migrates the shared layout components across the entire TripBoard HTML application:
1. **Asset Deployment**: Make `Assets/logo_svg.svg` and `Assets/bg2.png` globally available from the project root.
2. **Public Landing Pages**: `index.html`, `home.html`, and `login.html`.
3. **Admin Suite**: `admin.html`, `employees.html`, `trips.html`, `add-employee.html`, `employee-details.html`, `create-trip.html`, and `trip-details.html`.
4. **Employee Portal**: `employee-dashboard.html` and `documents.html`.
5. **Shared Footer**: Deploy standardized navy footer (`#071D3A`) with ice-blue text across all pages.

---

## 2. Core Architecture & Workflow Flowchart

```
+---------------------------------------------------------------------------------+
|                    Shared Components Migration Workflow                         |
+---------------------------------------------------------------------------------+
                                         |
                                         v
+---------------------------------------------------------------------------------+
|  Milestone 1: Root Assets & Tailwind Configuration Setup                        |
|  - Copy DESIGNS/Assets -> Assets/ at root                                       |
|  - Standardize Tailwind CDN script and theme extension block                    |
+---------------------------------------------------------------------------------+
                                         |
                                         v
+---------------------------------------------------------------------------------+
|  Milestone 2: Landing & Public Views (index.html, home.html, login.html)        |
|  - Brand lockup with logo_svg.svg + tagline                                     |
|  - Nav links + Gold Login button (#C9A227)                                      |
|  - Shared Tailwind footer                                                       |
+---------------------------------------------------------------------------------+
                                         |
                                         v
+---------------------------------------------------------------------------------+
|  Milestone 3: Admin Suite Headers (admin, employees, trips, details)            |
|  - Admin brand header with bg2.png background                                   |
|  - Active tab highlighting (Dashboard, Employees, Trips)                        |
|  - Gold Logout button (matching Login dimensions)                               |
+---------------------------------------------------------------------------------+
                                         |
                                         v
+---------------------------------------------------------------------------------+
|  Milestone 4: Employee Portal Headers (employee-dashboard, documents)           |
|  - Employee brand header with bg2.png background                                |
|  - Active tab highlighting (My Dashboard, My Documents)                         |
|  - Gold Logout button + Shared footer                                           |
+---------------------------------------------------------------------------------+
                                         |
                                         v
+---------------------------------------------------------------------------------+
|  Milestone 5: Verification, Daily Dev Log & Git Commit                          |
|  - Cross-page link integrity checks                                             |
|  - Dev log entry in docs/logs/2026-09-30.md                                     |
|  - Local commit on feature branch                                               |
+---------------------------------------------------------------------------------+
```

---

## 3. Component Specification (matching DESIGNS/shared-components-preview.html)

### Design Tokens & Configuration
```javascript
tailwind.config = {
    theme: {
        extend: {
            colors: {
                "primary-dark": "#071D3A",
                "primary-blue": "#172B4D",
                "brand-blue": "#2E5BFF",
                "bg-ice": "#EAF5FC",
                "accent-gold": "#D4AF37",
                "accent-gold-hover": "#A98717",
                "gold-btn": "#C9A227",
                "gold-btn-hover": "#A98717",
                "gold-light": "#F4D675",
                "status-green": "#2ECC71",
                "status-orange": "#F39C12",
                "footer-text": "#B8C9DA",
            },
            borderRadius: {
                "xl": "12px",
                "2xl": "16px",
            },
            boxShadow: {
                "card": "0 4px 20px rgba(0,0,0,0.08)",
                "nav": "0 4px 20px rgba(7,29,58,0.25)",
                "btn-gold": "0 4px 10px rgba(201,162,39,0.35)",
                "btn-gold-hover": "0 6px 15px rgba(201,162,39,0.50)",
            },
            fontFamily: {
                sans: ["Arial", "sans-serif"],
            }
        }
    }
}
```

### 1. Brand Lockup
- Logo image: `Assets/logo_svg.svg` (`h-12 md:h-14 w-auto drop-shadow-[0_2px_8px_rgba(0,0,0,0.35)]`)
- Title: `TRIPBOARD` (`text-white font-black text-2xl md:text-[27px] tracking-[2px]`)
- Subtitle: `Corporate Travel Management. Simplified.` (`text-sky-200/90 text-[10px] md:text-[11px] tracking-wider`)

### 2. Header Background & Frame
- Container: `min-h-[110px] md:h-[125px] relative flex flex-col md:flex-row items-center justify-between px-6 md:px-10 py-3 md:py-0 bg-cover bg-right md:bg-center bg-no-repeat`
- Inline background: `background-image: url('Assets/bg2.png'); background-color: #0c3370;`
- Wrapped in: `rounded-2xl overflow-hidden shadow-nav border border-slate-200` (or full width with rounded border)

### 3. Action Buttons (Login / Logout Parity)
- Class: `bg-gold-btn hover:bg-gold-btn-hover text-white text-sm font-semibold py-2 px-5 rounded-md shadow-btn-gold hover:shadow-btn-gold-hover hover:-translate-y-0.5 transition-all duration-200`

### 4. Shared Footer
- Container: `bg-primary-dark text-footer-text text-center py-5 px-4 text-sm mt-auto`
- Text: `© 2026 TripBoard. All Rights Reserved.`

---

## 4. Proposed Changes by Page

| Page | Role / Target | Changes |
|---|---|---|
| `Assets/` | Project Root | Copy `DESIGNS/Assets/*` into `Assets/` for universal relative path access |
| [index.html](file:///c:/projects/TripBoard/index.html) | Landing | Add Tailwind CDN + theme config; replace `<header>` with Tailwind brand navbar; replace `<footer>` with Tailwind footer |
| [home.html](file:///c:/projects/TripBoard/home.html) | Landing Mirror | Synchronize identically with `index.html` |
| [login.html](file:///c:/projects/TripBoard/login.html) | Authentication | Add Tailwind CDN; update header logo & brand lockup; update footer |
| [admin.html](file:///c:/projects/TripBoard/admin.html) | Admin Dashboard | Add Tailwind CDN; replace `.dashboard-header` with Tailwind Admin Header (Active: `Dashboard`); replace footer |
| [employees.html](file:///c:/projects/TripBoard/employees.html) | Admin Employees | Replace header (Active: `Employees`); replace footer |
| [trips.html](file:///c:/projects/TripBoard/trips.html) | Admin Trips | Replace header (Active: `Trips`); replace footer |
| [add-employee.html](file:///c:/projects/TripBoard/add-employee.html) | Admin Add Employee | Replace header (Active: `Employees`); replace footer |
| [employee-details.html](file:///c:/projects/TripBoard/employee-details.html) | Admin Employee Details | Replace header (Active: `Employees`); replace footer |
| [create-trip.html](file:///c:/projects/TripBoard/create-trip.html) | Admin Create Trip | Replace header (Active: `Trips`); replace footer |
| [trip-details.html](file:///c:/projects/TripBoard/trip-details.html) | Admin Trip Details | Replace header (Active: `Trips`); replace footer |
| [employee-dashboard.html](file:///c:/projects/TripBoard/employee-dashboard.html) | Employee Portal | Replace header (Active: `My Dashboard`); replace footer |
| [documents.html](file:///c:/projects/TripBoard/documents.html) | Employee Portal | Replace header (Active: `My Documents`); replace footer |

---

## 5. Milestone Breakdown

- **Milestone 1**: Asset Scaffolding (`Assets/` at root) & Tailwind CDN snippet readiness.
- **Milestone 2**: Public Landing Pages (`index.html`, `home.html`, `login.html`).
- **Milestone 3**: Admin Suite Headers & Footers (7 Admin pages).
- **Milestone 4**: Employee Portal Headers & Footers (2 Employee pages).
- **Milestone 5**: Full Integration Verification, Daily Dev Log (`docs/logs/2026-09-30.md`), and Git Commit.

---

## 6. Verification Plan

1. **Asset Integrity**:
   - Verify `Assets/logo_svg.svg` and `Assets/bg2.png` exist and load with HTTP 200 on all pages.
2. **Visual Parity**:
   - Compare all rendered headers against [DESIGNS/shared-components-preview.html](file:///c:/projects/TripBoard/DESIGNS/shared-components-preview.html).
   - Confirm logo drop shadow, typography, active gold underlines, and button styling match exactly.
3. **Navigation & Flow Verification**:
   - `index.html` -> Login button -> `login.html`.
   - `login.html` -> Back to Home -> `index.html`.
   - Admin pages -> Logout button -> `index.html`.
   - Employee portal pages -> Logout button -> `index.html`.
4. **Mobile & Desktop Responsiveness**:
   - Test desktop layout (horizontal navigation, brand lockup).
   - Test mobile layout (flex-col wrap, readable text, no horizontal overflow).

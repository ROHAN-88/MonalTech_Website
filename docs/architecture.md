# MonalTech Frontend Architecture

This document describes the high-level architecture of the MonalTech frontend, how the main pieces fit together, and where to look when you want to change or extend behavior.

## Overview

The MonalTech frontend is a single-page React application built with Vite and TypeScript. It follows a fairly standard structure:

- `main.tsx` bootstraps the React app and wraps it in the global `ThemeProvider`.
- `App.tsx` configures client-side routing, global layout, and scroll animations (AOS).
- Route-level pages live under `src/pages/` and compose smaller presentational components.
- Shared layout and visual sections live under `src/components/`.
- More specialized, page-specific building blocks live under `src/apps/`.

## Entry Point & Bootstrapping

- **File:** `src/main.tsx`
- Responsibilities:
  - Create the React root.
  - Wrap the app in `React.StrictMode`.
  - Wrap the app in `ThemeProvider` (from `src/components/theme-provider.tsx`).
  - Import global styles from `src/index.css`.

```tsx
import React from "react";
import ReactDOM from "react-dom/client";

import { ThemeProvider } from "@/components/theme-provider";
import App from "./App";

import "./index.css";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </React.StrictMode>,
);
```

## Routing & Pages

- **File:** `src/App.tsx`
- Libraries: `react-router-dom`, `aos`

`App.tsx` wires up the top-level router:

- Uses `BrowserRouter` as `Router`.
- Wraps the route tree with `UserContextProvider` (from `src/UserContext.tsx`).
- Declares all major routes using `Routes` and `Route`.
- Initializes AOS animations inside `useEffect`.

Defined routes include:

- `/` → `IndexPage` (landing page content).
- `/about-us` → `AboutUs`.
- `/services` → `Services`.
- `/products` → `Products`.
- `/contact-us` → `ContactUs`.
- `/partner` → `Partner`.
- `/fallback` → `Fallback` (placeholder for blog/terms/policies).
- `*` → `Notfound`.

Each of these route components lives under `src/pages/` and usually composes smaller components from `src/components/` or `src/apps/`.

### Layout

- **File:** `src/Layout.tsx`

The layout component (not shown here in full) typically handles:

- Rendering the shared `Navbar` at the top.
- Rendering route content using `Outlet` from `react-router-dom`.
- Optionally rendering a shared `Footer`.

## Components

### High-level Sections

Directory: `src/components/`

Key components include:

- `HomePage.tsx` – Composes the main landing page sections in order: `Hero`, `Sponsors`, `About`, `HowItWorks`, `Features`, `Services`, `Cta`, `Testimonials`, `Team`, `Pricing`, `Newsletter`, `FAQ`, `Footer`, and `ScrollToTop`.
- `Navbar.tsx` – Top navigation bar with logo, links, mobile drawer, and theme toggle.
- Section components like `Hero.tsx`, `About.tsx`, `Features.tsx`, `Pricing.tsx`, `FAQ.tsx`, `Team.tsx`, `Sponsors.tsx`, `Testimonials.tsx`, `Cta.tsx`, `Services.tsx`, `Footer.tsx`, etc.
- Utility components like `ScrollToTop.tsx`.

These components are mostly presentational and receive data either directly (inline arrays) or from small helper modules.

### UI Primitives (`ui/*`)

Directory: `src/components/ui/`

Implements reusable building blocks, often wrapping Radix UI primitives and adding Tailwind styles:

- `button.tsx` – Button variants (`primary`, `ghost`, etc.) used throughout the app (including navigation, CTAs, etc.).
- `card.tsx`, `badge.tsx`, `input.tsx`, etc. – Styled wrappers around basic HTML elements.
- `navigation-menu.tsx` – Navigation menu primitives used by `Navbar`.
- `dropdown-menu.tsx`, `sheet.tsx`, `accordion.tsx`, `avatar.tsx`, etc. – Abstracted Radix components.

These components centralize styling and behavior so that visual changes can be made in one place.

### Theming

- **Files:**
  - `src/components/theme-provider.tsx`
  - `src/components/mode-toggle.tsx`
  - `tailwind.config.js`

Key ideas:

- Tailwind is configured with `darkMode: ["class"]`.
- Custom CSS variables (`--background`, `--foreground`, etc.) define the theme palette.
- `ThemeProvider` handles the current theme (light/dark) and exposes it via context.
- `ModeToggle` toggles the theme using the context.
- `Navbar` and other components use `useTheme()` to pick appropriate assets (e.g., light vs dark logo variants).

## About Page Building Blocks

- **Directory:** `src/apps/aboutComponent/`

Contains granular components used to build the About page:

- `Tag.tsx` – Intro/summary block.
- `Services.tsx` – Services view for the About page.
- `Vision.tsx` – Company vision.
- `Mission.tsx` – Company mission.

These are composed in `src/pages/AboutUs.tsx` using a CSS grid layout.

## Partners

- **Directory:** `src/apps/partner/`
- **Types:** `src/datatype/partnerType.ts`

Partner-related components handle rendering a grid/list of partners using shared types and CSS modules:

- `PartnerArray.tsx` – Likely holds partner data or helpers.
- `PartnerCard.tsx` – Renders a single partner entry (logo, name, description, etc.).
- `Partner.module.css` – Styling specific to partner layouts.

The `Partner` page in `src/pages/Partner.tsx` composes these pieces.

## Styling

### Tailwind Configuration

- **File:** `tailwind.config.js`

Highlights:

- `content` paths cover pages, components, app, and the entire `src` directory.
- `theme.container` is configured for centered content with 1.5rem padding and a max width at `2xl`.
- Custom `colors` map Tailwind color tokens to CSS variables and define a primary color (`#DAA520`).
- Custom border radii and accordion animations are defined.
- `tailwindcss-animate` is registered as a plugin.

### Global Styles & CSS Modules

- `src/index.css` – Global base styles, Tailwind directives, font settings, etc.
- `src/App.css` – App-level overrides.
- `*.module.css` – Scoped styles for specific components (e.g., `AboutImage.module.css`, `ContactForm.module.css`, `Partner.module.css`).

Use Tailwind utility classes for most layout/spacing/typography and CSS modules for complex layouts or third-party styles.

## Animations (AOS)

- **File:** `src/App.tsx`
- Library: `aos`

`AOS.init()` is called inside a `useEffect` when `App` mounts. Components that should animate on scroll add appropriate `data-aos` attributes. The AOS CSS is imported globally (`import "aos/dist/aos.css";`).

## Data & Utilities

- **Types:** `src/datatype/partnerType.ts` – TypeScript types for partners and other shared entities.
- **Utilities:** `src/lib/utils.ts` – Generic helper functions (for example, class name merging, formatting helpers, etc.).

Using a central `lib` directory helps keep helper logic de-duplicated across components.

## Configuration

- `vite.config.ts` – Vite config with:
  - React plugin.
  - `@` alias pointing to `./src` for cleaner imports.
  - `server` and `preview` both bound to `0.0.0.0:3000` for local and containerized environments.
- `tsconfig.json` – TypeScript compiler options and path configuration.
- `.eslintrc.*`, `.prettierrc.*` (if present) – Linting and formatting rules.

## Recommended Extension Points

When adding new functionality, consider:

- **New pages** – Add a new component under `src/pages/` and mount it in `App.tsx` routes. Optionally add a link in `Navbar.tsx`.
- **New sections on the landing page** – Create a new presentational component under `src/components/` and add it to `HomePage.tsx`.
- **New UI primitives** – Add to `src/components/ui/` and reuse across pages.
- **New partner-like entities** – Add appropriate types to `src/datatype/`, helper arrays/components under `src/apps/`, and import into the relevant page.
- **New themes or brand variants** – Adjust `tailwind.config.js` colors and `theme-provider` implementation, then consume via Tailwind classes and `useTheme()`.

## Development Workflow

1. Start the dev server with `npm run dev`.
2. Make code changes in `src/*`.
3. Run `npm run lint` before committing.
4. Run `npm run build && npm run preview` to validate production behavior when making large UI or configuration changes.

Keeping changes localized (pages vs components vs UI primitives) will help maintainability as the project grows.

# MonalTech Frontend

This repository contains the source code for the MonalTech marketing/portfolio website. It is a modern, responsive single-page application built with React, TypeScript, Vite, Tailwind CSS, and a set of reusable UI components.

## Tech Stack

- **Framework:** React 18 + TypeScript
- **Bundler/Dev Server:** Vite 5
- **Styling:** Tailwind CSS, custom CSS modules
- **UI Primitives:** Radix UI + custom `ui/*` components
- **Routing:** React Router DOM v6
- **Animations:** AOS (Animate On Scroll)
- **Forms & Validation:** Formik + Yup
- **HTTP Client:** Axios

## Features

- Fully responsive layout optimized for desktop and mobile
- Light/dark theme toggle using a shared theme provider
- Modular page structure (`/about-us`, `/services`, `/products`, `/contact-us`, `/partner`, etc.)
- Reusable component sections (Hero, Features, Pricing, FAQ, Testimonials, Team, Sponsors, CTA, Footer, etc.)
- Partner showcase and reusable partner card types
- Scroll-based animations powered by AOS
- Developer-friendly structure with TypeScript, ESLint, Prettier, and Husky + lint-staged

## Getting Started

### Prerequisites

- Node.js **18+** (required for Vite 5)
- npm (recommended) or another Node package manager (yarn, pnpm)

### Installation

1. **Clone the repository**

   ```bash
   git clone <YOUR_REPOSITORY_URL>
   ```

2. **Change into the project directory**

   ```bash
   cd MonalTech_Frontend
   ```

3. **Install dependencies**

   ```bash
   npm install
   ```

### Development

Start the local development server:

```bash
npm run dev
```

By default Vite runs on [http://localhost:5173](http://localhost:5173). A variant with explicit host binding is also available:

```bash
npm run dev-host
```

### Build

Create a production build:

```bash
npm run build
```

The compiled assets will be output to the `dist` directory.

### Preview

Preview the production build locally using Vite's preview server:

```bash
npm run build
npm run preview
```

This uses the host and port configuration from `vite.config.ts` (defaults to `0.0.0.0:3000`).

### Linting & Formatting

Run ESLint across the codebase:

```bash
npm run lint
```

Automatically fix lint issues where possible:

```bash
npm run lint:fix
```

Husky and lint-staged are configured to run linting on staged files during commits.

## Project Structure

The most important directories are:

- `src/main.tsx` – React entry point that mounts the app and wraps it with the theme provider.
- `src/App.tsx` – Root component configuring React Router routes and AOS animations.
- `src/pages/` – Route-level pages such as `AboutUs`, `Services`, `Products`, `ContactUs`, `Partner`, `Fallback`, `Notfound`.
- `src/components/` – Reusable UI sections (Hero, Features, Pricing, FAQ, Team, Navbar, Footer, etc.) and base `ui/*` components built on Radix UI.
- `src/apps/aboutComponent/` – About page building blocks: `Mission`, `Vision`, `Services`, `Tag`.
- `src/assets/` – Static assets (logos, images, icons, animations).
- `src/datatype/` – Shared TypeScript types (e.g. partner types).
- `src/lib/` – Utility functions.
- `tailwind.config.js` – Tailwind theme configuration and design tokens.
- `vite.config.ts` – Vite configuration, including the `@` path alias.

For a deeper explanation of how pages, components, UI primitives, and theming work together, see `docs/architecture.md`.

## Theming & Dark Mode

The app uses a custom `ThemeProvider` (`src/components/theme-provider.tsx`) together with Tailwind's `dark` mode and CSS variables. The navbar and other UI components react to the current theme, switching between light and dark logo variants and colors.

## Routing

Client-side routing is handled via React Router DOM (`BrowserRouter`, `Routes`, `Route` in `src/App.tsx`).

Key routes include:

- `/` – Landing page (index) composed from sections in `HomePage`.
- `/about-us` – About page built from `apps/aboutComponent/*` blocks.
- `/services` – Services overview page.
- `/products` – Products overview page.
- `/contact-us` – Contact page with form and contact information.
- `/partner` – Partner-related content.
- `/fallback` – Placeholder page for routes such as Blog/Terms/Policies.
- Catch-all (`*`) – Not-found page.

## Contributing

1. Fork or clone the repository.
2. Create a new feature branch.
3. Run `npm run lint` and ensure there are no linting errors.
4. Open a pull request describing your changes.

## Documentation

Additional technical documentation lives under the `docs/` directory:

- `docs/architecture.md` – High-level architecture, folder structure, and implementation details.

If you add new major features or pages, please update both this `README.md` and the relevant document under `docs/`.

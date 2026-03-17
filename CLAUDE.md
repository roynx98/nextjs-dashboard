# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Random
Me llamo Pepe

## Commands

```bash
pnpm dev        # Start dev server with Turbopack
pnpm build      # Production build
pnpm start      # Start production server
```

There are no lint or test scripts configured.

To seed the database, run the dev server and visit `http://localhost:3000/seed`.

## Architecture

This is a Next.js App Router project following the [Next.js Learn course](https://nextjs.org/learn), building an invoice management dashboard with a Neon PostgreSQL backend.

### Data Layer

- **No ORM** — raw SQL via the `postgres` npm package with parameterized template literals
- All database query functions live in `app/lib/data.ts`
- Types are defined manually in `app/lib/definitions.ts` (not generated)
- Database schema + seeding logic is in `app/seed/route.ts` (accessible via GET `/seed`)
- Some queries include artificial `setTimeout` delays for demo/streaming purposes

### Routing & Rendering

- `/` — Public landing page
- `/login` — Auth page
- `/dashboard/(overview)` — Main dashboard (route group, no URL segment)
- `/dashboard/invoices` — Invoice management with search, pagination, CRUD
- `/dashboard/customers` — Customer listing

Dashboard routes are protected via Next.js middleware (auth via `next-auth` v5 beta).

### Key Patterns

- **Async Server Components** fetch data directly (no API layer needed for server-rendered pages)
- **Parallel data fetching** with `Promise.all()` — see `fetchCardData()` in `data.ts`
- **Suspense boundaries** with skeleton components for streaming — skeletons are in `app/ui/skeletons.tsx`
- **`loading.tsx` files** provide automatic loading UI per route segment
- **Zod** for form validation in server actions (`app/lib/actions.ts`)
- **`@/*`** path alias maps to the project root

### Styling

- Tailwind CSS with `@tailwindcss/forms` plugin
- Custom config includes `grid-cols-13` (used by revenue chart) and shimmer keyframe animation for skeleton loaders
- Fonts: Inter (body), Lusitana (headings) — loaded via `next/font/google` in `app/ui/fonts.ts`
- `clsx` is used for conditional class merging

# Centre Point Hospitality — Leads CRM (Frontend)

React 18 + Vite single-page app for the CPH Leads CRM. Pure lead-management:
lead tracking, follow-ups, RBAC, dashboards, and an audit trail.

## Stack

- React 18 + Vite (plain JSX)
- Tailwind CSS + ShadCN-style components on Radix primitives
- react-router-dom v6, axios, react-hook-form + zod
- recharts, lucide-react, sonner (toasts), date-fns

## Getting started

```bash
npm install
npm run dev
```

The dev server runs on http://localhost:5173 and proxies all `/api` requests
to the backend at **http://localhost:5000**. Start the backend first
(`cd ../backend && npm run dev`) and seed it (`npm run seed`) for demo data.

### Seeded credentials

- Admin: `admin@cph.local` / `Admin@123`
- Sales exec: `ravi@cph.local` / `Exec@123`
- Sales exec: `neha@cph.local` / `Exec@123`

## Scripts

- `npm run dev` — start the Vite dev server (with `/api` proxy)
- `npm run build` — production build to `dist/`
- `npm run preview` — preview the production build

## Project layout

- `src/lib/` — axios instance (`api.js`), `cn()` helper, date/currency formatters
- `src/context/` — `AuthContext` (auth + token refresh), `ThemeContext` (dark mode)
- `src/components/ui/` — ShadCN-style primitives
- `src/components/layout/` — `AppLayout`, `Sidebar`, `Topbar`, `ProtectedRoute`
- `src/pages/` — route pages

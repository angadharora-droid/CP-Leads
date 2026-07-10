# Centre Point Hospitality — Leads CRM

A lead-management platform for Centre Point Hospitality. Capture and track leads through a
sales pipeline, manage follow-ups / action points / instructions, with role-based access,
dashboards, and a full audit trail.

> Pure lead management — no kit generation, no rate master, no PDFs, no email.

## Stack
- **Frontend:** React 18 + Vite, Tailwind CSS, ShadCN-style UI (Radix), React Hook Form + Zod, Recharts
- **Backend:** Node.js + Express (services layer, Zod validation, Helmet, rate limiting)
- **Database:** MongoDB + Mongoose
- **Auth:** JWT access token (15 min) + rotating refresh token (httpOnly cookie, 7 days) with reuse detection

## Project layout
```
Leads CP/
  backend/    Express + Mongoose API
  frontend/   React + Vite client
```

## Prerequisites
- Node.js 18+ (tested on 25)
- A local MongoDB running at `mongodb://127.0.0.1:27017` (or set `MONGODB_URI` in `backend/.env`)

## Run it (two terminals)

**1. Backend**
```bash
cd backend
npm install
npm run seed     # creates demo users + sample leads
npm run dev      # http://localhost:5000
```

**2. Frontend**
```bash
cd frontend
npm install
npm run dev      # http://localhost:5173  (proxies /api -> backend :5000)
```

Open http://localhost:5173.

## Demo credentials (from `npm run seed`)
| Role           | Email            | Password   |
| -------------- | ---------------- | ---------- |
| Admin          | admin@cph.local  | Admin@123  |
| Sales Exec     | ravi@cph.local   | Exec@123   |
| Sales Exec     | neha@cph.local   | Exec@123   |

## Roles
- **admin** — sees all leads; manages users, the lead tracker, and audit logs.
- **sales_exec** — sees only their own assigned leads.

## Lead reference
Every lead gets an auto reference: `CPH-[CITY]-[DDMMYY]-[###]` (e.g. `CPH-MUMBAI-300626-001`).

## Pipeline stages
`New → Contacted → Qualified → Proposal → Negotiation → Won / Lost`

# Centre Point Hospitality — Leads CRM

A lead-management platform for Centre Point Hospitality. Capture and track leads through a
sales pipeline, manage follow-ups / action points / instructions, with role-based access,
dashboards, and a full audit trail.

## Kits (proposals & contracts)

Each lead can have **kits** — the documents sent to a client:

- **Event Kit** — generates a *Proposal* and a *Confirmation Contract* PDF (guest/function
  info, billing, room requirements, event & meal details, session timings, estimated revenue).
  A contract number is auto-assigned.
- **Corporate Rate Kit** — generates the corporate room-rate agreement letter (rate tables
  per property plus the standard terms).

Flow: create the kit on the lead → fill the details → download or **email the PDF** to the
client (SMTP must be configured, see below) → once signed, **upload the signed confirmation**
(photos or PDF, stored in MongoDB GridFS). Uploading flips the kit to *confirmed* and the
lead to *Contracted*.

Emailing requires SMTP settings in `backend/.env` (see `backend/.env.example`):
`SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS`, and optionally `SMTP_SECURE` / `MAIL_FROM`.
For Gmail, use an [App Password](https://myaccount.google.com/apppasswords), not the account password.

## Stack
- **Frontend:** React 18 + Vite, Tailwind CSS, ShadCN-style UI (Radix), React Hook Form + Zod, Recharts
- **Backend:** Node.js + Express (services layer, Zod validation, Helmet, rate limiting), pdfmake (PDF generation), Nodemailer (email), Multer + GridFS (signed-confirmation uploads)
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
npm run seed     # creates the admin user
npm run dev      # http://localhost:5000
```

**2. Frontend**
```bash
cd frontend
npm install
npm run dev      # http://localhost:5173  (proxies /api -> backend :5000)
```

Open http://localhost:5173.

## Default credentials (from `npm run seed`)
| Role  | Email           | Password  |
| ----- | --------------- | --------- |
| Admin | admin@cph.local | Admin@123 |

Override with `ADMIN_EMAIL`, `ADMIN_PASSWORD` (and optionally `ADMIN_NAME`) env vars
when seeding. Sales executive accounts are created by the admin from the Users page.

## Roles
- **admin** — sees all leads; manages users, the lead tracker, and audit logs.
- **sales_exec** — sees only their own assigned leads.

## Lead reference
Every lead gets an auto reference: `CPH-[CITY]-[DDMMYY]-[###]` (e.g. `CPH-MUMBAI-300626-001`).

## Pipeline stages
`New → Contacted → Qualified → Proposal → Negotiation → Won / Lost`

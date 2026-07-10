# CPH Leads CRM — Backend

Lead-management API for Centre Point Hospitality. Pure lead tracking: leads,
follow-ups, action points, instructions, RBAC, dashboards, and an audit trail.

## Stack

Node (ESM, Node 18+) · Express · Mongoose · JWT (access + rotating refresh) ·
bcryptjs · zod · helmet · cors · express-rate-limit · morgan.

## Prerequisites

- Node.js 18 or newer.
- A running local MongoDB instance (default `mongodb://127.0.0.1:27017/cph_leads_crm`).

## Setup & run

```bash
cd backend
npm install            # install dependencies
# ensure MongoDB is running locally
npm run seed           # wipe + seed users and sample leads
npm run dev            # start with nodemon (auto-reload)
# or
npm start              # start once with node
```

The API listens on `http://localhost:5000` by default. Health check:
`GET http://localhost:5000/api/health`.

## Environment

Configuration lives in `backend/.env` (already filled with working dev values so
the app runs out of the box). See `.env.example` for the full key list:

| Key | Default | Notes |
| --- | --- | --- |
| `NODE_ENV` | `development` | |
| `PORT` | `5000` | API port |
| `MONGODB_URI` | `mongodb://127.0.0.1:27017/cph_leads_crm` | Mongo connection string |
| `JWT_ACCESS_SECRET` | dev secret | change in production |
| `JWT_REFRESH_SECRET` | dev secret | change in production |
| `ACCESS_TOKEN_TTL` | `15m` | access-token lifetime |
| `REFRESH_TOKEN_TTL_DAYS` | `7` | refresh-cookie lifetime |
| `BCRYPT_ROUNDS` | `10` | password hash cost |
| `CLIENT_ORIGIN` | `http://localhost:5173` | CORS origin for the frontend |

## Seed credentials

After `npm run seed` you can log in with:

| Role | Email | Password |
| --- | --- | --- |
| Admin | `admin@cph.local` | `Admin@123` |
| Sales Exec | `ravi@cph.local` | `Exec@123` |
| Sales Exec | `neha@cph.local` | `Exec@123` |

## API surface

All routes are mounted under `/api`. JSON in/out. Auth via `Authorization:
Bearer <accessToken>` header; refresh handled via the httpOnly `cph_rt` cookie.

- `/api/auth` — login, refresh, logout, change-password, me
- `/api/users` — admin user management
- `/api/leads` — leads, notes, action points, follow-ups, instructions
- `/api/follow-ups` — personal follow-up + instruction queue
- `/api/dashboard` — admin and personal dashboards
- `/api/audit` — admin audit log

### Response envelope

```jsonc
// success
{ "success": true, "data": { /* ... */ } }
// error
{ "success": false, "error": { "message": "...", "code": "...", "details": null } }
```

# TRIMURYA CALLING CRM

Commercial multi-tenant CRM foundation: React/Vite web workspace, Express/Mongo API, and Kotlin Android agent shell.

## Run locally

1. Copy `.env.example` to `backend/.env`, and `frontend/.env.example` to `frontend/.env`.
   Set `MONGODB_URI`, JWT secrets, `WEB_ORIGIN`, and `VITE_API_URL` for your environment.
2. Start MongoDB locally or set `MONGODB_URI` to a managed cluster.
3. From the project root run `npm.cmd install`, `npm.cmd run seed -w backend`, then `npm.cmd run dev`. From the `backend` folder use `seed.cmd` and `start.cmd` instead.
4. Open `http://localhost:5173`.

## Demo login credentials

Run the seed command above first. All demo accounts use password: `Trimurya@123`.

| Role | Email | Company ID |
| --- | --- | --- |
| Super Admin | `superadmin@trimurya.local` | Leave blank |
| Super Admin | `superadmin@trimurya.local` | Trimurya@123 |
| Company Admin | `admin@acme.trimurya.local` | `TC-ACME-001` |
| Manager | `manager@acme.trimurya.local` | `TC-ACME-001` |
| Team Leader | `teamlead@acme.trimurya.local` | `TC-ACME-001` |
| Calling Agent | `agent@acme.trimurya.local` | `TC-ACME-001` |

The Android calling app uses only Agent ID and password: `ACME-AGENT-001` / `Trimurya@123`.

## Manager-to-agent automatic calling

A manager can create a call assignment with `POST /api/call-queue` (agent ID, customer name, phone and optional note). The agent app reads the next assignment from `GET /api/agent/next-call` and places the customer call after the Android phone permission has been granted. Call completion is recorded through `POST /api/agent/call-queue/:id/complete`.

These are local development credentials only. Change the password and remove/replace demo users before deploying.

The seed command creates an active demo company, license, five roles, two assigned leads and a synced call. It is idempotent, so running it again updates the sample records instead of duplicating them. New companies created through `POST /api/admin/companies` still begin with a `pending` license until payment approval activates it.

## Current vertical slice

- JWT access/refresh rotation, RBAC, license gate, API rate limiting and tenant-scoped leads/calls.
- Super Admin company provisioning endpoint.
- Company dashboard plus lead management and call history UI.
- Android dialer shell that uses the visible OS dialer only.

## Runtime configuration

All deployment-specific values are environment configuration; no MongoDB shard host, API URL, JWT secret, or CORS origin is embedded in application code.

- `backend/.env`: database URI, JWT secrets, server port, allowed web origins, and MongoDB retry/timeouts.
- `frontend/.env`: `VITE_API_URL`, the API base URL used by the web app.

## Production work still required

Razorpay webhook signature verification, R2 upload/signed playback endpoints, BullMQ jobs, complete billing/invoice/reporting modules, audit records, CSV import/export, Android token storage/sync workers, FCM, integration tests and deployment IaC are deliberately not represented as complete merely by UI. They need configured credentials, legal recording policy, and provider selection.
"# agentcallingscrm" 

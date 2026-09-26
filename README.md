# Project-Automtaion-plan-
# SheetFlow — Automation Engine

Google Sheet-a config/database maari use panni, row/column data based-la actions automatic-a execute pannura scheduling automation platform.

> Sheet-centric mini Zapier/Make. Google Sheets → Variable Resolver → Action Engine → WhatsApp / HTTP / Webhook.

---

## 🚦 Status at a glance

| # | Stage | Status | Notes |
|---|-------|--------|-------|
| 1 | Concept + architecture doc | ✅ Done | Full doc written (this repo) |
| 2 | Landing page / pitch (`index.html`) | ✅ Done | Static concept preview, published |
| 3 | Project scaffold (`apps/`, `services/`, `packages/`) | ⬜ Balance | Not started |
| 4 | Google OAuth 2.0 + Sheets API connect | ⬜ Balance | Core dependency for everything else |
| 5 | Dynamic column/header detection | ⬜ Balance | Depends on #4 |
| 6 | Variable Resolver engine (`{{Name}}` etc.) | 🟡 Prototyped | Logic proven in landing page demo (client-side only) — needs real backend version |
| 7 | Automation Builder (trigger/condition/action config) | ⬜ Balance | UI + schema not started |
| 8 | Scheduler (Redis + BullMQ) | ⬜ Balance | Design finalized, no code |
| 9 | Action Engine (plugin architecture) | ⬜ Balance | Interface not defined yet |
| 10 | WhatsApp action (Baileys) | ⬜ Balance | Needs session storage design first |
| 11 | HTTP / cURL action | ⬜ Balance | Simplest action, good first build |
| 12 | Webhook action (incoming + outgoing) | ⬜ Balance | — |
| 13 | Retry system | ⬜ Balance | Depends on #8 |
| 14 | Execution logs + dashboard stats | ⬜ Balance | Depends on #9–13 |
| 15 | Database schema (Postgres/Prisma) | ⬜ Balance | Schema drafted in doc, not migrated |
| 16 | Security layer (token encryption, rate limit, audit log) | ⬜ Balance | Critical — do alongside #4, not after |
| 17 | V2 — non-Sheet sources (MySQL, REST API) | ⬜ Future | After V1 stable |

**Legend:** ✅ Done · 🟡 Prototyped (demo-only, not production) · ⬜ Balance (not started)

---

## 🏗️ Architecture

```
Dashboard (Next.js)
      │
      ▼
Automation API (Node.js/TS)
      │
   ┌──┼──────────────┐
   ▼  ▼              ▼
Sheets  Scheduler   Action Engine
API    (Redis/BullMQ)    │
   │                 ┌───┼────┐
   ▼                 ▼   ▼    ▼
Row Data Parser   WhatsApp cURL Webhook
   │
   ▼
Variable Resolver ({{Name}}, {{Phone}}, {{Message}})
```

---

## ✅ What's actually built right now

- `index.html` — self-contained concept/pitch page:
  - Animated sheet row (Pending → Sent) demo
  - Live variable-resolver playground (client-side JS, no backend)
  - Pipeline, actions, triggers, retry-timeline, architecture sections

That's it — **no backend, no OAuth, no scheduler, no real WhatsApp/HTTP/webhook execution exist yet.** Everything below "Landing page" in the status table is the actual build.

---

## 🔜 Suggested build order (balance work)

1. **Auth + Sheets connect** — Google OAuth 2.0, encrypted token storage, spreadsheet/tab picker
2. **Header detection + Variable Resolver (real)** — parse sheet headers → `{{var}}` map, port the landing-page logic into the API
3. **HTTP/cURL action** — easiest action to prove the engine works end-to-end
4. **Scheduler (BullMQ + Redis)** — polling loop → job queue → worker
5. **Update-sheet action** — write `Status`, `Last_Run` back to the row
6. **Retry system** — wrap action execution with backoff
7. **WhatsApp (Baileys)** — session management, riskiest module, do last
8. **Logs + dashboard** — once real executions exist to show
9. **Security pass** — token encryption, rate limiting, webhook signature validation, secret masking (don't skip this before going live)

---

## 🧱 Tech stack

- **Frontend:** React / Next.js
- **Backend:** Node.js / TypeScript
- **Queue:** Redis + BullMQ
- **DB:** PostgreSQL + Prisma
- **WhatsApp:** Baileys (WhatsApp Web protocol — check ToS/account-risk before production)
- **Auth:** Google OAuth 2.0 + Sheets API + Drive API

---

## 📁 Planned structure

```
sheetflow/
├── apps/
│   ├── dashboard/      → Next.js
│   └── api/            → Node.js
├── services/
│   ├── scheduler/
│   ├── worker/
│   ├── whatsapp/
│   ├── google-sheets/
│   └── automation-engine/
├── packages/
│   ├── template-engine/
│   ├── types/
│   └── logger/
├── database/
│   └── prisma/
└── docker/
```

# Fuel EU Maritime — Full-Stack Assignment

Minimal **Fuel EU Maritime compliance** module: React + TypeScript + Tailwind dashboard and Node + TypeScript + PostgreSQL APIs for routes, compliance balance (CB), **Article 20 banking**, and **Article 21 pooling**. Layout follows **hexagonal (ports & adapters)** on both sides.

## Repository layout

| Path | Role |
|------|------|
| `frontend/` | React UI, domain types, `FuelApiPort`, HTTP adapter, tabs (Routes, Compare, Banking, Pooling) |
| `backend/` | Express HTTP adapter, Prisma PostgreSQL adapter, domain formulas & use-cases |
| `docker-compose.yml` | Local PostgreSQL 16 |
| `AGENT_WORKFLOW.md` | Required AI-agent usage log |
| `REFLECTION.md` | Short reflection on using agents |

## Architecture (hexagonal)

**Backend**

- **Core:** `src/core/domain` (pure formulas), `src/core/application` (use-cases), `src/core/ports` (repository interfaces).
- **Inbound:** `src/adapters/inbound/http` — Express app factory `createApp`.
- **Outbound:** `src/adapters/outbound/postgres` — Prisma implementations.
- **Infrastructure:** `src/infrastructure/server` — process entrypoint.

**Frontend**

- **Core:** `src/core/domain` (DTO types), `src/core/ports` (`FuelApiPort`).
- **Inbound UI:** `src/adapters/ui` — React tabs (implement user interaction).
- **Outbound:** `src/adapters/infrastructure/http-fuel-api.ts` — `fetch` client.

Frameworks (Express, Prisma, React) sit in adapters only; core has no React/Express imports.

## Prerequisites

- Node.js 20+
- Docker (for Postgres) or any PostgreSQL instance

## Setup & run

### Database

From the repo root:

```bash
docker compose up -d
```

Copy env and migrate:

```bash
cd backend
cp .env.example .env
npm install
npx prisma generate
npx prisma migrate deploy
npx prisma db seed
npm run dev
```

API default: `http://localhost:4000` (override with `PORT`).

### Frontend

```bash
cd frontend
cp .env.example .env
npm install
npm run dev
```

Open the URL Vite prints (default `http://localhost:5173`). Set `VITE_API_URL` if the API is not on `http://localhost:4000`.

## Tests

**Backend** (unit + HTTP integration with in-memory repositories):

```bash
cd backend && npm install && npm run test
```

**Frontend** (sample unit test):

```bash
cd frontend && npm install && npm run test
```

## Sample API responses

**`GET /routes`** — list of route rows (camelCase) including `routeId`, `ghgIntensity`, etc.

**`GET /routes/comparison`** — `baseline`, `comparisons[]` with `percentDiff` and `compliant`, plus `targetIntensity` **89.3368**.

**`POST /banking/bank`** — `{ cbBefore, banked, cbAfter, fleetBankBalance }`.

**`POST /pools`** — `{ poolId, members: [{ shipId, cbBefore, cbAfter }] }`.

## Screenshots

After `npm run dev` for both apps, capture the four tabs (Routes, Compare, Banking, Pooling) for your submission pack.

## Reference

Regulatory context: Fuel EU Maritime Regulation [(EU) 2023/1805](https://eur-lex.europa.eu/) — Annex IV intensity; Articles 20–21 for banking and pooling.

# AI Agent Workflow Log

This document records how AI-assisted tools were used for the Fuel EU Maritime assignment (Cursor Agent, inline assistants, etc.).

## Agents Used

- **Cursor Agent** (this environment): scaffolding, full backend/frontend implementation, tests, and documentation.
- **IDE inline completion** (optional): small typings and import fixes while editing.

## Prompts & Outputs

### Example 1 — Exact prompt and generated snippet

**Prompt (paraphrased):** “Complete the Fuel EU full-stack assignment from the PDF: hexagonal backend/frontend, PostgreSQL, four dashboard tabs, banking/pooling APIs, and mandatory markdown docs.”

**Generated output (summary):** A monorepo scaffold with:

- `backend/`: Prisma schema, migrations, seed, `createApp` Express routes, domain formulas for CB and pooling, Vitest unit tests and Supertest HTTP tests with in-memory repositories.
- `frontend/`: Vite + React + Tailwind, `FuelApiPort` + `HttpFuelApi`, four tabs wired to the API.

### Example 2 — Refining or correcting output

**Issue:** Initial banking logic used raw compliance only; the spec requires **adjusted** CB after bank entries.

**Refinement:** Updated `bank-use-cases` to compute **raw** CB from the route, then **adjusted** CB via `adjustedComplianceForShipYear` (fleet bank entries) before validating bank/apply actions.

---

## Validation / Corrections

- **Types:** TypeScript `strict` in both packages; ports/interfaces keep adapters swappable.
- **Domain:** `computeComplianceBalanceGco2eq` and `percentDiff` checked against the assignment PDF (target **89.3368** gCO₂e/MJ, energy ≈ fuel × 41 000 MJ/t).
- **Pooling:** Greedy allocation + `validatePoolRules` for deficit/surplus constraints.
- **HTTP:** Supertest smoke tests against `createApp` with in-memory repos (no DB required for CI).

## Observations

### Where the agent saved time

- Boilerplate for Prisma models, migrations, `docker-compose`, and Vite/Tailwind layout.
- Repetitive CRUD-style HTTP handlers and React table UIs.
- Initial test matrix for domain pure functions.

### Where it failed or hallucinated

- **Notion/PDF links** were not machine-readable; the brief had to be supplied as a local PDF or pasted text.
- Occasional **API shape** mismatches between first draft and the PDF (e.g. `year` vs `shipId+year`); resolved by aligning handlers with the brief and supporting optional `shipId` on list endpoints.

### How tools were combined

- **Cursor** for multi-file edits and repo-wide consistency (ports, adapters, tests).
- **Manual review** of PDF formulas and tab requirements before treating the solution as complete.

### Best practices followed

- **Hexagonal boundaries:** domain and application layers do not import Express or React.
- **Thin adapters:** HTTP and Prisma map DTOs ↔ domain types.
- **Tests at domain** and **HTTP factory** level to avoid coupling to a live DB.

# Reflection — AI agents on this assignment

**What I learned.** Using an agent for a bounded product brief (Fuel EU Maritime dashboard + APIs) works best when the requirements are fixed in text (PDF) and the architecture is decided up front (hexagonal layout, Prisma, Vite). The agent excelled at generating consistent folder structures, repeated endpoint handlers, and test scaffolds, while I still needed to verify regulatory formulas (target intensity, CB, pooling rules) against the spec.

**Efficiency vs manual coding.** The largest gains were speed: database schema + seed data, Express route wiring, React tabs with charts, and markdown deliverables. The slowest part remained **domain correctness** and **API contract alignment** with the UI, which required human judgment and a few iterative corrections.

**What I would do next time.** I would paste the full assignment into the chat first (or attach the PDF earlier) to avoid ambiguity, define OpenAPI-style request/response shapes before implementation, and add a small contract test suite so the frontend and backend stay in lockstep. I would also run a short **accessibility** pass on the tabs (focus order, live regions for errors) beyond the baseline responsive layout.

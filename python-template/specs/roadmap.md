# Roadmap: <PROJECT_NAME>

> **Note to editor:** Phased feature plan. Each feature should be implementable in 1-3 focused sessions. If bigger, split it.

## Legend

- `[ ]` not started
- `[~]` in progress (feature branch open)
- `[x]` done (merged to main)
- `[P]` parallelizable with adjacent features (different files)

---

## Phase 0 — Foundation

> **Goal:** Walking skeleton. An empty but deployed app with a healthcheck endpoint.
> **Exit criteria:** App deployed to staging, `/health` returns 200, CI pipeline green.

- [ ] **F0.1** — Create project structure (`app/routers`, `app/services`, `app/models`, `app/schemas`, `tests/`)
- [ ] **F0.2** — Add `/health` endpoint with DB connectivity check
- [ ] **F0.3** — Configure structured logging (structlog or loguru) → <sink>
- [ ] **F0.4** — CI pipeline: ruff → mypy → pytest → container build → deploy staging
- [ ] **F0.5** — Global exception handler returning RFC 7807 ProblemDetails
- [ ] **F0.6** — Database bootstrap: SQLAlchemy async engine, Alembic, first migration

---

## Phase 1 — MVP Core

> **Goal:** The thinnest end-to-end slice that proves the core value proposition.
> **Exit criteria:** <single concrete scenario that demonstrates value>

- [ ] **F1.1** — <smallest valuable feature>
- [ ] **F1.2** — <next>
- [ ] **F1.3** — <next>
- [ ] **F1.4** — Integration tests for Phase 1 endpoints using `pytest` + `httpx.AsyncClient` + testcontainers

---

## Phase 2 — MVP Polish

> **Goal:** Production-ready MVP.
> **Exit criteria:** Internal beta users can use it without hand-holding.

- [ ] **F2.1** — Authentication (OAuth2 Password Bearer + JWT via `python-jose`)
- [ ] **F2.2** — Authorization via FastAPI dependencies (role/scope checks)
- [ ] **F2.3** — Rate limiting (`slowapi`)
- [ ] **F2.4** — OpenTelemetry instrumentation (OTLP exporter)
- [ ] **F2.5** — OpenAPI tags + description polish (FastAPI auto-generates `/docs`)

---

## Phase 3 — v1.0

> **Goal:** Shippable to real customers.
> **Exit criteria:** Load tested to <target RPS>, runbook written, on-call rotation set up.

- [ ] **F3.1** — <feature>
- [ ] **F3.2** — <feature>
- [ ] **F3.3** — Performance testing & tuning

---

## Deferred / Parking Lot

Features we've discussed but won't build soon, with reason.

- **Multi-tenancy** — out of scope for v1; would require schema changes
- **Event sourcing** — overkill for current audit needs; revisit post-v1
- **GraphQL endpoint** — no consumer demand yet

---

## Lessons Learned (cross-feature)

> **Editor's note:** Features should also maintain their own `lessons-learned.md`. Only promote cross-cutting insights here.

- <YYYY-MM-DD> — <lesson>

---

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

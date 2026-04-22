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

- [ ] **F0.1** — Create solution structure (`.Api`, `.Application`, `.Domain`, `.Infrastructure`, test projects)
- [ ] **F0.2** — Add `/health` endpoint with DB connectivity check
- [ ] **F0.3** — Configure Serilog → <sink>, structured logging baseline
- [ ] **F0.4** — CI pipeline: build → test → format check → container → deploy staging
- [ ] **F0.5** — Global exception middleware returning ProblemDetails
- [ ] **F0.6** — Database bootstrap: EF Core DbContext, first migration, connection resilience

---

## Phase 1 — MVP Core

> **Goal:** The thinnest end-to-end slice that proves the core value proposition.
> **Exit criteria:** <single concrete scenario that demonstrates value>

- [ ] **F1.1** — <smallest valuable feature>
- [ ] **F1.2** — <next>
- [ ] **F1.3** — <next>
- [ ] **F1.4** — Integration tests for Phase 1 endpoints using WebApplicationFactory + Testcontainers

---

## Phase 2 — MVP Polish

> **Goal:** Production-ready MVP.
> **Exit criteria:** Internal beta users can use it without hand-holding.

- [ ] **F2.1** — Authentication (JWT Bearer)
- [ ] **F2.2** — Authorization policies
- [ ] **F2.3** — Rate limiting on write endpoints
- [ ] **F2.4** — OpenTelemetry instrumentation
- [ ] **F2.5** — OpenAPI / Swagger documentation

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

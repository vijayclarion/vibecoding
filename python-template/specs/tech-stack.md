# Tech Stack: <PROJECT_NAME>

> **Note to editor:** This file locks in HOW we build. The same mission could be built many ways — this captures our specific choices and *why*.

## Languages & Runtimes

- **Backend:** Python 3.12
- **Package manager:** uv (preferred) or pip
- **Lockfile:** `uv.lock` (or `requirements.txt` via `uv export`)
- **Frontend:** <React 18 + TypeScript / Jinja2 templates / N/A — API only>
- **Database:** <PostgreSQL 16 / SQLite (dev) / MySQL 8>
- **Cache:** <Redis 7 / in-memory / N/A>

## Key Frameworks & Libraries

| Library | Version | Why chosen |
|---|---|---|
| FastAPI | 0.11x | Modern async, auto OpenAPI, Pydantic v2 integration |
| Pydantic | 2.x | Validation, settings, DTOs |
| SQLAlchemy | 2.x (async) | ORM; async session for non-blocking I/O |
| Alembic | 1.13+ | Migrations |
| asyncpg / aiosqlite | — | Async DB drivers |
| httpx | 0.27+ | Async HTTP client (replaces `requests`) |
| structlog | 24.x | Structured logging, JSON renderer |
| pytest | 8.x | Test framework |
| pytest-asyncio | 0.23+ | Async test support |
| pytest-cov | 5.x | Coverage |
| ruff | 0.6+ | Lint + format (replaces flake8, isort, black) |
| mypy | 1.11+ | Static typing (`--strict`) |
| hypothesis | 6.x | Property-based testing (optional) |
| testcontainers | 4.x | Real Postgres in integration tests |

## Architecture

- **Pattern:** <Modular monolith / Microservices / Clean Architecture>
- **Style:** <Layered / Vertical slices / Hexagonal>
- **Boundaries:** <list bounded contexts / modules>
- **Communication:** <REST / gRPC / events via Kafka>
- **Deployment target:** <AWS ECS / Kubernetes / Azure Container Apps / Railway>

### Folder structure

```
app/
├── main.py                 ← FastAPI app factory, router wiring
├── core/
│   ├── config.py           ← Pydantic Settings (from env)
│   ├── security.py         ← JWT, password hashing
│   └── logging.py          ← structlog config
├── routers/                ← thin HTTP layer
│   └── orders.py
├── services/               ← business logic
│   └── order_service.py
├── repositories/           ← data access (no HTTP)
│   └── order_repository.py
├── models/                 ← SQLAlchemy ORM models
│   └── order.py
├── schemas/                ← Pydantic DTOs (IN/OUT)
│   └── order.py
├── dependencies.py         ← DI providers for Depends()
└── exceptions.py           ← Domain exceptions + handlers

tests/
├── conftest.py             ← fixtures (db, client, factories)
├── unit/
├── integration/
└── e2e/

migrations/                 ← Alembic
pyproject.toml
uv.lock
```

## Data Model (High-Level)

```
Customer ─< Order ─< OrderLineItem >─ Product
                 └─< Payment
                 └─< Fulfillment
```

## Request Lifecycle

For `POST /orders`:

1. Request arrives → CORS middleware → TrustedHost middleware
2. `OAuth2PasswordBearer` dependency validates JWT
3. `RequireScope("orders:write")` dependency checks scope
4. Pydantic validates request body against `CreateOrderRequest`
5. Router delegates to `OrderService.create_order(cmd, user)`
6. Service loads repo-backed aggregate, runs domain rules
7. `await session.commit()` persists; domain events published
8. Service returns `OrderCreated` DTO
9. FastAPI serializes response (with `response_model=OrderCreated`)

## Cross-Cutting Concerns

- **Authentication:** OAuth2 Password Bearer + JWT (RS256); tokens issued by <auth service>
- **Authorization:** FastAPI `Depends(require_scope("..."))`
- **Logging:** structlog → JSON → stdout; correlation id via middleware
- **Observability:** OpenTelemetry auto-instrumentation + manual spans for business logic
- **Error handling:** Global `exception_handler` for domain errors → RFC 7807 `application/problem+json`
- **Configuration:** Pydantic `BaseSettings` reading env + `.env` (dev)
- **Secrets:** <AWS Secrets Manager / Azure Key Vault / env vars>
- **Caching:** <`aiocache` with Redis backend / `functools.lru_cache` for pure functions>
- **Rate limiting:** `slowapi` or API gateway (Kong, AWS API Gateway)

## Testing Strategy

- **Unit tests:** `pytest` + `pytest-asyncio`
  - Target: ≥85% branch coverage on `services/` and `models/`
- **Integration tests:** `httpx.AsyncClient(transport=ASGITransport(app=app))` + Testcontainers Postgres
  - Every endpoint: ≥1 happy path + ≥2 error paths
- **Property-based:** `hypothesis` for invariants (optional but encouraged)
- **Performance:** `locust` or `k6` for key endpoints before release

## Quality Gates (enforced in CI)

- [ ] `ruff check .` — zero warnings
- [ ] `ruff format --check .` — formatted
- [ ] `mypy --strict .` — type-clean
- [ ] `pytest` — all green
- [ ] `pytest --cov` — ≥85% overall
- [ ] `pip-audit` or `safety check` — no known CVEs

## Non-Functional Requirements

| NFR | Target |
|---|---|
| Availability | 99.9% (43 min/month downtime budget) |
| P95 latency | < 150ms for reads, < 300ms for writes |
| Throughput | 500 RPS sustained, 2000 RPS burst |
| Recovery (RTO) | < 15 minutes |
| Recovery (RPO) | < 5 minutes |
| Security | <OWASP ASVS L2 / SOC 2> |
| Compliance | <GDPR / CCPA / none> |

## External Dependencies

- **ProductCatalog API** — https://... (internal, v2 contract)
- **Stripe** — payments
- **SendGrid** — transactional email

## Deployment

- **Environments:** `dev` → `staging` → `prod`
- **CI/CD:** <GitHub Actions / GitLab CI>
- **Pipeline:** lint → typecheck → test → container build → deploy staging → smoke → manual approval → prod
- **Container:** `python:3.12-slim` base, multi-stage with `uv`
- **ASGI server:** `uvicorn` behind `gunicorn` with `--workers $(nproc)` in prod
- **Migrations:** run as separate job before deploy

## Open Questions / Pending Decisions

- [ ] Sync vs async Alembic for migrations? (leaning sync — simpler)
- [ ] Celery or arq for background jobs?

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

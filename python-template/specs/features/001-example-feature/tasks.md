# Tasks: <FEATURE_NAME>

> **Plan:** `./plan.md`
> **Convention:** `[P]` = parallelizable with adjacent `[P]` tasks (different files, no dependency)

## Setup

- [ ] **T1** — Create feature branch: `git checkout -b feature/<NNN>-<short-name>`
- [ ] **T2** — Add packages to `pyproject.toml`: `uv add <pkg>` (if any new deps)

## Models Layer

- [ ] **T3** — Create `app/models/order.py` — `Order` SQLAlchemy model `[P]`
- [ ] **T4** — Create `app/models/order_line_item.py` — `OrderLineItem` model `[P]`

## Schemas Layer

- [ ] **T5** — Create `app/schemas/order.py` — `CreateOrderRequest`, `OrderResponse`, `CreateLineItem`

## Tests (written BEFORE service implementation)

- [ ] **T6** — Create `tests/unit/services/test_order_service.py` with failing tests:
  - happy path
  - customer not found → raises domain exception
  - empty line items → validation error (already caught by Pydantic — test as sanity check)
  - product not found → raises domain exception

## Repositories Layer

- [ ] **T7** — Create `app/repositories/order_repository.py` with `OrderRepository` (async methods: `add`, `get_by_id`)

## Services Layer

- [ ] **T8** — Create `app/services/order_service.py` with `OrderService.create_order()` — make tests pass
- [ ] **T9** — Verify tests from T6 now pass: `uv run pytest tests/unit/services/`

## Migrations

- [ ] **T10** — Run: `uv run alembic revision --autogenerate -m "add orders table"`
- [ ] **T11** — Review the generated migration for correctness
- [ ] **T12** — Apply: `uv run alembic upgrade head`

## Router Layer

- [ ] **T13** — Create `app/routers/orders.py` with `POST /orders` endpoint
- [ ] **T14** — Register router in `app/main.py`
- [ ] **T15** — Add `get_order_service` provider in `app/dependencies.py`

## Integration Tests

- [ ] **T16** — Create `tests/integration/test_orders_api.py`:
  - happy path: `POST /orders` → 201 Created + Location header
  - validation failure: empty line_items → 422
  - unauthorized: no JWT → 401
  - forbidden: JWT without scope → 403
  - referenced product not found → 422

## Observability

- [ ] **T17** — Add structured log entries at service entry/exit with correlation id
- [ ] **T18** — Emit `orders.created` counter metric on success

## Quality Gates

- [ ] **T19** — `uv run ruff check . --fix`
- [ ] **T20** — `uv run ruff format .`
- [ ] **T21** — `uv run mypy --strict .` — clean
- [ ] **T22** — `uv run pytest --cov=app --cov-report=term-missing` — all green, new code ≥85%

## Documentation

- [ ] **T23** — Update `spec.md` if anything deviated from plan
- [ ] **T24** — Add entries to `lessons-learned.md` for any surprises
- [ ] **T25** — Verify `/docs` (OpenAPI) renders correctly with tags and examples

## Wrap-up

- [ ] **T26** — Commit: `feat(orders): F<N.N> <short description>`
- [ ] **T27** — Open PR, link spec.md and plan.md
- [ ] **T28** — Merge to main after review
- [ ] **T29** — Update `roadmap.md` — mark F<N.N> as `[x]` done

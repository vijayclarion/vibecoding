# Validation: <FEATURE_NAME>

> **Spec:** `./spec.md`
> **Run this before merging.**

## Automated Checks

```bash
uv run ruff check .                     # zero warnings
uv run ruff format --check .            # formatted
uv run mypy --strict .                  # type clean
uv run pytest --cov=app                 # all green, coverage ≥85%
uv run pip-audit                        # no CVEs
```

## Acceptance Criteria Verification

Map each AC from `spec.md` to a test or manual check.

| AC | Verification | Result |
|---|---|---|
| AC1 | `test_orders_api.py::test_post_valid_order_returns_201` | ☐ |
| AC2 | `test_orders_api.py::test_post_empty_items_returns_422` | ☐ |
| AC3 | `test_orders_api.py::test_post_unknown_product_returns_422` | ☐ |
| AC4 | Manual smoke test (see below) | ☐ |

## Manual Smoke Test

Run locally:
```bash
uv run alembic upgrade head
uv run uvicorn app.main:app --reload
```

### Happy path

```bash
TOKEN="<dev JWT>"

curl -X POST http://localhost:8000/orders \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: $(uuidgen)" \
  -d '{
    "customer_id": "cust_123",
    "line_items": [{"product_id": "prod_abc", "quantity": 2}]
  }' \
  -i
```

**Expected:**
- Status: `201 Created`
- Header: `Location: /orders/<uuid>`
- Body: `{"id": "<uuid>"}`
- Log entry: JSON line with correlation id

### Error cases

```bash
# Empty line items → 422 (Pydantic validation)
curl -X POST http://localhost:8000/orders \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"customer_id": "cust_123", "line_items": []}' \
  -i

# Missing auth → 401
curl -X POST http://localhost:8000/orders \
  -H "Content-Type: application/json" \
  -d '{"customer_id": "cust_123", "line_items": [{"product_id": "prod_abc", "quantity": 1}]}' \
  -i
```

### OpenAPI check

Open `http://localhost:8000/docs` — verify:
- [ ] Endpoint appears under the correct tag
- [ ] Request/response schemas render properly
- [ ] Example payloads are present and realistic

## Non-Functional Verification

- [ ] P95 latency < 150ms under 100 RPS (measure with `locust` or `k6`)
- [ ] No memory leaks over 10-minute sustained load
- [ ] Correlation id appears in all logs for a given request
- [ ] `orders.created` metric emitted

## Observability Check

- [ ] Trace appears in <Jaeger / Tempo / Honeycomb> with all spans
- [ ] Log entry has correlation id, customer id (no PII beyond that)
- [ ] Error logs include stack trace but no secrets

## Rollback Plan

If this feature breaks prod after deploy:

1. Revert commit and redeploy previous container tag
2. If DB migration: `uv run alembic downgrade -1`
3. Post in #incidents Slack channel

## Sign-off

- [ ] Dev: ready for review
- [ ] Reviewer: code LGTM
- [ ] QA / Product owner: accepted
- [ ] Merged to main

# Validation: <FEATURE_NAME>

> **Spec:** `./spec.md`
> **Run this before merging.**

## Automated Checks

```bash
dotnet build --configuration Release    # zero warnings
dotnet test                             # all green
dotnet format --verify-no-changes       # style clean
dotnet list package --vulnerable        # no CVEs
```

## Acceptance Criteria Verification

Map each AC from `spec.md` to a test or manual check.

| AC | Verification | Result |
|---|---|---|
| AC1 | `CreateOrderEndpointTests.Post_ValidOrder_Returns201` | ☐ |
| AC2 | `CreateOrderEndpointTests.Post_InvalidPayload_Returns400` | ☐ |
| AC3 | `CreateOrderEndpointTests.Post_NonExistentCustomer_Returns422` | ☐ |
| AC4 | Manual smoke test (see below) | ☐ |

## Manual Smoke Test

Run locally with `dotnet run --project src/<PROJECT>.Api`.

### Happy path

```bash
TOKEN="<dev JWT>"

curl -X POST http://localhost:5000/orders \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: $(uuidgen)" \
  -d '{
    "customerId": "cust_123",
    "lineItems": [{"productId": "prod_abc", "quantity": 2}]
  }' \
  -i
```

**Expected:**
- Status: `201 Created`
- Header: `Location: /orders/ord_...`
- Body: `{"id": "ord_..."}`
- Log entry: `Information` with correlation id

### Error cases

```bash
# Empty line items → 400
curl -X POST http://localhost:5000/orders \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"customerId": "cust_123", "lineItems": []}' \
  -i

# Missing auth → 401
curl -X POST http://localhost:5000/orders \
  -H "Content-Type: application/json" \
  -d '{"customerId": "cust_123", "lineItems": [{"productId": "prod_abc", "quantity": 1}]}' \
  -i
```

## Non-Functional Verification

- [ ] P95 latency < 150ms under 100 RPS (measure with `dotnet run` + `k6`)
- [ ] No memory leaks over 10-minute sustained load
- [ ] Correlation id appears in all logs for a given request
- [ ] `orders.created` metric emitted

## Observability Check

- [ ] Trace appears in <Jaeger / App Insights> with all spans
- [ ] Log entry has correlation id, customer id (no PII)
- [ ] Error logs include stack trace but no secrets

## Rollback Plan

If this feature breaks prod after deploy:

1. <specific rollback step — e.g., "Revert commit and redeploy previous container tag">
2. <if DB migration: "`dotnet ef database update <PreviousMigrationName>`">
3. <communication: "Post in #incidents Slack channel">

## Sign-off

- [ ] Dev: ready for review
- [ ] Reviewer: code LGTM
- [ ] QA / Product owner: accepted
- [ ] Merged to main

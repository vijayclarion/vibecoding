# Plan: <FEATURE_NAME>

> **Spec:** `./spec.md`
> **Status:** Draft / Approved / Executing

## Approach

<1-2 paragraphs. High-level technical approach. Why this approach vs alternatives considered.>

## Files to Create

| Path | Purpose |
|---|---|
| `src/<PROJECT>.Domain/Orders/Order.cs` | Aggregate root |
| `src/<PROJECT>.Domain/Orders/OrderLineItem.cs` | Value object |
| `src/<PROJECT>.Application/Orders/CreateOrder/CreateOrderCommand.cs` | Command DTO |
| `src/<PROJECT>.Application/Orders/CreateOrder/CreateOrderHandler.cs` | Use case |
| `src/<PROJECT>.Application/Orders/CreateOrder/CreateOrderValidator.cs` | FluentValidation |
| `src/<PROJECT>.Infrastructure/Orders/OrderRepository.cs` | EF Core repo |
| `src/<PROJECT>.Api/Endpoints/OrdersEndpoints.cs` | Minimal API endpoints |
| `tests/<PROJECT>.UnitTests/Orders/CreateOrderHandlerTests.cs` | Unit tests |
| `tests/<PROJECT>.IntegrationTests/Orders/CreateOrderEndpointTests.cs` | Integration tests |

## Files to Modify

| Path | Change |
|---|---|
| `src/<PROJECT>.Api/Program.cs` | Register `IOrderRepository`, map endpoints |
| `src/<PROJECT>.Infrastructure/Persistence/AppDbContext.cs` | Add `DbSet<Order>`, fluent config |

## Data Model Changes

```csharp
// Order aggregate
public class Order
{
    public OrderId Id { get; private set; }
    public CustomerId CustomerId { get; private set; }
    public IReadOnlyCollection<OrderLineItem> LineItems => _lineItems.AsReadOnly();
    public OrderStatus Status { get; private set; }
    public DateTimeOffset CreatedAt { get; private set; }
    // ...
}
```

**Migration:** `dotnet ef migrations add AddOrdersTable`

## API Contract

### `POST /orders`

**Request:**
```json
{
  "customerId": "cust_123",
  "lineItems": [
    { "productId": "prod_abc", "quantity": 2 }
  ]
}
```

**Responses:**
- `201 Created` with `Location: /orders/{id}` header, body: `{ "id": "ord_..." }`
- `400 Bad Request` with ProblemDetails on validation failure
- `401 Unauthorized` if no JWT
- `403 Forbidden` if JWT lacks `orders:write` scope
- `422 Unprocessable Entity` if product does not exist

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Product catalog API latency | Medium | Medium | Polly: 3 retries w/ exponential backoff, 500ms timeout |
| Duplicate submissions | High | Low | Idempotency-Key header, 24h dedupe cache |
| N+1 queries when loading line items | Medium | Medium | `.Include(o => o.LineItems)` in repository |

## Research Notes

<Any prior-art or investigation relevant to the implementation. Links to docs, RFCs, ADRs. Delete section if not needed.>

## Alternatives Considered

- **Event sourcing for Order** — rejected: overkill for current audit needs, adds complexity
- **Separate command/query DBs** — deferred: not yet justified by load

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

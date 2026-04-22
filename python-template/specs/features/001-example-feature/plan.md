# Plan: <FEATURE_NAME>

> **Spec:** `./spec.md`
> **Status:** Draft / Approved / Executing

## Approach

<1-2 paragraphs. High-level technical approach. Why this approach vs alternatives considered.>

## Files to Create

| Path | Purpose |
|---|---|
| `app/models/order.py` | SQLAlchemy ORM model |
| `app/schemas/order.py` | Pydantic DTOs (CreateOrderRequest, OrderResponse) |
| `app/repositories/order_repository.py` | Data access |
| `app/services/order_service.py` | Business logic |
| `app/routers/orders.py` | FastAPI router |
| `tests/unit/services/test_order_service.py` | Service unit tests |
| `tests/integration/test_orders_api.py` | Endpoint integration tests |

## Files to Modify

| Path | Change |
|---|---|
| `app/main.py` | Include `orders` router |
| `app/dependencies.py` | Add `get_order_service` provider |
| `migrations/versions/<timestamp>_add_orders.py` | New Alembic migration |

## Data Model Changes

```python
# app/models/order.py
from __future__ import annotations
from datetime import datetime
from uuid import UUID, uuid4
from sqlalchemy import ForeignKey, String
from sqlalchemy.orm import Mapped, mapped_column, relationship
from app.models.base import Base

class Order(Base):
    __tablename__ = "orders"

    id: Mapped[UUID] = mapped_column(primary_key=True, default=uuid4)
    customer_id: Mapped[str] = mapped_column(String(50))
    status: Mapped[str] = mapped_column(String(20))
    created_at: Mapped[datetime]

    line_items: Mapped[list["OrderLineItem"]] = relationship(
        back_populates="order",
        cascade="all, delete-orphan",
        lazy="selectin",  # avoid N+1
    )
```

**Migration:** `uv run alembic revision --autogenerate -m "add orders table"`

## API Contract

### `POST /orders`

**Request:**
```json
{
  "customer_id": "cust_123",
  "line_items": [
    { "product_id": "prod_abc", "quantity": 2 }
  ]
}
```

**Responses:**
- `201 Created` with `Location: /orders/{id}` header, body: `{"id": "..."}`
- `400 Bad Request` with ProblemDetails on validation failure
- `401 Unauthorized` if no JWT
- `403 Forbidden` if JWT lacks `orders:write` scope
- `422 Unprocessable Entity` if referenced product does not exist

## Pydantic Schemas

```python
# app/schemas/order.py
from pydantic import BaseModel, Field

class CreateLineItem(BaseModel):
    product_id: str = Field(..., min_length=1)
    quantity: int = Field(..., gt=0, le=1000)

class CreateOrderRequest(BaseModel):
    customer_id: str = Field(..., min_length=1)
    line_items: list[CreateLineItem] = Field(..., min_length=1)

class OrderResponse(BaseModel):
    id: str
```

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Product catalog API latency | Medium | Medium | httpx timeout=500ms, 3 retries with tenacity |
| Duplicate submissions | High | Low | Idempotency-Key header, Redis dedupe (24h TTL) |
| N+1 queries on line items | Medium | Medium | `lazy="selectin"` on relationship |

## Research Notes

<Any prior-art or investigation. Links to docs, RFCs, ADRs. Delete if not needed.>

## Alternatives Considered

- **Synchronous SQLAlchemy** — rejected: would block the event loop under load
- **Separate read/write models (CQRS)** — deferred: not yet justified by load

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

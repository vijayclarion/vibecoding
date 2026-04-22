# Tasks: <FEATURE_NAME>

> **Plan:** `./plan.md`
> **Convention:** `[P]` = parallelizable with adjacent `[P]` tasks (different files, no dependency)

## Setup

- [ ] **T1** — Create feature branch: `git checkout -b feature/<NNN>-<short-name>`
- [ ] **T2** — Add NuGet packages listed in `plan.md` → "Files to Modify"

## Domain Layer

- [ ] **T3** — Create `Order.cs` aggregate with private setters and factory method `[P]`
- [ ] **T4** — Create `OrderLineItem.cs` value object `[P]`
- [ ] **T5** — Create `OrderId.cs` strongly-typed id `[P]`
- [ ] **T6** — Create `OrderStatus.cs` enum `[P]`

## Application Layer

- [ ] **T7** — Create `CreateOrderCommand.cs` (input DTO)
- [ ] **T8** — Create `CreateOrderResult.cs` (output DTO)
- [ ] **T9** — Create `CreateOrderValidator.cs` — FluentValidation rules
- [ ] **T10** — Create `ICreateOrderHandler.cs` interface
- [ ] **T11** — Create `CreateOrderHandler.cs` — implementation (tests written first in T12)

## Tests (written BEFORE implementation where possible)

- [ ] **T12** — Unit tests for `CreateOrderHandler`:
  - happy path
  - customer not found → returns error
  - empty line items → validation error
  - product not found → returns error
- [ ] **T13** — Validator tests for `CreateOrderValidator`

## Infrastructure Layer

- [ ] **T14** — Add `Orders` DbSet to `AppDbContext.cs`
- [ ] **T15** — Create `OrderConfiguration.cs` (EF Core fluent config)
- [ ] **T16** — Create `OrderRepository.cs` implementing `IOrderRepository`
- [ ] **T17** — Run `dotnet ef migrations add AddOrdersTable`
- [ ] **T18** — Apply migration and verify schema

## API Layer

- [ ] **T19** — Create `OrdersEndpoints.cs` with `POST /orders` endpoint
- [ ] **T20** — Wire up dependency injection in `Program.cs`
- [ ] **T21** — Integration test: `POST /orders` happy path (201 Created, Location header)
- [ ] **T22** — Integration test: validation failure (400 ProblemDetails)
- [ ] **T23** — Integration test: unauthorized (401)

## Observability

- [ ] **T24** — Add structured logging at handler entry/exit with correlation id
- [ ] **T25** — Emit `orders.created` metric on success

## Quality Gates

- [ ] **T26** — `dotnet format` — clean
- [ ] **T27** — `dotnet test` — all green
- [ ] **T28** — Code coverage on new code ≥ 80%
- [ ] **T29** — No new Code Analysis warnings

## Documentation

- [ ] **T30** — Update `spec.md` if anything deviated from plan
- [ ] **T31** — Add entries to `lessons-learned.md` for any surprises
- [ ] **T32** — Update OpenAPI / Swagger if public API changed

## Wrap-up

- [ ] **T33** — Commit with message: `feat(orders): F<N.N> <short description>`
- [ ] **T34** — Open PR, link spec.md and plan.md
- [ ] **T35** — Merge to main after review
- [ ] **T36** — Update `roadmap.md` — mark F<N.N> as `[x]` done

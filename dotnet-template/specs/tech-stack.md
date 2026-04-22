# Tech Stack: <PROJECT_NAME>

> **Note to editor:** This file locks in HOW we build. The same mission could be built many ways — this captures our specific choices and *why*.

## Languages & Runtimes

- **Backend:** C# 12, .NET 8 (LTS)
- **Target framework:** `net8.0`
- **Nullable reference types:** enabled
- **Implicit usings:** enabled
- **Frontend:** <React 18 + TypeScript / Blazor Server / N/A — API only>
- **Database:** <PostgreSQL 16 / Azure SQL / SQL Server 2022>
- **Cache:** <Redis 7 / in-memory / N/A>

## Key Frameworks & Libraries

| Library | Version | Why chosen |
|---|---|---|
| ASP.NET Core | 8.x | <Minimal APIs: faster cold starts / MVC: team familiarity> |
| Entity Framework Core | 8.x | <ORM for complex domain / use Dapper for hot paths> |
| FluentValidation | 11.x | Expressive validation; better than DataAnnotations |
| Serilog | 4.x | Structured logging, rich sinks |
| MediatR | 12.x | <optional — CQRS handler dispatch> |
| xUnit | 2.x | Test framework |
| FluentAssertions | 6.x | Readable assertions |
| Testcontainers | 3.x | Real database in integration tests |
| Polly | 8.x | <optional — resilience for external calls> |

## Architecture

- **Pattern:** <Monolith / Modular monolith / Microservices>
- **Style:** <Clean Architecture / Vertical Slice / Onion>
- **Boundaries:** <list bounded contexts / modules>
- **Communication:** <REST / gRPC / events via Azure Service Bus>
- **Deployment target:** <Azure App Service / AWS ECS / Kubernetes / Docker>

### Folder structure

```
src/
├── <PROJECT>.Api/              ← endpoints, Program.cs, middleware
├── <PROJECT>.Application/      ← use cases, DTOs, interfaces
├── <PROJECT>.Domain/           ← entities, value objects, domain events
├── <PROJECT>.Infrastructure/   ← EF Core, external APIs, storage
└── <PROJECT>.Contracts/        ← shared DTOs (if exposed to other services)

tests/
├── <PROJECT>.UnitTests/
├── <PROJECT>.IntegrationTests/ ← WebApplicationFactory + Testcontainers
└── <PROJECT>.E2ETests/         ← optional
```

## Data Model (High-Level)

Sketch of main entities & relationships. The agent will elaborate in feature specs.

```
Customer ─< Order ─< OrderLineItem >─ Product
                 └─< Payment
                 └─< Fulfillment
```

## Request Lifecycle

For a key endpoint (e.g., `POST /orders`), trace the full pipeline:

1. Request arrives → authentication middleware validates JWT
2. Authorization policy check → `OrdersWritePolicy`
3. Model binding + FluentValidation
4. Endpoint handler delegates to `IOrderService`
5. Service loads aggregate via `IOrderRepository` (EF Core)
6. Domain logic mutates aggregate
7. Repository persists (`SaveChangesAsync`)
8. Domain events dispatched via MediatR
9. Response serialized with source-generated `JsonSerializerContext`

## Cross-Cutting Concerns

- **Authentication:** JWT Bearer tokens, issued by <Azure AD / IdentityServer / Auth0>
- **Authorization:** Policy-based, roles from JWT `roles` claim
- **Logging:** Serilog → <Seq / Application Insights / ELK>, JSON format, correlation id per request
- **Observability:** OpenTelemetry traces + metrics to <Jaeger / App Insights>
- **Error handling:** Global exception middleware returning RFC 7807 `ProblemDetails`
- **Configuration:** `IOptions<T>` bound from `appsettings.{env}.json` + env vars
- **Secrets:** <Azure Key Vault / AWS Secrets Manager / dotnet user-secrets (dev only)>
- **Caching:** <IMemoryCache for hot reads / Redis for distributed>
- **Rate limiting:** `Microsoft.AspNetCore.RateLimiting` — fixed window on write endpoints

## Testing Strategy

- **Unit tests:** xUnit + FluentAssertions + NSubstitute (mocking)
  - Target: 80% line coverage on `Domain` and `Application`
- **Integration tests:** `WebApplicationFactory<Program>` with Testcontainers for PostgreSQL
  - Every endpoint has ≥1 happy path + ≥2 error path tests
- **Contract tests:** <Pact / none>
- **E2E:** <Playwright / manual>
- **Performance:** NBomber or k6 for key endpoints before release

## Quality Gates (enforced in CI)

- [ ] `dotnet build` — zero warnings, warnings-as-errors
- [ ] `dotnet test` — all green
- [ ] `dotnet format --verify-no-changes` — style enforced
- [ ] Code coverage report (minimum thresholds)
- [ ] `dotnet list package --vulnerable` — no known CVEs
- [ ] Static analysis via Roslyn analyzers + <SonarCloud / none>

## Non-Functional Requirements

| NFR | Target |
|---|---|
| Availability | 99.9% (43 min/month downtime budget) |
| P95 latency | < 150ms for reads, < 300ms for writes |
| Throughput | 500 RPS sustained, 2000 RPS burst |
| Recovery (RTO) | < 15 minutes |
| Recovery (RPO) | < 5 minutes |
| Security | <OWASP ASVS L2 / SOC 2 / PCI-DSS scope-adjacent> |
| Compliance | <GDPR / CCPA / none> |
| Browser support | N/A (API only) |

## External Dependencies

- **ProductCatalog API** — https://... (internal, v2 contract)
- **Stripe** — payments, webhook-based settlement
- **SendGrid** — transactional email
- **Azure Service Bus** — order.* event topic

## Deployment

- **Environments:** `dev` → `staging` → `prod`
- **CI/CD:** <GitHub Actions / Azure DevOps>
- **Pipeline:** build → test → container build → deploy to staging → smoke tests → manual approval → prod
- **Container:** `mcr.microsoft.com/dotnet/aspnet:8.0-alpine` base
- **Migrations:** run on startup in dev, separate job in prod

## Open Questions / Pending Decisions

- [ ] <unresolved item that the agent should track>
- [ ] Event sourcing for orders? (leaning no for v1)

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

# Feature: <FEATURE_NAME> (F<N.N>)

> **Roadmap reference:** <link or copy the bullet from roadmap.md>
> **Branch:** `feature/<NNN>-<short-name>`
> **Status:** Draft / In Review / In Implementation / Validated / Merged

## User Story

As a **<role>**, I want to **<action>**, so that **<benefit>**.

## Background / Context

<Any context the reader (and agent) needs that isn't obvious. Link to relevant mission.md sections. Link to external docs.>

## Acceptance Criteria

Use Given/When/Then format. These become integration tests.

- [ ] **AC1** — Given <precondition>, when <action>, then <outcome>
- [ ] **AC2** — Given <precondition>, when <action>, then <outcome>
- [ ] **AC3** — Edge case: <case>
- [ ] **AC4** — Error case: <case>
- [ ] **AC5** — Error case: <case>

## Requirements

### Functional

- <requirement> — e.g., "POST /orders returns 201 with Location header pointing to GET /orders/{id}"
- <requirement>

### Non-Functional

- **Performance:** <target — e.g., "P95 < 150ms at 100 RPS">
- **Security:** <requirement — e.g., "Requires `orders:write` scope in JWT">
- **Observability:** <what must be logged / traced>
- **Backwards compatibility:** <N/A or details>

### Out of Scope

- <explicitly not in this feature>
- <...>

## Dependencies

- **Prerequisite features:** <F0.6 DB bootstrap>
- **External services:** <Stripe API / SendGrid / N/A>
- **New packages:** <none / list with versions>

## Open Questions

Track unresolved items with `[NEEDS CLARIFICATION]`. Resolve before moving to plan.

- [ ] [NEEDS CLARIFICATION] <question>
- [ ] [NEEDS CLARIFICATION] <question>

## Validation Scorecard

How we'll know it works.

- [ ] Unit tests for service logic (≥2 edge cases)
- [ ] Integration test for happy path (`httpx.AsyncClient`)
- [ ] Integration tests for each error case above
- [ ] Property-based test for invariants (if applicable)
- [ ] Manual smoke test: <steps>
- [ ] No regressions in existing test suite
- [ ] Logs show correlation id for every request
- [ ] Metrics emitted: <list>

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <n> |

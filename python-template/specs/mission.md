# Mission: <PROJECT_NAME>

> **Note to editor:** This file describes WHAT we're building and WHY. Keep it free of tech details (those go in `tech-stack.md`).

## The Problem

<One paragraph. Who has the pain? What does it cost them today? Be concrete — numbers, anecdotes, quotes.>

*Example:* "Our ops team manually reconciles ~800 orders per day across 3 spreadsheets, taking 4 hours of a senior analyst's time. Errors cost us an average of $12k/month in refunds."

## Our Solution

<One paragraph. What does the product do at a high level? Don't mention frameworks.>

*Example:* "OrderService is a single REST API that owns the order lifecycle end-to-end: creation, payment, fulfillment, returns. Ops team uses a dashboard backed by this API, eliminating the spreadsheet reconciliation."

## Target Audience

- **Primary:** <who uses this most? What's their technical level?>
- **Secondary:** <other users / stakeholders>
- **Operators:** <who runs and maintains it?>

*Example:*
- Primary: Ops analysts (non-technical, 5-person team)
- Secondary: Customer support (occasional read-only queries)
- Operators: Platform team (on-call rotation)

## Core Principles

Ranked in priority order. When principles conflict, the higher one wins.

1. <principle — e.g., "Correctness over speed — never drop an order">
2. <principle — e.g., "Auditability — every state change is logged">
3. <principle — e.g., "Observable — P95 latency visible in dashboards">
4. <principle — e.g., "Boring tech — prefer proven over novel">

## Success Metrics

Measurable, time-bound.

- <metric> — e.g., "Reduce daily reconciliation from 4h to <15min within 90 days of launch"
- <metric> — e.g., "Zero unhandled 500 errors in the first month"
- <metric> — e.g., "P95 endpoint latency < 150ms under 500 RPS"

## Non-Goals

What we explicitly will NOT do. This list is as important as what we will do.

- <non-goal> — e.g., "We are not building a general-purpose OMS for external customers"
- <non-goal> — e.g., "No multi-tenancy in v1"
- <non-goal> — e.g., "No front-end code in this repo — dashboard lives separately"

## Constraints

Business or regulatory constraints that bound every decision.

- <constraint> — e.g., "Must be PCI-DSS scope-adjacent (no card numbers stored)"
- <constraint> — e.g., "Must integrate with existing ProductCatalog API (no schema changes there)"
- <constraint> — e.g., "Launch before Black Friday (hard deadline)"

## Glossary

Project vocabulary — so the agent uses your terms consistently.

- **Order** — a customer's request for one or more products
- **Line item** — a single (product, quantity) pair within an order
- **Fulfillment** — the warehouse process of picking, packing, shipping
- **Reconciliation** — matching our order records against payment provider records
- <term> — <definition>

## Stakeholders

- **Product owner:** <name or role>
- **Tech lead:** <name or role>
- **Primary users:** <name or role>

## Revision History

| Date | Change | Author |
|------|--------|--------|
| YYYY-MM-DD | Initial draft | <name> |

# 🚀 Spec-Driven Development Expert Guide for GitHub Copilot

> Master the craft of directing Copilot with precision through structured specifications.

---

## 📚 Table of Contents

1. [The SDD Philosophy](#1-the-sdd-philosophy)
2. [Core Components](#2-core-components)
3. [The Three Pillars: Constitution, Specs, Plans](#3-the-three-pillars-constitution-specs-plans)
4. [File Structure & Organization](#4-file-structure--organization)
5. [How Copilot Reads Your Specs](#5-how-copilot-reads-your-specs)
6. [The Complete Workflow](#6-the-complete-workflow)
7. [Step-by-Step: Building Your First SDD Project](#7-step-by-step-building-your-first-sdd-project)
8. [Copilot-Specific Prompting Techniques](#8-copilot-specific-prompting-techniques)
9. [Common Mistakes & How to Avoid Them](#9-common-mistakes--how-to-avoid-them)
10. [Mastery Checklist](#10-mastery-checklist)

---

## 1. The SDD Philosophy

### What Makes SDD Different from "Just Prompting Copilot"

**Without SDD (ad-hoc vibe coding):**
```
You:     "Write a login function"
Copilot: [generates code]
You:     "Hmm, that's not quite right..."
Copilot: [generates different code]
You:     [repeats 5 more times]
```

**With SDD (disciplined prompting):**
```
You:     [write spec.md: "POST /login accepts email+password, validates with Zod,
                        returns JWT on success, 401 on invalid credentials"]
Copilot: [reads spec.md, asks clarifying questions]
You:     [answer questions, confirm plan]
Copilot: [generates code that matches spec exactly]
You:     [it works first time]
```

### Why This Works with Copilot

1. **Copilot reads files in your editor** — it can see your specs
2. **Copilot understands context** — you can point it at specific patterns
3. **Copilot respects structure** — explicit requirements reduce hallucinations
4. **Copilot works faster with clarity** — vague prompts cost 10× retries

### The Core Insight

> *"The better your spec, the less you need to guide Copilot. Perfect specs require zero back-and-forth."*

---

## 2. Core Components

Every SDD + Copilot project has these building blocks:

### A. **Constitution** (Immutable project rules)
- What you're building (mission)
- How you're building it (tech-stack)
- What you're building next (roadmap)

### B. **Feature Specifications** (Per-feature intent)
- What the feature does (spec.md)
- How to build it (plan.md)
- What work needs doing (tasks.md)

### C. **Copilot Configuration** (How Copilot works in your project)
- `.copilot-instructions.md` or `/docs/COPILOT.md` — rules Copilot follows
- `/.github/copilot-instructions.md` — (new in 2024+) standard location

### D. **Prompts** (Your words to Copilot)
- Constitution prompts (kickoff)
- Feature prompts (spec → plan → tasks)
- Implementation prompts (per-task)
- Debug prompts (troubleshooting)

### E. **Validation & Quality** (Proof it worked)
- Automated tests (what Copilot generates must pass)
- Checklists (manual verification steps)
- Rollback plans (if something breaks)

---

## 3. The Three Pillars: Constitution, Specs, Plans

### Pillar 1: Constitution (Project DNA)

The Constitution is the **single source of truth** Copilot consults before generating anything.

#### `mission.md` — What are we building?

```markdown
# Mission: OrderService

## The Problem
Our ops team manually reconciles 800 orders/day across 3 spreadsheets (4h/day).
Errors cost $12k/month.

## Our Solution
Single REST API owning order lifecycle: creation, payment, fulfillment, returns.

## Target Audience
- Primary: Ops analysts (non-technical, 5-person team)
- Secondary: Customer support (read-only queries)

## Core Principles (ranked)
1. Correctness — never drop an order
2. Auditability — every state change logged
3. Performance — P95 < 150ms
4. Boring tech — proven over novel

## Success Metrics
- Reduce daily reconciliation from 4h to <15min within 90 days
- Zero unhandled 500 errors in first month
- P95 latency < 150ms under 500 RPS

## Non-Goals
- Not a general-purpose OMS for external customers
- No multi-tenancy in v1
- No front-end code (dashboard separate)
```

**Copilot uses this to understand:** What matters for your project.

#### `tech-stack.md` — How are we building?

```markdown
# Tech Stack: OrderService

## Languages & Frameworks
- Backend: Node.js 20 LTS, Express.js
- Frontend: React 18 + TypeScript (separate repo)
- Database: PostgreSQL 16
- Auth: OAuth2 + JWT (RS256)

## Key Libraries
- Validation: Zod (not Joi, not yup)
- ORMi SQLAlchemy -> TypeORM (async)
- Testing: Jest + Supertest
- Logging: Winston (structured, JSON to stdout)

## Folder Structure
```
src/
├── routes/           — HTTP endpoints
├── services/         — business logic
├── models/           — data models
├── middleware/       — auth, logging, error handling
├── utils/            — helpers
└── types/            — TypeScript interfaces
tests/
├── unit/
├── integration/
```

## Architectural Patterns
- RESTful endpoints (no GraphQL v1)
- Stateless services (scale horizontally)
- Request-scoped contexts (correlation ids)
- Circuit breaker for external APIs

## Non-Functional Requirements
| NFR | Target |
|---|---|
| Availability | 99.9% (43 min/month downtime) |
| P95 latency | < 150ms reads, < 300ms writes |
| Throughput | 500 RPS sustained, 2000 RPS burst |
| RTO | < 15 minutes |
| Security | OWASP ASVS L2 |

## Conventions
- **Error handling:** Always return standard error response shape
- **Logging:** JSON with correlation-id in every log
- **Auth:** Check JWT scope on every protected endpoint
- **Validation:** Validate inputs with Zod at router level
- **Testing:** Every endpoint has ≥1 happy path + ≥2 error tests
```

**Copilot uses this to understand:** What patterns and libraries to use.

#### `roadmap.md` — What are we building next?

```markdown
# Roadmap: OrderService

## Phase 0 — Foundation
- [ ] F0.1 — Project scaffold + CI/CD pipeline
- [ ] F0.2 — Database schema + migrations setup
- [ ] F0.3 — /health endpoint with DB check
- [ ] F0.4 — Global error handler + logging baseline

## Phase 1 — MVP
- [ ] F1.1 — Create order (POST /orders)
- [ ] F1.2 — Get order (GET /orders/:id)
- [ ] F1.3 — List orders (GET /orders?status=pending)
- [ ] F1.4 — Update order status (PATCH /orders/:id/status)

**Exit criteria:** Ops can create, view, update orders via API.

## Phase 2 — Core Features
- [ ] F2.1 — JWT auth + scopes
- [ ] F2.2 — Payment webhook integration (Stripe)
- [ ] F2.3 — Order fulfillment tracking
- [ ] F2.4 — Email notifications

## Phase 3 — Polish
- [ ] F3.1 — OpenAPI/Swagger documentation
- [ ] F3.2 — Rate limiting
- [ ] F3.3 — Comprehensive monitoring + alerts
```

**Copilot uses this to understand:** Scope and priority.

---

### Pillar 2: Feature Specifications

For each feature in the roadmap, create a subfolder:

```
specs/features/001-create-order/
├── spec.md         — What the feature does
├── plan.md         — How you'll build it
├── tasks.md        — Atomic steps
└── validation.md   — How to verify it works
```

#### Example: `specs/features/001-create-order/spec.md`

```markdown
# Feature: Create Order (F1.1)

## User Story
As an **ops analyst**, I want to **create a new order** with line items,
so that **I can record customer purchases**.

## Acceptance Criteria
- [ ] **AC1** — POST /orders with valid payload → 201 Created + Location header
- [ ] **AC2** — Response body includes { id, customerId, status, createdAt }
- [ ] **AC3** — Empty line_items → 400 Bad Request with field-level error
- [ ] **AC4** — Invalid customer_id → 422 Unprocessable Entity
- [ ] **AC5** — Requires `orders:write` scope; missing → 401 Unauthorized

## Requirements

### Functional
- Accept JSON: { customerId: string, lineItems: [{productId, quantity}] }
- Validate with Zod schema
- Create Order in DB (initially status="pending")
- Return 201 with Location header

### Non-Functional
- P95 latency < 300ms
- Idempotent: same request twice = same order (via Idempotency-Key header)
- Structured logging with correlation id

### Out of Scope
- No payment processing yet
- No email notification yet

## Dependencies
- F0.2 (DB schema) must be complete
- F0.3 (health endpoint) should be done
- F2.1 (JWT auth) needed for scope checking

## Validation Scorecard
- [ ] POST /orders happy path returns 201
- [ ] Location header points to GET /orders/{id}
- [ ] Invalid inputs return 400/422 with ProblemDetails
- [ ] Unauthorized (no JWT) returns 401
- [ ] Logs include correlation id
- [ ] 95th percentile latency < 300ms
- [ ] All tests green
- [ ] No new lint warnings
```

#### Example: `specs/features/001-create-order/plan.md`

```markdown
# Plan: Create Order (F1.1)

## Approach
Straightforward CRUD: validate input with Zod → save to DB → return location.
No complex business logic yet (payment, fulfillment come later).

## Files to Create
| Path | Purpose |
|---|---|
| `src/types/order.ts` | TypeScript interfaces |
| `src/models/order.ts` | Database model |
| `src/routes/orders.ts` | Endpoints |
| `src/services/orderService.ts` | Business logic |
| `tests/integration/orders.test.ts` | Integration tests |

## Files to Modify
| Path | Change |
|---|---|
| `src/app.ts` | Import and register orders router |
| `src/db/migrations/<timestamp>_createOrdersTable.ts` | DB schema |

## Request/Response Contract

**POST /orders**
```
Request:
{
  "customerId": "cust_123",
  "lineItems": [
    { "productId": "prod_abc", "quantity": 2 }
  ]
}

Response (201):
Location: /orders/ord_xyz
{
  "id": "ord_xyz",
  "customerId": "cust_123",
  "lineItems": [...],
  "status": "pending",
  "createdAt": "2024-04-22T10:30:00Z"
}

Error (400):
{
  "status": 400,
  "title": "Bad Request",
  "detail": "lineItems must have at least 1 item",
  "instance": "/orders"
}
```

## Risks & Mitigations
| Risk | Mitigation |
|---|---|
| Duplicate submissions (user clicks button twice) | Idempotency-Key header + unique constraint |
| Invalid productId causes 500 instead of 422 | Validate product exists; return 422 if not |
| Missing correlation id in logs | Middleware injects into every request |

## Alternatives Considered
- Event sourcing for Order — rejected (overkill for v1)
- CQRS with separate read model — deferred (not needed yet)
```

#### Example: `specs/features/001-create-order/tasks.md`

```markdown
# Tasks: Create Order (F1.1)

## Setup
- [ ] **T1** — Create branch: `git checkout -b feature/001-create-order`
- [ ] **T2** — Ensure tests run: `npm test`

## Data Layer
- [ ] **T3** — Create `src/types/order.ts` with TypeScript interfaces
- [ ] **T4** — Create migration: `npm run db:migrate:create` → `create_orders_table`
- [ ] **T5** — Run migration: `npm run db:migrate`

## Service Layer
- [ ] **T6** — Create `src/services/orderService.ts` with `createOrder()` function
- [ ] **T7** — Unit tests for `createOrder`: happy path + 3 error cases

## API Layer
- [ ] **T8** — Create `src/routes/orders.ts` with POST endpoint
- [ ] **T9** — Wire up in `src/app.ts`
- [ ] **T10** — Integration test: POST /orders returns 201
- [ ] **T11** — Integration test: invalid payload returns 400
- [ ] **T12** — Integration test: missing auth returns 401

## Quality Gates
- [ ] **T13** — `npm run lint` — zero warnings
- [ ] **T14** — `npm run typecheck` — zero errors
- [ ] **T15** — `npm test` — all green
- [ ] **T16** — Coverage check: ≥85% on new code

## Commit & Merge
- [ ] **T17** — `git commit -m "feat(orders): F1.1 create order endpoint"`
- [ ] **T18** — Open PR, link to spec.md
- [ ] **T19** — Merge after review
- [ ] **T20** — Update roadmap.md: mark F1.1 as [x] done
```

---

### Pillar 3: Plans (How Copilot will work)

#### `.github/copilot-instructions.md` (or `COPILOT.md`)

```markdown
# GitHub Copilot Instructions

You are assisting with **OrderService**, a Node.js/Express order management API.

## Before You Generate Anything
1. Read `/specs/mission.md` — understand what we're building
2. Read `/specs/tech-stack.md` — learn our conventions
3. If working on a feature, read `/specs/features/NNN-*/spec.md` — know what's needed
4. If implementing a task, read `/specs/features/NNN-*/plan.md` — know the design

## Core Rules (Always Apply)

### Coding Conventions
- **Language:** TypeScript with strict mode
- **Framework:** Express.js (no decorators, no classes for routes)
- **Validation:** Zod for all input validation
- **Errors:** Always return standard ProblemDetails shape (RFC 7807)
- **Logging:** Winston with JSON format, include correlation-id
- **Testing:** Jest with Supertest for integration tests
- **No:** No global state, no callback hell, no magic numbers

### Architecture
- Routes are thin: receive request → delegate to service → return response
- Services contain business logic
- Models handle persistence
- Middleware handles cross-cutting concerns (auth, logging, errors)

### Error Handling
```typescript
// Always return this shape on error:
res.status(statusCode).json({
  status: statusCode,
  title: "Error Title",
  detail: "Human-readable explanation",
  instance: req.path
});
```

### Logging
```typescript
// Always include correlationId
logger.info({
  correlationId: req.correlationId,
  action: "order.created",
  orderId: order.id,
  customerId: order.customerId
});
```

### Testing
- Every route has ≥3 tests: happy path + 2 error cases
- Use Supertest: `const res = await request(app).post('/orders')`
- Test happy path AND error paths

## When Copilot is Stuck
- Use `[NEEDS CLARIFICATION]` to flag uncertainty
- Stop and ask for direction rather than guessing
- If a line of code doesn't follow conventions, reject it

## Definition of Done
Code is complete only when:
- [ ] Compiles with zero TypeScript errors
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm test` passes (all tests green)
- [ ] New code has ≥85% coverage
- [ ] Updated relevant spec/plan if anything changed
```

---

## 4. File Structure & Organization

Here's the complete file structure Copilot expects:

```
your-project/
│
├── .github/
│   └── copilot-instructions.md    ← Copilot reads this first
│
├── specs/                          ← SDD artifacts (version controlled!)
│   ├── mission.md                  ← What we're building
│   ├── tech-stack.md               ← How we're building
│   ├── roadmap.md                  ← What's next
│   ├── LessonsLearned.md          ← Grows over time
│   └── features/
│       ├── 000-foundation/         ← Phase 0
│       │   ├── spec.md
│       │   ├── plan.md
│       │   ├── tasks.md
│       │   └── validation.md
│       ├── 001-create-order/       ← Phase 1, Feature 1
│       │   ├── spec.md
│       │   ├── plan.md
│       │   ├── tasks.md
│       │   └── validation.md
│       ├── 002-list-orders/        ← Phase 1, Feature 2
│       └── ...
│
├── src/                            ← Source code
│   ├── routes/
│   ├── services/
│   ├── models/
│   ├── middleware/
│   ├── types/
│   ├── app.ts
│   └── index.ts
│
├── tests/                          ← Test files
│   ├── unit/
│   ├── integration/
│   └── setup.ts
│
├── migrations/                     ← Database migrations
│   └── 001_create_tables.sql
│
├── tsconfig.json                   ← TypeScript config
├── jest.config.js                  ← Testing config
├── eslint.config.js                ← Linting rules
├── package.json
└── README.md                       ← Project overview
```

---

## 5. How Copilot Reads Your Specs

### When You Open Copilot Chat

Copilot automatically includes:
1. **Currently open file** — context for the task
2. **Symbols in scope** — functions, types it can reference
3. **.github/copilot-instructions.md** — IF you ask it to use specs

### To Make Copilot Read Your Specs:

**Option A: Explicit in the prompt**
```
@Copilot Read /specs/mission.md, /specs/tech-stack.md, and 
/specs/features/001-create-order/spec.md. 
Now implement the tasks in /specs/features/001-create-order/tasks.md.
```

**Option B: Reference files in the prompt**
```
Based on the plan in plan.md, create the orderService.ts file.
@plan
```

**Option C: Use Copilot's `.md` file context window**
Open `spec.md` in your editor, then ask Copilot questions — it reads the file you're viewing.

### Copilot's Context Window (What It Can Read)

- **Always:** The `.github/copilot-instructions.md` file
- **When you ask:** Any file you mention by name or path
- **When you select:** The code you've highlighted
- **When you ask @-mentions:** You can reference files with `@filename`

---

## 6. The Complete Workflow

```
┌─────────────────────────────────────────────────────────────┐
│                    SDD + COPILOT WORKFLOW                   │
└─────────────────────────────────────────────────────────────┘

PHASE 0: CONSTITUTION (One-time setup)
  ↓
1. Write mission.md                (What are we building?)
   → Copilot reads, asks clarifying questions
   → You answer every question

2. Write tech-stack.md             (How are we building?)
   → Copilot reads, suggests improvements
   → You confirm choices

3. Write roadmap.md                (What's next?)
   → Copilot reads, asks if phases are right size
   → You adjust, commit

4. Write .github/copilot-instructions.md
   → Codify your conventions
   → Copilot will follow these rules forever


PHASE 1: PER-FEATURE LOOP (repeat for each feature)
  ↓
1. Create spec.md                  (What the feature does)
   → Copilot reads mission/tech-stack, asks about spec
   → You clarify requirements
   → COMMIT spec.md

2. Create plan.md                  (How you'll build it)
   → Copilot reads spec, asks about architecture
   → You confirm design
   → COMMIT plan.md

3. Create tasks.md                 (Atomic steps)
   → Copilot reads plan, asks if tasks are right size
   → You adjust breakdown
   → COMMIT tasks.md

4. Implement each task             (Per-task generation)
   → You pick task T1 from tasks.md
   → You ask Copilot to implement it
   → Copilot reads mission/tech-stack/plan/task
   → Copilot generates code
   → You review, run tests, approve
   → REPEAT for T2, T3, ...

5. Validate                        (Final check)
   → You run validation.md checklist
   → Tests pass, performance OK, no regressions
   → MERGE to main
   → UPDATE roadmap.md


REPEAT for every feature until MVP is shipped.
```

---

## 7. Step-by-Step: Building Your First SDD Project

### Day 1: Constitution Setup (2-3 hours)

**Step 1: Create the skeleton**
```bash
mkdir my-sdd-project && cd my-sdd-project
git init
mkdir -p specs/features
mkdir -p .github
mkdir -p src tests
touch .github/copilot-instructions.md
touch specs/{mission,tech-stack,roadmap}.md
```

**Step 2: Kickoff prompt to Copilot**

Open Copilot Chat and paste:
```
I'm starting a new project using Spec-Driven Development.
Act as a senior architect.

Project: A REST API for [describe your project in 2-3 sentences].

Team: [your team size, experience, preferences]
Constraints: [deadline, tech stack preferences, compliance needs]

Please help me write the Constitution:
1. mission.md — what we're building and for whom
2. tech-stack.md — languages, frameworks, NFRs
3. roadmap.md — phased feature plan

Ask me clarifying questions ONE AT A TIME.
When done, I'll write these files in /specs/ using your guidance.
```

**Step 3: Answer Copilot's questions**

Expect ~8-12 questions about:
- Target audience
- Success metrics
- Tech preferences (Node vs Python? Postgres vs MongoDB?)
- Architecture style (monolith? microservices?)
- Testing approach
- Roadmap granularity

**Answer every question thoughtfully.** Say "deferred" if unsure.

**Step 4: Copilot drafts the Constitution**

Copy Copilot's output into `/specs/mission.md`, `/specs/tech-stack.md`, `/specs/roadmap.md`.

**Step 5: Set Copilot instructions**

Create `.github/copilot-instructions.md` with:
```markdown
# GitHub Copilot Instructions for [ProjectName]

You are helping build [project description].

## Read These First
1. /specs/mission.md
2. /specs/tech-stack.md
3. /specs/roadmap.md
4. When working on a feature: /specs/features/[feature-name]/spec.md

## Key Conventions
- Language: [language + version]
- Framework: [framework + version]
- Validation: [library]
- Testing: [framework + approach]
- Errors: [error format]
- Logging: [format]

## Rules
- [list 5-10 key rules from tech-stack.md]

## Definition of Done
Code is done when:
- [ ] Compiles/runs with zero warnings
- [ ] Tests pass
- [ ] No lint issues
- [ ] No type errors
- [ ] Follows conventions above
```

**Step 6: Commit**

```bash
git add specs/ .github/
git commit -m "chore: establish project constitution"
```

---

### Day 2: First Feature Specification (1-2 hours)

**Step 1: Pick the smallest feature from Phase 1**

Let's say it's "Create order endpoint" (F1.1).

**Step 2: Ask Copilot to help write spec.md**

```
I'm implementing F1.1 from the roadmap: "Create order (POST /orders)".

Based on /specs/mission.md, /specs/tech-stack.md, and /specs/roadmap.md:

Please draft /specs/features/001-create-order/spec.md with:
- User story
- 5 acceptance criteria (Given/When/Then)
- Functional + non-functional requirements
- Out-of-scope section
- Validation scorecard

Ask me clarifying questions about the requirements.
```

**Step 3: Review & refine spec.md**

Copilot drafts. You review. If something's wrong:
```
The spec says "validate with Zod" but I didn't mention that.
Actually, we use [ValidationLibrary]. Please update.
```

**Step 4: Write plan.md**

```
Now draft /specs/features/001-create-order/plan.md with:
- High-level approach (1 paragraph)
- Files to create (with purpose)
- Files to modify (with changes)
- Request/response contract
- Risks & mitigations
- Alternatives considered

Make sure it respects /specs/tech-stack.md patterns.
```

**Step 5: Write tasks.md**

```
Now draft /specs/features/001-create-order/tasks.md with:
- Atomic tasks (each ≤30 minutes of work)
- Ordered by dependency
- Mark [P] for parallelizable tasks
- Include test tasks before implementation
- End with quality gates + merge tasks

Don't start implementing yet — I'll review this first.
```

**Step 6: Commit all three files**

```bash
git add specs/features/001-create-order/
git commit -m "feat: F1.1 create order — spec, plan, tasks"
```

---

### Day 3-4: Implementation (Per-task generation)

**For each task in tasks.md:**

**Step 1: Open the relevant file**

If T3 is "create order.ts model", open the (empty or existing) file in your editor.

**Step 2: Ask Copilot to implement the task**

```
Implement task T3 from /specs/features/001-create-order/tasks.md:
"Create src/types/order.ts with TypeScript interfaces"

Read:
- /specs/features/001-create-order/plan.md (the contract)
- /specs/tech-stack.md (conventions)
- /specs/.../[other completed tasks] if relevant

Generate the file. Stop before testing — I'll run it.
```

**Step 3: Review the diff**

Copilot generates. You check:
- Does it match the spec/plan?
- Does it follow conventions from tech-stack.md?
- Are there obvious bugs?
- Is the naming consistent?

**Step 4: Run tests**

```bash
npm test
npm run lint
npm run typecheck
```

**Step 5: Approve and continue**

```
Looks good. Now implement T4 from tasks.md: [next task]
```

---

### Day 5: Validation & Commit

**Step 1: Run the validation.md checklist**

Manually or automated:
- All tests pass
- API contracts match spec.md
- Performance is acceptable
- No regressions

**Step 2: Commit & update roadmap**

```bash
git add src/ tests/
git commit -m "feat(orders): F1.1 create order endpoint

See specs/features/001-create-order/ for full spec and plan."

# Update specs/roadmap.md
# Mark [x] F1.1 — Create order (POST /orders)
git add specs/roadmap.md
git commit -m "docs: mark F1.1 complete"
```

**Step 3: Celebrate — move to F1.2**

Rinse and repeat for the next feature.

---

## 8. Copilot-Specific Prompting Techniques

### Technique 1: The "Read This First" Pattern

```
Before you write anything, read these files:
- /specs/mission.md (what we're building)
- /specs/tech-stack.md (how we're building)
- @current-file (the file I'm editing)

Then [task].
```

**Why this works:** Copilot loads context in order, applies the latest to the task.

---

### Technique 2: The "Reference Existing Code" Pattern

```
Follow the same pattern as src/routes/users.ts.
Now implement src/routes/orders.ts with:
- POST /orders
- GET /orders/:id
- PATCH /orders/:id/status
```

**Why this works:** Copilot learns from examples in your codebase.

---

### Technique 3: The "Reject & Refine" Pattern

```
I don't like that approach. Instead:
- Use [different library]
- Handle the error case with [specific strategy]
- Keep the request validation the same

Generate again.
```

**Why this works:** Copilot often generates plausible-but-not-ideal code on first try. Rejection with direction yields better results.

---

### Technique 4: The "Atomic Task" Pattern

```
Implement this task ONLY:
"T5 from /specs/features/001-create-order/tasks.md:
Create src/services/orderService.ts with createOrder() function"

Don't generate routes, don't generate tests. Just the service.
Stop when the function is done.
```

**Why this works:** Small scope = higher accuracy. Copilot won't get lost.

---

### Technique 5: The "Explicit Constraints" Pattern

```
Generate the endpoint with these constraints:
1. Use Zod for validation (from tech-stack.md)
2. Return ProblemDetails on error (standard error shape)
3. Include correlation-id in response headers
4. Log with Winston using JSON format
5. Handle the case where customer doesn't exist → 422 response

Don't deviate from these constraints.
```

**Why this works:** Constraints reduce hallucinations. Copilot can't invent "better" patterns.

---

### Technique 6: The "Ask Questions" Pattern

If Copilot is uncertain:

```
[NEEDS CLARIFICATION]

What should happen if:
- The customer ID doesn't exist in the database?
- The line_items array is empty?
- The request includes an unknown field?

Answer these, then I'll ask you to generate the validation code.
```

**Why this works:** Forces Copilot to stop and ask rather than guess.

---

## 9. Common Mistakes & How to Avoid Them

### ❌ Mistake 1: Vague Specifications

**Bad:** "Create an order endpoint"
**Good:** "POST /orders accepts { customerId: string, lineItems: [{ productId: string, quantity: number }] }, validates with Zod, returns 201 + Location header on success, 400 on validation failure, 422 if customer doesn't exist"

**Fix:** Use the spec.md template. Write acceptance criteria. Be concrete.

---

### ❌ Mistake 2: Skipping the Plan

**Bad:** Spec → jump straight to code
**Good:** Spec → plan → tasks → code

**Why:** The plan is where you design. Copilot will ask questions if the design is unclear. Better to discover gaps in plan.md than in code.

---

### ❌ Mistake 3: Overloaded Tasks

**Bad:** T1 = "Implement entire create order feature"
**Good:** T1, T2, T3, T4, ... = 10 small tasks

**Why:** Copilot gets lost in large tasks. Small tasks = accurate code, easy review.

---

### ❌ Mistake 4: Not Reading the Spec

**Bad:** You ask Copilot to implement a feature, then complain it's wrong
**Good:** You read spec.md first, know what you're asking for, then guide Copilot

**Why:** Copilot is a tool. You're the architect. Know what you want.

---

### ❌ Mistake 5: Accepting the First Output

**Bad:** Copilot generates code, you commit it as-is
**Good:** Copilot generates code, you review, test, challenge, refine

**Why:** First outputs are often plausible but not optimal. Refinement is where quality happens.

---

### ❌ Mistake 6: Inconsistent Conventions

**Bad:** First endpoint uses Zod, second endpoint uses manual validation, third uses DataAnnotations
**Good:** All endpoints follow tech-stack.md patterns

**Fix:** Reference tech-stack.md in every prompt. Tell Copilot to reject deviations.

---

### ❌ Mistake 7: No Tests

**Bad:** Copilot generates code, you don't write tests
**Good:** You write tests before OR with the code

**Why:** Generated code without tests is unmaintainable. Tests prove it works.

---

### ❌ Mistake 8: Specs That Drift from Reality

**Bad:** Plan says "use PostgreSQL" but code uses SQLite
**Good:** Update plan.md when reality diverges

**Why:** Specs are your source of truth. If they diverge, future engineers will be confused.

---

## 10. Mastery Checklist

Track your progress toward SDD + Copilot mastery:

### Foundation (Week 1)
- [ ] Created all three Constitution files (mission, tech-stack, roadmap)
- [ ] Wrote `.github/copilot-instructions.md` with 10+ rules
- [ ] Committed Constitution to git
- [ ] Understand the "three pillars" concept
- [ ] Can explain why specs reduce Copilot hallucinations

### Specification (Week 2)
- [ ] Written 3+ complete feature specs (spec.md + plan.md + tasks.md)
- [ ] Each spec has 5+ acceptance criteria
- [ ] Each plan has explicit file/API contracts
- [ ] Each tasks breakdown is ≤10 atomic items
- [ ] Can review another person's spec for completeness

### Implementation (Week 3-4)
- [ ] Implemented 2+ complete features using SDD
- [ ] Each feature: spec → plan → tasks → 10+ tasks implemented
- [ ] All features have ≥85% test coverage
- [ ] Tests pass on first run (not after 5 iterations)
- [ ] Can implement a task with zero back-and-forth with Copilot

### Refinement (Week 5+)
- [ ] Maintains LessonsLearned.md with real insights
- [ ] Updates tech-stack.md when patterns emerge
- [ ] Mentors others on SDD
- [ ] Can debug when Copilot hallucinates (find the real cause, adjust spec/plan)
- [ ] Achieves 50%+ first-run accuracy (code from Copilot works mostly as-is)

### Expert (Month 2+)
- [ ] Shipped 5+ features end-to-end with SDD
- [ ] New team members can read your specs and start coding
- [ ] Can write a spec so clear that Copilot needs zero clarifying questions
- [ ] Have patterns for every common situation (CRUD, auth, validation, errors)
- [ ] Can mentor teams on SDD methodology
- [ ] Measure and report productivity gains (e.g., 3× faster feature delivery)

---

## 🎓 Learning Path

### Phase 1: Theory (Days 1-2)
1. Read this entire guide (2-3 hours)
2. Study the Constitution template files
3. Understand the spec → plan → tasks flow

### Phase 2: First Project (Days 3-7)
1. Create Constitution for a real (or practice) project
2. Implement 1-2 features end-to-end
3. Refine specs based on what you learned
4. Commit everything

### Phase 3: Pattern Recognition (Weeks 2-3)
1. Implement 3-5 more features
2. Start recognizing patterns (CRUD, auth, validation, errors)
3. Codify patterns in tech-stack.md
4. Update `.github/copilot-instructions.md` with common issues you encountered

### Phase 4: Mentoring (Week 4+)
1. Review specs written by teammates
2. Coach others on how to prompt Copilot effectively
3. Maintain project specs as living documents
4. Measure and iterate on the SDD process itself

---

## 🔗 Key Resources

- **GitHub Spec Kit:** https://github.com/github/spec-kit (CLI for SDD scaffolding)
- **Copilot Documentation:** https://docs.github.com/en/copilot
- **Copilot Best Practices:** https://github.blog/2023-06-20-how-to-write-better-prompts-for-github-copilot/
- **RFC 7807 (ProblemDetails):** Standard error response format

---

## 💡 Pro Tips

1. **Version your specs.** Commit them alongside code. Future you will thank present you.

2. **Use feature branches for features.** `feature/001-create-order` = branch for spec + code.

3. **Keep specs close to code.** Same repo. Same commit. Don't let them diverge.

4. **Make specs readable.** Other humans (and Copilot) need to understand them.

5. **Run quality gates on every commit.** Tests, lint, typecheck — always.

6. **Celebrate wins.** When a feature ships with zero bugs from Copilot, that's a win. Document what worked in LessonsLearned.md.

---

**The secret to mastery:** Write better specs, not better prompts. Copilot will follow.

Now go build something great. 🚀

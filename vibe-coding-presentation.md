# 🎯 Vibe Coding for Developers

> *A practical guide for .NET and Python developers to harness AI-assisted development*

---

## 📑 Table of Contents

1. [What is Vibe Coding?](#1-what-is-vibe-coding)
2. [Why Vibe Coding Matters](#2-why-vibe-coding-matters)
3. [The Vibe Coding Mindset](#3-the-vibe-coding-mindset)
4. [Tools & Frameworks](#4-tools--frameworks)
5. [Structured Vibe Coding: Context Is King](#5-structured-vibe-coding-context-is-king)
6. [The Vibe Coding Workflow](#6-the-vibe-coding-workflow)
7. [.NET Use Cases](#7-net-use-cases)
8. [Python Use Cases](#8-python-use-cases)
9. [Prompt Engineering Playbook](#9-prompt-engineering-playbook)
10. [Pattern Drift & Context Collapse](#10-pattern-drift--context-collapse)
11. [Anti-Patterns to Avoid](#11-anti-patterns-to-avoid)
12. [Best Practices & Governance](#12-best-practices--governance)
13. [Hands-On Demo Script](#13-hands-on-demo-script)
14. [Key Takeaways](#14-key-takeaways)

---

## 1. What is Vibe Coding?

**Vibe Coding** is a term coined by **Andrej Karpathy** (former Tesla AI lead, OpenAI founding member) in a viral X post in February 2025:

> *"There's a new kind of coding I call 'vibe coding', where you fully give in to the vibes, embrace exponentials, and forget that the code even exists."*

Instead of writing every line yourself, you **describe the intent in natural language** and let an AI assistant produce the code. You act as the **architect, reviewer, and tester** — not the typist.

### 📈 How It Evolved (2025 → 2026)

| Era | Style | Reality |
|---|---|---|
| **Early 2025 — "Pure" vibe coding** | "Forget the code exists" | Great for throwaway weekend projects. Broke at scale. |
| **Late 2025 — Hype peak** | Everyone trying it | Discovered: AI code has **~70% more issues** and **~4× more duplication** without structure. |
| **2026 — Structured vibe coding** | Specs + context + guardrails | Production-grade when combined with architectural discipline. |

> **Modern definition (2026):** Vibe coding is a disciplined practice where developers write natural-language **specs**, AI generates code under **structured human oversight**, with **persistent project context**, **multi-model orchestration**, and **layered validation**.

The "vibe" today is less about casualness and more about **fluency in directing AI systems** toward correct, maintainable software.

### 🆚 Vibe Coding vs No-Code / Low-Code

| | Vibe Coding | No-Code (Bubble, Webflow) | Low-Code |
|---|---|---|---|
| **Output** | Real source code you own | Locked into platform | Platform + code escape hatch |
| **Portability** | Full (deploy anywhere) | Limited | Partial |
| **Ceiling** | As high as the language | Platform-limited | Platform-limited |
| **Audience** | Devs + literate builders | Non-devs | Mostly non-devs |

### 🧭 Traditional Coding vs. Vibe Coding

| Aspect | Traditional Coding | Vibe Coding |
|---|---|---|
| **Primary activity** | Writing syntax | Describing intent |
| **Speed** | Typing speed limited | Thought speed limited |
| **Dev role** | Code producer | Code director / reviewer |
| **Iteration** | Manual refactor | Prompt refinement |
| **Skill focus** | Memorizing APIs | System design + prompting |
| **Testing** | Write after code | Generate alongside code |

### ⚠️ Important Distinction

Vibe coding is **not** "blindly accept whatever the AI writes." For production systems, it's better described as **AI-Assisted Engineering** — where:
- You *own* the design
- You *review* every change
- You *test* the output
- You *refuse* code you don't understand in critical paths

---

## 2. Why Vibe Coding Matters

### 📊 The Productivity Case

- **Boilerplate elimination** — controllers, DTOs, migrations, test scaffolding
- **Context switching reduction** — stay in flow, describe instead of Google
- **Onboarding acceleration** — new codebases become navigable via AI Q&A
- **Documentation on demand** — generate READMEs, OpenAPI specs, ADRs

### 🎯 What It Unlocks

```
Junior Dev + AI     → Mid-level output
Mid-level Dev + AI  → Senior-level output
Senior Dev + AI     → Architect-level leverage
```

The amplifier effect is real — but only if you know **what good looks like**.

---

## 3. The Vibe Coding Mindset

### The 5 Mental Shifts

1. **From "how do I write this?" → "what do I want?"**
   Focus on outcomes, not syntax.

2. **From solo author → pair programmer**
   Treat the AI as a capable junior who needs context.

3. **From perfect first draft → iterative refinement**
   Prompt → generate → critique → re-prompt.

4. **From memorization → verification**
   You don't need to know every API, but you must verify the code works.

5. **From code ownership → outcome ownership**
   You own the *behavior*, the AI helps with the *characters*.

---

## 4. Tools & Frameworks

### 🛠️ The 2026 Vibe Coding Stack

Modern vibe coding tools fall into **two distinct groups**:

**Group A — AI Coding Assistants** (for developers, inside your editor/terminal):

| Tool | Best For | Signature Feature |
|---|---|---|
| **Claude Code** | Terminal-first agentic workflows; complex multi-file changes | `CLAUDE.md` project context + Plan mode + Skills |
| **Cursor** | IDE-integrated development with visual diffs | Codebase indexing + inline chat |
| **Windsurf** | Polished alternative to Cursor with strong multi-file edits | Cascade agent |
| **GitHub Copilot** | Inline autocomplete + chat | Deepest IDE integrations (VS Code, JetBrains, Visual Studio) |
| **Gemini CLI** | Terminal agent with largest free tier | 1M token context + `GEMINI.md` |
| **Aider** | Git-native CLI pair programmer | Direct git commits |

**Group B — AI App Builders** (for non-devs / rapid prototypes):

| Tool | Best For |
|---|---|
| **Lovable** | Full-stack apps from a description (hosted) |
| **Bolt.new** | Browser-based app prototyping |
| **v0.dev** | React/Next.js UI generation |
| **Replit Agent** | End-to-end app building with hosting |
| **Firebase Studio / Google AI Studio** | Prototyping + Google Cloud deployment |

### 🏗️ Framework: The **PROMPT** Method

A mnemonic to structure every prompt you write:

| Letter | Meaning | Example |
|---|---|---|
| **P**ersona | Who should the AI act as? | "Act as a senior .NET API architect" |
| **R**equirement | What exactly do you want? | "Build a REST endpoint for user registration" |
| **O**utput format | How should the answer look? | "Return C# code with XML docs" |
| **M**ust-haves | Non-negotiable constraints | "Use FluentValidation, async/await" |
| **P**itfalls | What to avoid | "No Entity Framework, use Dapper" |
| **T**ests | How will we verify? | "Include xUnit tests with Moq" |

### 🏗️ Framework: The **CRISPE** Pattern (alternative)

- **C**apacity / Role
- **R**equest
- **I**nsight / Context
- **S**tatement of need
- **P**ersonality / Tone
- **E**xperiment / Iterate

---

## 5. Structured Vibe Coding: Context Is King

> *"In 2026, the best developers aren't the ones who type the fastest — they're the ones who provide the clearest context."*

**Unstructured vibe coding** (ad-hoc prompts, no project context) is what produces the infamous AI code quality problems. **Structured vibe coding** adds three layers of discipline on top:

### The Three Pillars of Structured Vibe Coding

```
         ┌──────────────────────────────────────────┐
         │  1. PROJECT CONTEXT (persistent memory)  │
         │     CLAUDE.md / AGENTS.md / GEMINI.md    │
         ├──────────────────────────────────────────┤
         │  2. SPECS (what + how, modular)          │
         │     what-feature-x.md, how-security.md   │
         ├──────────────────────────────────────────┤
         │  3. QUALITY GATES (automated validation) │
         │     tests, linters, type checkers, CI    │
         └──────────────────────────────────────────┘
```

### 📄 The Context File — Your Most Important File

Every serious AI coding tool in 2026 reads a project-root markdown file for persistent context:

| Tool | Context file |
|---|---|
| Claude Code | `CLAUDE.md` |
| Gemini CLI | `GEMINI.md` |
| Cursor | `.cursor/rules/*.mdc` |
| Generic / emerging standard | `AGENTS.md` |
| GitHub Copilot | `.github/copilot-instructions.md` |

**Example `CLAUDE.md` for a .NET project:**

```markdown
# Project: OrderService
## Stack
- .NET 8, ASP.NET Core Minimal APIs, EF Core 8, PostgreSQL
- xUnit + FluentAssertions + Testcontainers for tests
- Serilog → Seq for logging

## Conventions
- Namespaces follow folder structure
- DTOs live in /Contracts; entities in /Domain
- All endpoints return ProblemDetails on error
- Async everywhere; no .Result or .Wait()
- Use FluentValidation, never DataAnnotations

## Pitfalls to avoid
- No direct DbContext usage in endpoints — always via a Service
- No public setters on domain entities
- Never log PII (email, phone, full name)

## Definition of Done
- xUnit integration test covering happy path + 1 error case
- `dotnet format` passes
- `dotnet test` passes
```

**Example `CLAUDE.md` for a Python project:**

```markdown
# Project: analytics-api
## Stack
- Python 3.12, FastAPI, SQLAlchemy 2.0 (async), Pydantic v2, Alembic
- pytest + httpx.AsyncClient for tests
- ruff + mypy --strict

## Conventions
- Routers in /app/routers, services in /app/services, models in /app/models
- Pydantic schemas separate from SQLAlchemy models
- All endpoints have response_model + type hints
- Use dependency injection (Depends) — no globals

## Definition of Done
- 100% branch coverage on new code
- `ruff check .` and `mypy .` pass
- `pytest` passes in <10 seconds
```

### 📋 Spec-Driven Vibe Coding

Inspired by Sean Grove's ["Specs are the new code"](https://www.youtube.com/results?search_query=sean+grove+specs+are+the+new+code) talk (AI Engineer 2025) and Red Hat's **Vibes-Specs-Skills-Agents** framework:

**Split specs into two file types:**

- **`what-*.md`** — describes *what* the end state looks like (user stories, acceptance criteria)
- **`how-*.md`** — describes *how* to build it within your environment (patterns, security rules, concurrency model)

**Example `specs/how-security.md`:**
```markdown
# HOW: Security and Authentication
## Standards
- All API endpoints MUST require JWT validation
- Secrets MUST come from environment variables, never hardcoded
- All user input MUST be validated before DB queries

## Patterns
- Use [Authorize] on controller methods (.NET) / Depends(get_current_user) (FastAPI)
- Pass X-Correlation-ID to all downstream calls

## Lessons Learned (auto-updated)
- 2026-03-01: Always check JWT 'aud' claim to prevent token reuse
```

When an agent encounters a bug, instruct it to: **record the fix in `LessonsLearned.md` and update the relevant `how-*.md` spec.** Every mistake becomes a permanent improvement.

---

## 6. The Vibe Coding Workflow

### The 7-Step Loop

```
┌─────────────────────────────────────────────────────────┐
│  1. DESCRIBE intent in plain language                   │
│  2. CONTEXT share: codebase, constraints, stack         │
│  3. GENERATE via AI                                     │
│  4. REVIEW: read every line, challenge assumptions      │
│  5. RUN: execute, test, observe                         │
│  6. REFINE: prompt with what's wrong                    │
│  7. COMMIT: with meaningful message + tests             │
└─────────────────────────────────────────────────────────┘
```

### 🔁 The Three Layers of Context

1. **Project context** — architecture, patterns, conventions (feed via README/docs)
2. **Task context** — what this specific change should do
3. **Constraint context** — performance, security, style rules

> **Rule of thumb:** If a human dev would need it to do the task, the AI needs it too.

---

## 7. .NET Use Cases

### Use Case 1: Scaffolding a Web API

**Scenario:** Build a product catalog REST API in ASP.NET Core 8.

**Prompt:**
```
Act as a senior .NET 8 architect.

Build a Minimal API project for a Product Catalog with:
- Endpoints: GET /products, GET /products/{id}, POST /products, PUT /products/{id}, DELETE /products/{id}
- Entity Framework Core with SQL Server
- DTOs separate from entities
- FluentValidation for POST/PUT
- Global exception middleware returning ProblemDetails
- Serilog for structured logging
- xUnit + WebApplicationFactory integration tests

Return:
1. Folder structure
2. Program.cs
3. Each file with full code and XML docs
4. A sample curl script to test
```

**What you get:** A runnable, idiomatic .NET 8 project in under a minute.

---

### Use Case 2: LINQ Refactoring

**Prompt:**
```
Refactor this imperative loop into a single LINQ expression.
Explain trade-offs (readability vs performance):

[paste code]
```

---

### Use Case 3: Generating Unit Tests for Legacy Code

**Prompt:**
```
Here is a legacy service class in C#:
[paste code]

Generate xUnit tests with Moq that:
- Cover every public method
- Include one happy path and two edge cases per method
- Mock IRepository and ILogger<T>
- Use the AAA pattern with comments
- Name tests using MethodName_Scenario_ExpectedResult
```

---

### Use Case 4: EF Core Migration Planning

**Prompt:**
```
Review my current DbContext [paste] and the new requirement:
"Add a many-to-many relationship between Orders and Promotions with a CreatedAt timestamp on the join."

Produce:
1. Updated entity classes
2. Fluent API configuration
3. The exact `dotnet ef migrations add` command
4. Rollback plan
```

---

### Use Case 5: Blazor Component Generation

**Prompt:**
```
Build a reusable Blazor Server component called <PagedTable TItem="T">
- Generic over T
- Props: Items (IEnumerable<T>), PageSize (int, default 10), Columns (RenderFragment)
- Features: client-side paging, column sort, empty state
- Use Tailwind classes (assume Tailwind is configured)
- Include a usage example with a Product list
```

---

### Use Case 6: Performance Diagnosis

**Prompt:**
```
This endpoint takes 4 seconds for 1000 rows. Diagnose and fix:
[paste controller + service + query]

Consider: N+1 queries, missing indexes, async anti-patterns, overfetching.
Propose 3 optimizations ranked by impact vs effort.
```

---

## 8. Python Use Cases

### Use Case 1: FastAPI Microservice

**Prompt:**
```
Act as a Python backend engineer.

Build a FastAPI microservice for a URL shortener:
- POST /shorten  → returns short code
- GET /{code}    → 302 redirect
- GET /stats/{code} → click count
- Storage: Redis (use `redis-py` async client)
- Pydantic v2 models
- Dependency injection for Redis client
- Pytest + httpx.AsyncClient tests
- Dockerfile (multi-stage, python:3.12-slim)

Output project tree first, then each file in full.
```

---

### Use Case 2: Pandas Data Wrangling

**Prompt:**
```
I have a CSV with columns: order_id, customer_id, item, qty, unit_price, order_date (string 'DD/MM/YYYY').

Write a Pandas script that:
1. Loads the CSV with proper dtype inference and date parsing
2. Flags rows where qty <= 0 or unit_price <= 0
3. Computes revenue = qty * unit_price
4. Produces monthly revenue per customer
5. Saves a pivot (customers × months) to Excel with a total row
6. Handles missing values gracefully with logging

Use type hints and a main() function.
```

---

### Use Case 3: Test Generation with Pytest

**Prompt:**
```
Here is a function:
[paste]

Write pytest tests that:
- Use parametrize for boundary cases
- Include a freezegun fixture where time matters
- Achieve 100% branch coverage
- Add a hypothesis-based property test for invariants
```

---

### Use Case 4: Django Model & Admin

**Prompt:**
```
Create a Django app `inventory` with:
- Model: Warehouse (name, location, capacity)
- Model: StockItem (warehouse FK, sku, quantity, reorder_level)
- Admin: list_display, search, inline StockItems on Warehouse
- Migration file
- DRF viewset + serializer for StockItem with filter by warehouse
- Signal that emails admins when quantity <= reorder_level
```

---

### Use Case 5: ML Pipeline Skeleton

**Prompt:**
```
Build a scikit-learn pipeline for a binary classification problem:
- Load data from parquet
- Train/val/test split stratified on `label`
- Preprocessing: numeric (impute median + scale), categorical (impute mode + one-hot)
- Model: LogisticRegression with class_weight='balanced'
- Cross-validation with StratifiedKFold
- Report: accuracy, precision, recall, F1, ROC-AUC, confusion matrix plot
- Save model with joblib and a model card (markdown)
```

---

### Use Case 6: Async Web Scraper

**Prompt:**
```
Build an async scraper using httpx + asyncio + BeautifulSoup:
- Input: list of URLs from urls.txt
- Concurrency: 10 (use asyncio.Semaphore)
- Retries: 3 with exponential backoff (use tenacity)
- Extract: <title>, <meta description>, first <h1>
- Output: JSONL with url, title, description, h1, fetched_at
- Respect robots.txt
- Structured logging with loguru
```

---

## 9. Prompt Engineering Playbook

### 🎯 Anatomy of a Great Coding Prompt

```
[ROLE]       Act as a senior Python developer specializing in FastAPI.
[CONTEXT]    We use Python 3.12, SQLAlchemy 2.0 (async), Pydantic v2.
[TASK]       Create a /users/me endpoint that returns the current user.
[INPUTS]     JWT in Authorization header.
[CONSTRAINTS] No global state. Reuse get_current_user dependency.
[OUTPUT]     Single Python file + pytest test in a separate block.
[STYLE]      PEP 8, type hints, docstrings, no comments on obvious lines.
```

### ✍️ 10 Prompt Patterns That Work

1. **Few-shot**: Show 1-2 examples of the style you want.
2. **Chain-of-thought**: "First outline your approach, then write code."
3. **Role-play**: "Act as a security engineer reviewing this PR."
4. **Constraint-first**: List non-negotiables before the ask.
5. **Tree-of-options**: "Give me 3 approaches with trade-offs, then recommend one."
6. **Rubber duck**: "Explain what this code does, then propose improvements."
7. **Test-first**: "Write the tests first. Then the implementation to pass them."
8. **Diff mode**: "Return only a unified diff against this file."
9. **Self-critique**: "Now review your own output and fix 3 issues."
10. **Stepwise**: "Do this in stages. Show me stage 1 and wait for approval."

### 🐛 The Golden Rule of Debugging Prompts

> **Describe the bug or desired behavior — NOT the fix.**

| ❌ Weak | ✅ Strong |
|---|---|
| "Add a WHERE clause to filter by created_at" | "The pagination returns duplicates when items are inserted during traversal. Expected: each item appears exactly once across all pages." |
| "Use async/await here" | "This endpoint blocks the thread pool under 100 concurrent requests. P99 latency spikes to 8s." |
| "Add null check" | "The app crashes with NullReferenceException when a user has no assigned department." |

Describing symptoms lets the AI consider the *right* fix, not just the one you guessed.

### 📏 The Specificity Ladder

The same request, leveled up:

```
Level 1 (weak):    "Write a login endpoint."
Level 2 (ok):      "Write a POST /auth/login endpoint that returns a JWT."
Level 3 (good):    "In auth/controller.py, implement POST /auth/login accepting
                    email + password as JSON, validating via Pydantic, hashing
                    with bcrypt, returning {token, expires_at} or 401."
Level 4 (great):   Level 3 + "Follow the patterns in users/controller.py.
                    Add pytest tests covering success, bad creds, rate limit.
                    Respect the security rules in specs/how-security.md."
```

**Warning:** Don't over-specify implementation details — let the model make reasonable choices within your constraints.

### 🚫 Prompt Smells

| Smell | Fix |
|---|---|
| "Make it better" | State *what* better means: faster, shorter, typed, tested. |
| "Write some code" | Define the public interface and the success criteria. |
| "Fix the bug" | Paste the error + reproduction steps + expected behavior. |
| Wall of code, no context | Add the stack, versions, and the actual goal. |
| "Optimize this" | Profile first — tell the AI *what* is slow and *by how much*. |

---

## 10. Pattern Drift & Context Collapse

These are the **two most-reported failure modes** in 2026 vibe coding. Knowing them saves weeks of debugging.

### 🌀 Pattern Drift

> *Each feature gets built with slightly different patterns because the AI has no memory of what it built before.*

**Symptoms:**
- Three different error-handling styles in one codebase
- Same utility function written 4 different ways
- Naming conventions that change mid-project (`getUserById` → `fetchUser` → `retrieveUserRecord`)
- 4× more code duplication than human-written projects *(GitClear, 2025)*

**Root cause:** No persistent project context. Each prompt re-invents conventions.

**Fix:**
- `CLAUDE.md` / `AGENTS.md` with explicit conventions
- Spec files in `/specs` loaded per task
- A `LessonsLearned.md` that grows over time
- Run a linter + formatter on every change (CI-enforced)

---

### 💥 Context Collapse

> *Picture this: your agent just renamed a service method to `getTasksByProject`, and two prompts later it calls `fetchProjectTasks` as if the rename never happened.*

**Symptoms:**
- Agent contradicts its own earlier decisions
- Function calls reference methods that don't exist anymore
- Imports from files that were deleted
- Quality suddenly drops mid-session

**Root cause:** Context window exhausted (typically ~170-200k tokens of usable budget). The LLM quite literally cannot "remember" earlier turns.

**Fix — "Frequent Intentional Compaction":**
1. Start **fresh sessions** for new features — don't let sessions grow for hours
2. Keep a running `progress.md` summarizing decisions made
3. At session start, feed `CLAUDE.md` + current spec + `progress.md`
4. Keep prompts **modular** — one concern per prompt
5. When you feel the model "slipping," stop and compact: ask it to summarize state, then start over with that summary

---

### 🔢 The 2026 Data on AI-Generated Code Quality

Based on industry research cited by CodeRabbit, GitClear, and arXiv studies:

| Metric | Without structure | With structured vibe coding |
|---|---|---|
| Issue rate vs human code | **~70% higher** | Comparable |
| Code duplication | **~4× higher** | Near-parity |
| Security vulnerabilities | **~40% of generated code** contains at least one | Dramatically lower with security specs + SAST |

The takeaway: **the methodology works — it just needs guardrails.**

---

## 11. Anti-Patterns to Avoid

### ❌ The "Trust Fall"
Accepting code without reading it. Always review — especially for:
- Auth / authorization
- SQL / NoSQL queries (injection risk)
- File I/O and path handling
- Crypto and secrets
- Concurrency primitives

### ❌ The "Stack Overflow Copy-Paste" on Steroids
Pasting AI code across files without understanding the coupling. Leads to mysterious breakages.

### ❌ The "Hallucinated API"
AI invents methods/packages that don't exist. Verify by running, not reading.

### ❌ The "Prompt Spiral"
Re-prompting the same model with "no that's wrong" without new information. Start fresh with better context instead.

### ❌ The "Tests Are Optional"
Skipping tests because the code "looks right." AI-generated code especially needs tests — you didn't write it, so you don't have the mental model.

### ❌ The "Blank Canvas" Start
Asking AI to build everything from scratch with no architectural context. This is the single biggest driver of pattern drift. **Always start from a boilerplate, starter kit, or an existing project with a populated `CLAUDE.md`.**

### ❌ The "Giant Leap" Prompt
Asking for 5 features in one prompt. LLMs excel at **small, contained tasks**. Break big asks into 30-60 minute chunks, each with its own verification step.

### ❌ The "Green Test" Illusion
Accepting code because the test passes. **A test that validates the wrong behavior is worse than no test at all.** Review the test logic, not just the pass/fail.

### ❌ The "Marathon Session"
Running a single AI conversation for 6 hours. Context degrades; the agent forgets early decisions. **Start fresh sessions** at feature boundaries — reload `CLAUDE.md` + the relevant spec.

### ❌ The "Auto-Accept Everything"
Setting your agent to auto-approve all changes in production code. Keep **Plan Mode / Review Policy = manual** for anything that ships to users.

---

## 12. Best Practices & Governance

### ✅ For Individuals

- **Read every line** before committing.
- **Run it locally** — never commit unrun code.
- **Keep prompts in a file** (`prompts.md`) — they're as valuable as code.
- **Start small** — single function, single file, then scale.
- **Pair AI with version control** — commit often, easy to revert.

### 🏢 For Teams

| Area | Guideline |
|---|---|
| **Secrets** | Never paste `.env`, tokens, or customer PII into prompts. |
| **IP** | Confirm your AI tool's data retention policy. |
| **Code review** | AI-generated PRs need stricter review, not lighter. |
| **Licensing** | Ask tools to avoid verbatim copyrighted code. |
| **Attribution** | Some teams tag AI-assisted commits (`[ai-assisted]`). |
| **Testing** | Require tests for all AI-generated logic paths. |
| **Critical paths** | Auth, billing, data deletion → no vibe coding without senior review. |

### 🔒 Security Checklist

- [ ] No secrets in prompts
- [ ] Generated SQL uses parameterization
- [ ] Input validation present
- [ ] Dependencies verified (not hallucinated)
- [ ] No hardcoded credentials
- [ ] Error messages don't leak internals

---

## 13. Hands-On Demo Script

### 🐍 Python Demo (15 min)

**Build a FastAPI "todo" service from zero.**

**Step 1 — Skeleton (1 prompt):**
```
Create a FastAPI project for a todo service. Python 3.12, SQLite, SQLAlchemy async.
Endpoints: CRUD on /todos. Pydantic v2 schemas. Folder structure first.
```

**Step 2 — Tests (1 prompt):**
```
Add pytest + httpx async tests covering all endpoints,
including 404 on missing todo and 422 on bad payload.
```

**Step 3 — Feature add (1 prompt):**
```
Add a /todos/{id}/complete endpoint that toggles done=True and sets completed_at=utcnow().
Update the tests.
```

**Step 4 — Refactor (1 prompt):**
```
Extract business logic into a TodoService class. Keep routes thin. Inject via Depends.
```

**Step 5 — Ship (1 prompt):**
```
Add a Dockerfile (multi-stage) and a docker-compose.yml with a postgres service.
Switch the app to postgres via env var DATABASE_URL.
```

---

### 🔷 .NET Demo (15 min)

**Build a Minimal API "notes" service.**

**Step 1:**
```
Scaffold a .NET 8 Minimal API for Notes (CRUD) with EF Core + SQLite.
Project + Program.cs + Note entity + NoteDto + endpoints.
```

**Step 2:**
```
Add FluentValidation and a global exception handler returning ProblemDetails.
```

**Step 3:**
```
Add xUnit integration tests using WebApplicationFactory with an in-memory SQLite DB.
```

**Step 4:**
```
Add JWT bearer auth; only authenticated users see/modify their own notes.
Add a UserId claim scoping filter.
```

**Step 5:**
```
Produce a GitHub Actions workflow: restore, build, test, publish on main.
```

---

## 14. Key Takeaways

### 🎯 The 7 Rules of Structured Vibe Coding (2026 Edition)

1. **You are the architect, not the scribe.**
2. **Context is king** — `CLAUDE.md`/`AGENTS.md` + specs + examples.
3. **Never start from a blank canvas** — use a boilerplate, existing project, or scaffold first.
4. **Describe the symptom, not the fix** when debugging.
5. **Small chunks** — 30-60 min tasks, not full features in one prompt.
6. **Verify, don't trust** — run it, test it, read it.
7. **Never vibe-code what you don't understand** in production paths.

### ✈️ Pre-Flight Checklist: Starting a New Vibe-Coded Project

Before writing your first real prompt:

- [ ] `CLAUDE.md` / `AGENTS.md` committed with stack, conventions, pitfalls
- [ ] `/specs` folder with at least one `what-*.md` and one `how-security.md`
- [ ] Linter + formatter configured (`ruff`/`mypy` for Python, `dotnet format` for .NET)
- [ ] Test framework set up and runnable (`pytest` / `dotnet test`)
- [ ] CI pipeline that runs tests + lint on every push
- [ ] Pre-commit hooks (optional but recommended)
- [ ] Secrets management (`.env.example` committed, `.env` gitignored)
- [ ] `LessonsLearned.md` (starts empty, grows over time)

### 🧗 The Growth Curve

```
Week 1:  "Wow, it writes code!"
Week 2:  "Why does it keep hallucinating?"
Week 4:  "Better prompts → better code."
Week 8:  "I'm shipping 3× more."
Week 12: "My CLAUDE.md is my most valuable file."
Week 24: "I know exactly when NOT to use it."
Week 52: "Specs are my real deliverable; code is just output."
```

### 📚 Recommended Next Steps

- ✅ Install Claude Code, Cursor, or Copilot today
- ✅ Create a `CLAUDE.md` for one existing project this week
- ✅ Pick one boring task and vibe-code it end-to-end with the structured workflow
- ✅ Keep a `prompts.md` and `LessonsLearned.md` in your repo
- ✅ Do a team retro: "What did AI help with? What did it break?"
- ✅ Share prompt recipes and context files across your team

---

## 🙌 Q & A

> *"The best developers of 2026 aren't those who type fastest — they're those who think clearest and prompt sharpest."*

### 📖 Further Reading

- **Andrej Karpathy's original "vibe coding" tweet** (Feb 2025)
- **Sean Grove — "Specs are the New Code"** (AI Engineer 2025)
- **Addy Osmani — "My LLM coding workflow going into 2026"** (Medium)
- **Martin Fowler — "Context Engineering for Coding Agents"** (martinfowler.com)
- **Red Hat — "Vibes, specs, skills, and agents: The four pillars of AI coding"**
- **Claude Code docs:** https://docs.claude.com/en/docs/claude-code
- **Anthropic prompting guide:** https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview
- **Google Cloud — "Vibe Coding Explained"**

---

*Presentation prepared for developer audiences working with .NET and Python. Feel free to fork, adapt, and share.*

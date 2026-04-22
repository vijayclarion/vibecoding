# 🎯 Vibe Coding & Spec-Driven Development for Developers

> *A practical 2026 guide for .NET and Python developers to harness AI-assisted development with discipline*

---

## 📑 Table of Contents

1. [What is Vibe Coding?](#1-what-is-vibe-coding)
2. [Why Vibe Coding Matters](#2-why-vibe-coding-matters)
3. [The Vibe Coding Mindset](#3-the-vibe-coding-mindset)
4. [Tools & Frameworks](#4-tools--frameworks)
5. [Structured Vibe Coding: Context Is King](#5-structured-vibe-coding-context-is-king)
6. [Spec-Driven Vibe Coding (SDD)](#6-spec-driven-vibe-coding-sdd)
7. [The SDD Workflow: Greenfield & Legacy](#7-the-sdd-workflow-greenfield--legacy)
8. [Building the Constitution: Mission, Tech-Stack, Roadmap](#8-building-the-constitution-mission-tech-stack-roadmap)
9. [Feature Specification Workflow](#9-feature-specification-workflow)
10. [The Vibe Coding Workflow](#10-the-vibe-coding-workflow)
11. [.NET Use Cases](#11-net-use-cases)
12. [Python Use Cases](#12-python-use-cases)
13. [Prompt Engineering Playbook](#13-prompt-engineering-playbook)
14. [Pattern Drift & Context Collapse](#14-pattern-drift--context-collapse)
15. [Anti-Patterns to Avoid](#15-anti-patterns-to-avoid)
16. [Best Practices & Governance](#16-best-practices--governance)
17. [Hands-On Demo Script](#17-hands-on-demo-script)
18. [Key Takeaways](#18-key-takeaways)

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

## 6. Spec-Driven Vibe Coding (SDD)

> *"Specifications don't serve code — code serves specifications. The spec isn't a guide for implementation; it's the source that generates implementation."* — GitHub Spec Kit

**Spec-Driven Development (SDD)** is the natural evolution of structured vibe coding. Instead of prompting ad-hoc, you write **living, executable specifications** that become the source of truth for both humans and AI agents. The AI doesn't just generate code — it generates **artifacts** (specs, plans, tasks) that you review and refine before a single line of code is written.

### 🎯 Why SDD Fixes Vibe Coding's Biggest Problems

| Problem | How SDD Fixes It |
|---|---|
| **Pattern drift** | Constitution enforces non-negotiable principles on every change |
| **Context collapse** | Specs live in files, not the chat window — reload them anytime |
| **"Looks right but doesn't work"** | Plan & tasks are reviewed before implementation starts |
| **Scattered requirements** | Security, compliance, and design rules live in one versioned place |
| **Agent hallucinations** | The agent reads your specs, not guesses |
| **Team misalignment** | Specs are the shared language between PMs, devs, and AI |

### 📂 The SDD Artifact Hierarchy

```
your-project/
├── .specify/ or specs/             ← SDD's "brain"
│   ├── constitution.md             ← immutable principles (the "why")
│   ├── mission.md                  ← what we're building + for whom
│   ├── tech-stack.md               ← how we're building it
│   ├── roadmap.md                  ← phased plan of features
│   └── features/
│       └── 001-user-auth/
│           ├── spec.md             ← feature intent + acceptance criteria
│           ├── plan.md             ← technical approach
│           ├── tasks.md            ← atomic, ordered implementation steps
│           └── validation.md       ← how we'll verify it works
├── src/                            ← the actual code (last-mile)
└── CLAUDE.md / AGENTS.md           ← agent entry point, points at specs/
```

### 🛠️ Two SDD Implementations Worth Knowing

**1. GitHub Spec Kit** (the most widely adopted standard)

Install via:
```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

Workflow uses slash commands in your AI agent:
```
/speckit.constitution  → establish principles
/speckit.specify       → define what to build
/speckit.clarify       → resolve [NEEDS CLARIFICATION] items
/speckit.plan          → technical plan (architecture, deps)
/speckit.tasks         → break plan into atomic tasks
/speckit.analyze       → validate consistency between artifacts
/speckit.implement     → hand off to agent for execution
```

Works with GitHub Copilot, Claude Code, Gemini CLI, Cursor, Windsurf.

**2. DeepLearning.AI / JetBrains Workflow** (lighter, conversational)

Uses a three-file **Constitution** (`mission.md` + `tech-stack.md` + `roadmap.md`) driven by conversation with the agent. Works with any AI coding tool — no CLI required.

---

## 7. The SDD Workflow: Greenfield & Legacy

### 🌱 Greenfield Project (New from Scratch)

```
┌─────────────────────────────────────────────────────────────┐
│  1. CONSTITUTION  →  mission.md + tech-stack.md + roadmap.md│
│  2. PICK FEATURE  →  next item from roadmap phase 1         │
│  3. SPECIFY       →  spec.md (what) + plan.md (how)         │
│  4. CLARIFY       →  resolve ambiguities with the agent     │
│  5. TASKS         →  break plan into atomic steps           │
│  6. IMPLEMENT     →  agent executes; you review per task    │
│  7. VALIDATE      →  run validation.md checks               │
│  8. COMMIT & MERGE                                          │
│  9. REPLAN        →  update roadmap, return to step 2       │
└─────────────────────────────────────────────────────────────┘
```

### 🏚️ Legacy / Brownfield Project (Existing Codebase)

The exact same workflow — but the agent **reverse-engineers** the constitution from what's already there.

```
┌─────────────────────────────────────────────────────────────┐
│  1. REVERSE-ENGINEER CONSTITUTION                           │
│     - Point agent at README.md, TODO.md, codebase, issues   │
│     - Agent explores code, commits, configs                 │
│     - Agent proposes mission / tech-stack / roadmap         │
│     - You clarify, correct, commit                          │
│  2. From here: SAME as greenfield (steps 2-9 above)         │
└─────────────────────────────────────────────────────────────┘
```

**Key insight:** Your existing issue tracker, README, TODOs, and even commit history become *inputs* the agent uses to draft the constitution. You don't start from zero — you formalize what's already implicit.

### ✅ The Human-in-the-Loop Checkpoints

SDD builds in **four explicit review gates** where you critique before proceeding:

| Gate | You verify |
|---|---|
| After **Constitution** | Mission, stack, and roadmap match your intent |
| After **Specify** | The feature description captures what you actually want |
| After **Plan** | Technical approach respects constraints, no major gaps |
| After **Tasks** | Breakdown is atomic enough, ordering is correct |

**Skip these at your peril** — each checkpoint prevents downstream rework that costs 10×.

---

## 8. Building the Constitution: Mission, Tech-Stack, Roadmap

The Constitution is your project's **source of truth**, captured in three files. Let's build each from scratch.

### 📜 8.1 The Seed Prompt (Kickoff Conversation)

Whether you're using Spec Kit or the lightweight approach, you start with a conversation. Here's the canonical kickoff prompt:

```
I want to build a new project. Act as a senior architect and help me write
its Constitution — three files: mission.md, tech-stack.md, and roadmap.md.

Project description:
<paste 1-3 paragraphs describing what you want to build>

Stakeholder inputs (optional):
<paste any existing README, pitch deck, PRD, or notes>

Please:
1. Ask me clarifying questions ONE AT A TIME about:
   - target audience & success metrics
   - architectural trade-offs (speed vs fidelity, cost vs features)
   - tech stack preferences & constraints
   - feature priorities for an MVP
2. Organize the roadmap in small phases (2-4 features per phase)
3. After our conversation, create the three files in /specs/
4. Flag any assumptions with [ASSUMPTION] so I can verify them
```

### 📄 8.2 `mission.md` — What & Why (NO Tech Details)

**Purpose:** High-level product intent. A new team member should understand *what* you're building and *for whom* in 5 minutes.

**Template:**
```markdown
# Mission: <Project Name>

## The Problem
<One paragraph. Who has the pain? What does it cost them today?>

## Our Solution
<One paragraph. What does the product do at a high level?>

## Target Audience
- Primary: <persona>, <use case>
- Secondary: <persona>, <use case>

## Core Principles
- <principle #1 — e.g., "Privacy by default">
- <principle #2 — e.g., "Works offline-first">
- <principle #3 — e.g., "10-second onboarding">

## Success Metrics
- <metric> — e.g., "User completes first task within 60 seconds"
- <metric> — e.g., "P95 latency < 200ms"

## Non-Goals
<What we explicitly will NOT do. This is as important as what we will.>
- e.g., "We are not building a general-purpose CRM."
- e.g., "No self-hosted option in v1."

## Glossary
- **Term** — definition (so the agent uses your vocabulary consistently)
```

**How to build it from raw inputs:**
1. Paste your raw idea + any existing docs into the chat.
2. Use the seed prompt above.
3. When the agent asks about tone, audience, or non-goals — **answer every question**.
4. Review the draft. Missing audience? Missing non-goals? Ask the agent to edit (don't edit manually — keeps artifacts in sync).
5. Commit to Git.

---

### 🔧 8.3 `tech-stack.md` — How We Build It

**Purpose:** Key architecture decisions separated from product intent. Same product can be built many ways — this locks in your choices.

**Template:**
```markdown
# Tech Stack: <Project Name>

## Languages & Runtimes
- Backend: <.NET 8 / Python 3.12 / Node 20>
- Frontend: <React 18 + TypeScript / Blazor / HTMX>
- Database: <PostgreSQL 16 / SQLite / SQL Server>

## Key Frameworks & Libraries
- <Framework + version + why chosen>
- e.g., "ASP.NET Core Minimal APIs — faster cold starts than MVC for our use case"
- e.g., "FastAPI — native async, auto OpenAPI, team familiarity"

## Architecture
- Pattern: <Monolith / Modular monolith / Microservices>
- Boundaries: <list bounded contexts / modules>
- Communication: <REST / gRPC / events>
- Deployment target: <Azure App Service / AWS ECS / K8s / serverless>

## Data Model (High-Level)
<Sketch of main entities & relationships — the agent will elaborate later>

## Request Lifecycle
<For a key endpoint, trace the full pipeline: validation → auth → service → repo → DB>

## Cross-Cutting Concerns
- **Authentication:** <JWT / OAuth2 / API keys>
- **Authorization:** <role-based / policy-based>
- **Logging:** <Serilog → Seq / structlog → Loki>
- **Observability:** <OpenTelemetry + Jaeger>
- **Error handling:** <ProblemDetails / Problem+JSON>
- **Configuration:** <env vars + dotnet user-secrets / Pydantic Settings>
- **Secrets:** <Azure Key Vault / AWS Secrets Manager>

## Testing Strategy
- Unit: <framework, coverage target>
- Integration: <WebApplicationFactory / httpx.AsyncClient>
- E2E: <Playwright / Selenium>

## Quality Gates
- Linter: <dotnet format / ruff>
- Type checker: <Roslyn analyzers / mypy --strict>
- Security: <Snyk / dotnet list package --vulnerable>

## Constraints & Non-Functional Requirements
- Performance: P95 < <X>ms
- Availability: <99.9% / best-effort>
- Compliance: <SOC 2 / GDPR / none>
- Browser support: <latest 2 Chrome, Firefox, Safari>

## Open Questions / Pending Decisions
- [ ] <unresolved item>
```

**How to build it from raw inputs:**
1. After `mission.md` is committed, ask the agent:
   ```
   Now let's write tech-stack.md. Based on our mission and these constraints:
   - <team experience: "5 devs, all TypeScript + Python">
   - <existing infra: "Azure tenant, prefer managed PG">
   - <non-functional: "must handle 10k RPS eventually">
   Ask me the key architectural questions one at a time, then draft tech-stack.md.
   ```
2. Expect questions like: *"Speed vs data fidelity for reads?" "Strong vs eventual consistency across services?" "Config flags vs env vars for the LLM provider?"*
3. For anything you're unsure about, answer with "leave configurable" or "[DEFERRED]" — you can decide later.
4. Commit.

---

### 🗺️ 8.4 `roadmap.md` — Phased Feature Plan

**Purpose:** The sequence of features that turns mission → MVP → v1.0. Organized in **small phases** so the agent ships in digestible chunks.

**Template:**
```markdown
# Roadmap: <Project Name>

## Phase 0 — Foundation
- [ ] F0.1 — Project skeleton (repo, CI/CD, healthcheck endpoint)
- [ ] F0.2 — Database schema + migrations setup
- [ ] F0.3 — Logging + observability baseline

## Phase 1 — MVP Core (the walking skeleton)
- [ ] F1.1 — <smallest feature that proves the value prop end-to-end>
- [ ] F1.2 — <next>
- [ ] F1.3 — <next>
**Exit criteria:** <what "done" looks like for Phase 1>

## Phase 2 — MVP Polish
- [ ] F2.1 — Auth
- [ ] F2.2 — Dashboard
**Exit criteria:** <...>

## Phase 3 — v1.0 (shippable)
...

## Deferred / Parking Lot
- <features we discussed but won't build soon — with reason>
```

**How to build it from raw inputs:**
1. With `mission.md` and `tech-stack.md` committed, prompt:
   ```
   Now let's write roadmap.md. Break our mission into 3-4 phases.
   - Phase 1 should be the thinnest end-to-end slice that proves core value.
   - Each feature should be implementable in 1-3 days.
   - Ask me how granular to make it, then draft.
   ```
2. Review every feature — can it be built in one focused session? If no, ask the agent to split it.
3. **Define explicit exit criteria** for each phase. Without these, scope creep is guaranteed.
4. Commit.

---

### 🏚️ 8.5 Reverse-Engineering the Constitution (Existing Projects)

For an existing project, the inputs change but the process is the same.

**The seed prompt:**
```
I have an existing codebase that I want to bring under Spec-Driven Development.
Your job is to reverse-engineer its Constitution from what's already there.

Inputs available:
- Source code (you have read access to this repo)
- README.md — high-level project description
- TODO.md — planned work
- <any other artifacts: ADRs, old PRDs, issue tracker exports>

Please:
1. Explore the codebase to understand the architecture. Look at:
   - project structure, build files, dependency manifests
   - main entry points
   - config files (how it's deployed)
   - test patterns
2. Read the README, TODO, and any /docs files.
3. Draft mission.md, tech-stack.md, roadmap.md in /specs/
4. Flag every inference with [REVERSE-ENGINEERED - please confirm]
5. Ask me clarifying questions about:
   - target audience (probably not obvious from code)
   - success metrics
   - prioritization of the TODO items
```

**What happens next:**
- Agent performs many tool calls (reading files, searching code).
- Agent drafts all three files with clearly marked inferences.
- You correct, clarify, and commit.
- From here, the workflow is **identical** to greenfield.

**Tip:** The richer the existing artifacts (issues, ADRs, old Confluence pages), the richer the conversation. Paste them in.

---

## 9. Feature Specification Workflow

Once the Constitution is committed, you implement features **one at a time** via a three-artifact workflow: **spec → plan → tasks → implement**.

### 🔄 The Feature Loop

```
Pick next feature from roadmap
        │
        ▼
┌──────────────────────┐
│ 1. CREATE BRANCH     │   git checkout -b feature/001-user-auth
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 2. SPECIFY           │   /specify  → spec.md (what + acceptance criteria)
│    Conversational:   │              + requirements
│    answer agent's    │              + validation scorecard
│    questions         │
└──────────┬───────────┘
           ▼      🛑 HUMAN REVIEW GATE
┌──────────────────────┐
│ 3. CLARIFY           │   Resolve [NEEDS CLARIFICATION] items
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 4. PLAN              │   /plan → plan.md (architecture, data model)
│                      │          + research.md if new deps needed
└──────────┬───────────┘
           ▼      🛑 HUMAN REVIEW GATE
┌──────────────────────┐
│ 5. TASKS             │   /tasks → tasks.md (atomic ordered steps)
│                      │            [P] = parallelizable
└──────────┬───────────┘
           ▼      🛑 HUMAN REVIEW GATE
┌──────────────────────┐
│ 6. ANALYZE (optional)│   /analyze — check consistency across artifacts
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 7. IMPLEMENT         │   Agent executes tasks one by one
│    You approve each  │   Review each diff, run tests
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 8. VALIDATE          │   Run validation.md checks
│                      │   Manual smoke test
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 9. COMMIT + MERGE    │   Specs go in with the code
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│ 10. REPLAN           │   Update roadmap, capture lessons
└──────────────────────┘
```

### 📄 9.1 `spec.md` — Feature Intent

```markdown
# Feature: <Name> (F<N.N>)

## User Story
As a <role>, I want to <action>, so that <benefit>.

## Acceptance Criteria
- [ ] Given <precondition>, when <action>, then <outcome>
- [ ] <edge case 1>
- [ ] <edge case 2>

## Requirements
- Functional: <list>
- Non-functional: <perf, security, a11y>
- Out of scope: <explicitly NOT doing>

## Dependencies
- Prerequisite features: <F0.2 DB schema>
- External: <Stripe, SendGrid>

## Validation Scorecard
- [ ] Unit tests cover every branch
- [ ] Integration test for happy path + 2 error paths
- [ ] Manual smoke test steps documented
- [ ] No regressions in existing tests
```

### 🏗️ 9.2 `plan.md` — Technical Approach

```markdown
# Plan: <Feature Name>

## Approach
<1 paragraph summary of how we'll build it>

## Files to Create
- `src/features/auth/LoginEndpoint.cs` (or .py)
- `src/features/auth/AuthService.cs`
- `tests/features/auth/LoginTests.cs`

## Files to Modify
- `Program.cs` — register new service
- `appsettings.json` — add JWT config section

## Data Model Changes
<any DB schema additions / migrations needed>

## API Contract
<request/response shapes, status codes>

## Risks & Mitigations
- Risk: <rate limiting at login endpoint>
- Mitigation: <add fixed-window rate limiter, 5/min/IP>
```

### ✅ 9.3 `tasks.md` — Atomic Steps

```markdown
# Tasks: <Feature Name>

## Setup
- [ ] T1. Add NuGet package `Microsoft.AspNetCore.Authentication.JwtBearer`

## Data Layer
- [ ] T2. Add `User` entity in `Domain/User.cs` [P]
- [ ] T3. Add migration `AddUsersTable`

## Service Layer
- [ ] T4. Create `IAuthService` interface
- [ ] T5. Implement `AuthService` with password hashing
- [ ] T6. Unit tests for `AuthService` (happy path + 3 edge cases)

## API Layer
- [ ] T7. Add `/auth/login` endpoint
- [ ] T8. Add `/auth/register` endpoint
- [ ] T9. Integration tests using `WebApplicationFactory`

## Wiring
- [ ] T10. Register services in `Program.cs`
- [ ] T11. Add JWT config to `appsettings.Development.json`

[P] = parallelizable with adjacent [P] tasks
```

### 🎯 9.4 Golden Rules for the Feature Loop

1. **Never skip the review gates** — 30 seconds of review saves 2 hours of undoing.
2. **Edit via the agent, not manually** — keeps `spec.md`, `plan.md`, `tasks.md` in sync.
3. **Commit specs with the code** — they're part of your history, not throwaway scaffolding.
4. **One feature, one branch** — mirror the SDD feature folder to the git branch.
5. **Let the agent ask questions** — if it stops asking, you're probably under-specifying.
6. **[NEEDS CLARIFICATION] is a feature** — the agent flagging uncertainty is exactly what you want.

---

## 10. The Vibe Coding Workflow

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

## 11. .NET Use Cases

### 🌱 Use Case 0a: SDD for a NEW .NET Project (End-to-End)

**Scenario:** Build `OrderService` — a new .NET 8 Minimal API for an e-commerce order management system.

#### Step 1: Create the repo & install Spec Kit (optional)

```bash
mkdir OrderService && cd OrderService
git init
# Option A: GitHub Spec Kit
uvx --from git+https://github.com/github/spec-kit.git specify init .
# Option B: Lightweight — just create the folder
mkdir -p specs/features
```

#### Step 2: Kickoff prompt (in Claude Code / Copilot / Cursor)

```
Act as a senior .NET architect. Help me write the Constitution for a new
project called OrderService.

Project description:
OrderService is a REST API for a mid-size e-commerce company to manage
customer orders: creation, payment, fulfillment, returns. It will replace
a creaky Node.js service that can't handle our Black Friday load.

Stakeholder inputs:
- Team: 4 .NET devs, all comfortable with C# 12 and EF Core
- Infra: Azure (App Service, Azure SQL, Application Insights)
- NFRs: P95 < 150ms, 99.9% uptime, PCI-DSS scope adjacent (no card storage)
- Must integrate with: existing ProductCatalog API, Stripe, SendGrid

Please:
1. Ask me clarifying questions ONE AT A TIME about audience, trade-offs,
   tech preferences, and MVP priorities.
2. Organize the roadmap into 3-4 phases, 2-4 features each.
3. When we're done, create /specs/mission.md, /specs/tech-stack.md,
   /specs/roadmap.md.
4. Flag assumptions with [ASSUMPTION].
```

#### Step 3: Expect ~8-12 questions

Typical questions from the agent:
- "What tone should `mission.md` take — formal, playful, neutral?"
- "Is this the single source of truth for orders, or does another system own parts?"
- "Authentication: who calls this API — internal services only, or external partners?"
- "Idempotency requirements for order creation?"
- "Event-driven (Service Bus) or synchronous calls to downstream services?"
- "Minimal APIs or MVC controllers?"
- "Dapper or EF Core?"
- "How granular should the roadmap be — tickets or phases?"

**Answer every one.** Say "deferred" or "leave configurable" when unsure.

#### Step 4: Review the three files

```
specs/
├── mission.md     ← product intent, audience, success metrics, non-goals
├── tech-stack.md  ← .NET 8, Minimal APIs, EF Core + Azure SQL, Serilog → App Insights, ...
└── roadmap.md     ← Phase 0: scaffold; Phase 1: CRUD orders; Phase 2: payment; ...
```

Something missing? Tell the agent — **don't edit manually**:
> "The mission doesn't mention our internal ops team as a secondary audience. Please add them and propagate any related changes to roadmap."

#### Step 5: Commit the Constitution

```bash
git add specs/
git commit -m "chore: establish project constitution (mission, tech-stack, roadmap)"
```

#### Step 6: Implement Feature F1.1

Open a **fresh chat session** (prevents context bloat). Use the feature spec prompt:

```
We're implementing feature F1.1 from /specs/roadmap.md:
"Create an order (POST /orders) with line items, returning 201 + order id."

Please:
1. Read /specs/mission.md, /specs/tech-stack.md, /specs/roadmap.md first.
2. Create a feature branch: feature/001-create-order
3. Draft /specs/features/001-create-order/spec.md with user story,
   acceptance criteria, requirements, validation scorecard.
4. Ask me about anything ambiguous. Use [NEEDS CLARIFICATION] markers.
```

#### Step 7: Plan → Tasks → Implement

After you approve `spec.md`:
```
Now draft plan.md — files to create/modify, data model changes, API contract,
risks. Keep it concise but specific.
```

After you approve `plan.md`:
```
Now draft tasks.md — atomic, ordered steps. Mark parallelizable tasks with [P].
Tests before implementation where it makes sense.
```

After you approve `tasks.md`:
```
Implement the tasks in order. Stop and show me the diff after each task.
I'll approve or ask for changes before you proceed.
```

#### Step 8: Validate & commit

```bash
dotnet test
dotnet format --verify-no-changes
git add .
git commit -m "feat(orders): F1.1 create order endpoint

See specs/features/001-create-order/ for full spec, plan, and tasks."
git checkout main && git merge feature/001-create-order
```

#### Step 9: Replan & repeat

Ask the agent:
> "Update `roadmap.md` — mark F1.1 complete, note any lessons learned for future features."

Then pick F1.2 and repeat from Step 6.

---

### 🏚️ Use Case 0b: SDD for an EXISTING .NET Project (Legacy/Brownfield)

**Scenario:** You inherit `LegacyERP` — a 4-year-old .NET Framework 4.8 Web API with 80k LOC, patchy docs, and a `TODO.md` full of open work.

#### Step 1: Reverse-engineer the Constitution

In a fresh Claude Code / Copilot session at the repo root:

```
I've just inherited this .NET codebase. I want to bring it under
Spec-Driven Development by reverse-engineering its Constitution.

Inputs available:
- The full source tree (you have read access)
- README.md — high-level overview
- TODO.md — open work I'm expected to tackle
- /docs/ — some old architecture notes (incomplete)

Please:
1. Explore the codebase:
   - Read .csproj / Directory.Build.props for framework versions + packages
   - Map the solution structure (projects, their references)
   - Identify main entry points (Global.asax, Startup.cs, Program.cs)
   - Scan /Controllers, /Services, /Repositories to infer architecture
   - Read web.config / appsettings.json for deployment hints
   - Note test framework + coverage patterns
2. Read README.md, TODO.md, and /docs/*.md
3. Draft these files in /specs/:
   - mission.md  — inferred product intent (flag guesses)
   - tech-stack.md — what IS, not what SHOULD BE
   - roadmap.md — phase the TODO items into logical groups
4. Mark EVERY inference with [REVERSE-ENGINEERED - confirm]
5. Ask me:
   - Who are the actual users?
   - What are the real pain points the TODO is trying to fix?
   - Are there non-functional requirements not visible in code?
```

#### Step 2: Expect many tool calls

The agent will do 20-50 read/search operations across the codebase. Let it run. It's building a mental model you never had to manually compile.

#### Step 3: Review & correct drafts

The agent might say:
> "[REVERSE-ENGINEERED - confirm] The test coverage in /tests/LegacyERP.IntegrationTests looks thin (32 tests for 80k LOC). Is raising coverage a goal?"

**This is gold.** It's surfacing the invisible debt. Add it to the roadmap.

#### Step 4: First modernization feature

Once the Constitution is committed, pick the smallest TODO item:

```
Next feature from roadmap: F1.1 "Migrate /api/customers endpoints from
EF6 to EF Core 8"

Please:
1. Read /specs/constitution files again.
2. Read the EXISTING /Controllers/CustomersController.cs and /Repositories/CustomerRepository.cs.
3. Draft spec.md, plan.md, tasks.md in /specs/features/001-ef-core-migration/
4. Specifically identify:
   - What can stay API-compatible (contract tests?)
   - What will break (EF6 lazy loading → EF Core explicit loading)
   - Migration order (parallel implementations? feature flag?)
```

#### Step 5: Same loop as greenfield

Spec → Plan → Tasks → Implement, with review gates at each step.

**Legacy-specific tips:**
- Use **contract tests** as regression insurance — have the agent capture current behavior as tests *before* refactoring.
- Feature flag risky migrations to enable side-by-side comparison.
- Update `LessonsLearned.md` aggressively — legacy projects have the richest lessons.
- Don't try to modernize everything — the Constitution helps you pick battles.

---

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

## 12. Python Use Cases

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

## 13. Prompt Engineering Playbook

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

## 14. Pattern Drift & Context Collapse

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

## 15. Anti-Patterns to Avoid

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

## 16. Best Practices & Governance

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

## 17. Hands-On Demo Script

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

## 18. Key Takeaways

### 🎯 The 7 Rules of Spec-Driven Vibe Coding (2026 Edition)

1. **The spec is the source of truth — code is the last-mile output.**
2. **Constitution before code:** mission, tech-stack, roadmap — always.
3. **Never start from a blank canvas** — scaffold or reverse-engineer first.
4. **Three review gates are non-negotiable:** spec, plan, tasks.
5. **Describe the symptom, not the fix** when debugging.
6. **Small chunks** — 30-60 min atomic tasks, not full features in one prompt.
7. **Edit artifacts via the agent** — keeps spec, plan, tasks in sync.

### ✈️ Pre-Flight Checklist: Starting a New Vibe-Coded Project

Before writing your first real prompt:

- [ ] `/specs/mission.md` committed (what + audience + non-goals)
- [ ] `/specs/tech-stack.md` committed (languages, frameworks, NFRs)
- [ ] `/specs/roadmap.md` committed (phased features with exit criteria)
- [ ] `CLAUDE.md` / `AGENTS.md` pointing the agent at `/specs/`
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
Week 16: "I started writing specs first — everything changed."
Week 24: "I know exactly when NOT to use it."
Week 52: "Specs ARE my real deliverable; code is just output."
```

### 📚 Recommended Next Steps

- ✅ Install Claude Code, Cursor, or Copilot today
- ✅ Install **GitHub Spec Kit** (`uvx --from git+https://github.com/github/spec-kit.git specify init <project>`)
- ✅ Pick one existing project and **reverse-engineer its Constitution** this week
- ✅ Pick one new project and **start with the Constitution** before any code
- ✅ Keep a `prompts.md` and `LessonsLearned.md` in your repo
- ✅ Do a team retro: "Where did our specs help? Where did they fall short?"
- ✅ Share Constitution templates and prompt recipes across your team

---

## 🙌 Q & A

> *"The best developers of 2026 aren't those who type fastest — they're those who think clearest, spec sharpest, and review hardest."*

### 📖 Further Reading

**Spec-Driven Development:**
- **GitHub Spec Kit** — https://github.com/github/spec-kit
- **GitHub Blog: "Spec-driven development with AI"** — https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
- **Microsoft Developer: "Diving Into Spec-Driven Development With GitHub Spec Kit"**
- **DeepLearning.AI course: "Spec-Driven Development with Coding Agents"** (with JetBrains)
- **Sean Grove — "Specs are the New Code"** (AI Engineer 2025)
- **Martin Fowler — "Understanding Spec-Driven Development"**

**Vibe Coding Foundations:**
- **Andrej Karpathy's original "vibe coding" tweet** (Feb 2025)
- **Addy Osmani — "My LLM coding workflow going into 2026"** (Medium)
- **Martin Fowler — "Context Engineering for Coding Agents"** (martinfowler.com)
- **Red Hat — "Vibes, specs, skills, and agents: The four pillars of AI coding"**

**Tool Documentation:**
- **Claude Code docs:** https://docs.claude.com/en/docs/claude-code
- **Anthropic prompting guide:** https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview
- **Google Cloud — "Vibe Coding Explained"**

---

*Presentation prepared for developer audiences working with .NET and Python. Feel free to fork, adapt, and share.*

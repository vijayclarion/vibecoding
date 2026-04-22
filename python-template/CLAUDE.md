# CLAUDE.md — Agent Entry Point

You are working on **<PROJECT_NAME>**, a Python 3.12 project.

## READ THESE FIRST (in order)

Before answering ANY question or making ANY change, read:

1. `/specs/mission.md` — what we're building and why
2. `/specs/tech-stack.md` — architecture, libraries, non-functional requirements
3. `/specs/roadmap.md` — phased feature plan and current status
4. The feature folder in `/specs/features/<current-feature>/` if working on a feature

If the user asks about something not covered in these files, **ask for clarification before guessing**.

## Core conventions (must always apply)

- **Language:** Python 3.12, type hints everywhere (`from __future__ import annotations` at top)
- **Style:** PEP 8, enforced via ruff + black
- **Async:** Use `async`/`await` for I/O. Never mix sync + async handlers.
- **Folder structure:**
  - `app/` — main package
    - `routers/` — FastAPI routers (thin, delegate to services)
    - `services/` — business logic
    - `models/` — SQLAlchemy ORM models
    - `schemas/` — Pydantic DTOs (separate from ORM models)
    - `repositories/` — data access
    - `core/` — config, security, logging setup
    - `dependencies.py` — DI wiring
  - `tests/` — mirrors `app/` structure
  - `migrations/` — Alembic
- **DTOs (`schemas/`) are separate from ORM models (`models/`)** — never return ORM objects from endpoints
- **Validation:** Pydantic v2 for inputs, FastAPI response_model for outputs
- **Errors:** raise `HTTPException` with structured detail; global handler maps to RFC 7807 Problem
- **Logging:** structlog or loguru, JSON format, include correlation id
- **No PII in logs** — ever

## Pitfalls to avoid

- No mutable default arguments (`def f(x=[])` is a bug)
- No broad `except:` — catch specific exceptions
- No circular imports — use forward refs or TYPE_CHECKING guards
- No `datetime.utcnow()` — use `datetime.now(UTC)` (Python 3.12+)
- No `requests` in async code — use `httpx.AsyncClient`
- No global DB sessions — inject via `Depends`

## Definition of Done

A task is complete only when:
- [ ] `ruff check .` passes (zero warnings)
- [ ] `ruff format --check .` passes
- [ ] `mypy --strict .` passes
- [ ] `pytest` passes (all tests)
- [ ] New logic has unit tests (happy path + ≥2 edge cases)
- [ ] Integration test covers the endpoint end-to-end (if an endpoint)
- [ ] Branch coverage ≥80% on new code
- [ ] Updated the relevant spec/plan/tasks files if anything deviated from plan

## Workflow rules

- **One feature = one branch = one feature folder** (`feature/NNN-short-name`)
- **Review gates:** stop and show me the diff after spec, plan, tasks, and each task during implementation
- **Edit docs via me** — never let me edit `spec.md`/`plan.md`/`tasks.md` manually
- **When stuck:** add `[NEEDS CLARIFICATION]` rather than guessing
- **After every bug or surprise:** propose an addition to `/LessonsLearned.md`

## Useful commands in this repo

```bash
uv sync                           # install deps
uv run pytest                     # run tests
uv run ruff check . --fix         # lint + fix
uv run ruff format .              # format
uv run mypy --strict .            # type check
uv run alembic revision --autogenerate -m "<msg>"   # new migration
uv run alembic upgrade head       # apply migrations
uv run uvicorn app.main:app --reload  # dev server
```

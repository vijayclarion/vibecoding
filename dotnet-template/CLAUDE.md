# CLAUDE.md — Agent Entry Point

You are working on **<PROJECT_NAME>**, a .NET 8 project.

## READ THESE FIRST (in order)

Before answering ANY question or making ANY change, read:

1. `/specs/mission.md` — what we're building and why
2. `/specs/tech-stack.md` — architecture, libraries, non-functional requirements
3. `/specs/roadmap.md` — phased feature plan and current status
4. The feature folder in `/specs/features/<current-feature>/` if working on a feature

If the user asks about something not covered in these files, **ask for clarification before guessing**.

## Core conventions (must always apply)

- **Language:** C# 12, .NET 8, nullable reference types enabled
- **Async everywhere** — no `.Result` or `.Wait()`, no `async void` except for event handlers
- **Folder structure:**
  - `/Domain` — entities, value objects, domain services
  - `/Application` — use cases, DTOs, interfaces
  - `/Infrastructure` — EF Core, external APIs, email, storage
  - `/Api` — endpoints, middleware, `Program.cs`
  - `/Tests/*.UnitTests` and `/Tests/*.IntegrationTests`
- **DTOs are separate from entities** — never return entities from endpoints
- **Validation:** FluentValidation, never DataAnnotations
- **Errors:** always return `ProblemDetails` on failure
- **Logging:** Serilog, structured (`logger.LogInformation("User {UserId} did {Action}", ...)`)
- **No PII in logs** — ever

## Pitfalls to avoid

- No direct `DbContext` usage in endpoints — always via a service
- No public setters on domain entities — use factory methods
- No `Dictionary<string, object>` bags — always strongly typed
- No `Task.Run` in ASP.NET Core request paths
- No `ConfigureAwait(false)` in ASP.NET Core — it's a no-op there

## Definition of Done

A task is complete only when:
- [ ] Code compiles with zero warnings
- [ ] `dotnet format --verify-no-changes` passes
- [ ] `dotnet test` passes (all tests)
- [ ] New logic has unit tests (happy path + ≥2 edge cases)
- [ ] Integration test covers the endpoint end-to-end (if an endpoint)
- [ ] No new Code Analysis warnings (CA rules)
- [ ] Updated the relevant spec/plan/tasks files if anything deviated from plan

## Workflow rules

- **One feature = one branch = one feature folder** (`feature/NNN-short-name`)
- **Review gates:** stop and show me the diff after spec, plan, tasks, and each task during implementation
- **Edit docs via me** — never let me edit `spec.md`/`plan.md`/`tasks.md` manually (keeps artifacts in sync)
- **When stuck:** add `[NEEDS CLARIFICATION]` rather than guessing
- **After every bug or surprise:** propose an addition to `/LessonsLearned.md`

## Useful commands in this repo

```bash
dotnet build                    # must be clean
dotnet test                     # must be green
dotnet format                   # auto-fix style
dotnet ef migrations add <Name> # create a migration
dotnet ef database update       # apply migrations
```

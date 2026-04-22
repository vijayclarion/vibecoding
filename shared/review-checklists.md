# Review Checklists

Use these checklists at each of SDD's four human-in-the-loop gates. Each gate exists to catch problems cheaply — before they propagate into code.

---

## Gate 1: Constitution Review

After the agent drafts `mission.md`, `tech-stack.md`, `roadmap.md`.

### mission.md

- [ ] Primary audience is identified and specific (not "developers" — *which* developers?)
- [ ] Problem statement is concrete (includes numbers, anecdotes, or quotes)
- [ ] Success metrics are measurable and time-bound
- [ ] **Non-goals section exists** and lists at least 3 items
- [ ] Core principles are ranked (conflicts resolve in favor of higher)
- [ ] Glossary exists for any non-obvious domain terms
- [ ] NO technology mentions leaked in (those belong in `tech-stack.md`)
- [ ] Constraints (regulatory, deadline, budget) captured

### tech-stack.md

- [ ] Languages, frameworks, and **versions** are specified
- [ ] Each library has a one-line "why chosen" justification
- [ ] Architecture pattern (monolith / modular monolith / microservices) is explicit
- [ ] Folder structure is sketched
- [ ] Request lifecycle is traced for at least one key endpoint
- [ ] All cross-cutting concerns covered (auth, logging, errors, config, secrets, observability)
- [ ] Testing strategy includes unit + integration + E2E (or explicit "no E2E for now")
- [ ] Quality gates listed (lint, format, typecheck, coverage target)
- [ ] Non-functional requirements have **concrete targets** (not "fast" but "P95 < 150ms")
- [ ] External dependencies listed with purpose
- [ ] Deployment pipeline sketched

### roadmap.md

- [ ] Phase 0 (Foundation) exists with CI, healthcheck, logging baseline
- [ ] Phase 1 is the **thinnest end-to-end slice** proving core value
- [ ] Every phase has explicit **exit criteria**
- [ ] Each feature is implementable in 1-3 focused sessions (if bigger, split)
- [ ] Parking Lot section lists deferred items with reasons
- [ ] No phase depends on a feature from a later phase

**Red flags:**
- Mission reads like a marketing page → too vague
- Tech-stack reads like "use best tools" → missing specifics
- Roadmap has "Phase 1: Build MVP" as a single bullet → not decomposed

---

## Gate 2: Feature Spec Review

After `spec.md` is drafted for a feature.

- [ ] User story follows "As a X, I want Y, so that Z" form
- [ ] Acceptance criteria are in Given/When/Then form (testable!)
- [ ] Both happy path AND error cases listed in ACs
- [ ] Functional requirements are specific (status codes, response shapes, invariants)
- [ ] Non-functional requirements have concrete targets
- [ ] **Out-of-scope section** exists (what this feature won't do)
- [ ] Dependencies on other features are explicit
- [ ] All `[NEEDS CLARIFICATION]` markers have been resolved
- [ ] Validation scorecard lists specific, checkable items

**Red flags:**
- "System shall be secure" → not specific enough
- ACs that can't be turned into tests → rewrite them
- No error cases → agent will forget to handle them

---

## Gate 3: Plan Review

After `plan.md` is drafted.

- [ ] Approach paragraph explains **why** this approach over alternatives
- [ ] Every file listed under "Files to Create" maps to a layer in `tech-stack.md`
- [ ] "Files to Modify" is explicit (no "update where needed")
- [ ] Data model changes include migration command
- [ ] API contract has request, response(s), **all error status codes**, headers
- [ ] At least 3 risks identified with likelihood + impact + mitigation
- [ ] Alternatives considered section has genuine rejected options
- [ ] Plan respects `tech-stack.md` conventions (no rogue library choices)
- [ ] Performance-sensitive paths are flagged

**Red flags:**
- "Files to modify: as needed" → agent doesn't have a concrete picture
- No risks listed → plan is naive
- Plan introduces a library not in `tech-stack.md` → should require amendment

---

## Gate 4: Tasks Review

After `tasks.md` is drafted.

- [ ] Tasks are **atomic** — each completable in ≤30-45 minutes
- [ ] Tasks are **ordered** by dependency
- [ ] Test tasks come BEFORE implementation where it makes sense (TDD)
- [ ] `[P]` markers make sense — parallelizable tasks touch different files
- [ ] Migration / schema tasks exist if data model changed
- [ ] Wiring tasks exist (dependency injection, router registration)
- [ ] Quality gate tasks at the end (lint, typecheck, coverage)
- [ ] Documentation tasks at the end (update specs if deviated)
- [ ] Commit / PR / merge / roadmap-update tasks at the very end

**Red flags:**
- A task says "implement everything" → too big, split
- No test tasks → agent will forget or skip
- No wiring task → code will exist but not be reachable

---

## Gate 5: Per-Task Review (during implementation)

After **each** task the agent completes.

- [ ] Diff is reasonable size (small = easy to review)
- [ ] Changes stay within the scope of the task (no drive-by refactors)
- [ ] New files are placed according to folder conventions
- [ ] Naming follows project conventions (see `CLAUDE.md`)
- [ ] Tests related to this task pass
- [ ] No new lint / type errors introduced
- [ ] No secrets, PII, or TODOs left in the diff
- [ ] Error handling matches project patterns
- [ ] Logs are structured and correlation-id-aware

**When to push back:**
- Diff touches unrelated files → ask agent to split
- Agent invents a pattern not in `tech-stack.md` → flag and ask for justification
- Agent skips a test → make them add it before proceeding

---

## Gate 6: Validation Review (before merge)

Before opening/merging the PR.

- [ ] All items in `validation.md` checked
- [ ] Every acceptance criterion has been verified
- [ ] All automated quality gates pass (lint, format, typecheck, tests, coverage)
- [ ] Manual smoke test performed
- [ ] Observability check passed (traces, logs, metrics)
- [ ] No new CVEs in dependencies
- [ ] Documentation updated (spec/plan/tasks reflect reality)
- [ ] `LessonsLearned.md` updated if any surprises occurred
- [ ] Rollback plan documented
- [ ] Roadmap will be updated to mark feature done post-merge

---

## The universal "should I keep going?" check

Ask yourself at every gate:

> *"If I stopped here and handed this to a teammate cold, would they understand what to do next?"*

If no — clarify before proceeding. Every minute spent on clarity at a gate saves 10 minutes downstream.

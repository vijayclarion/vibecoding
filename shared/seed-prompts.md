# Seed Prompts

Copy-paste prompts for every stage of Spec-Driven Development. Fill in the `<PLACEHOLDERS>` and drop into your AI agent.

---

## 1. Constitution Kickoff (NEW project)

```
Act as a senior <STACK> architect. Help me write the Constitution for a new project.

Project description:
<2-4 sentences. What the product does and for whom.>

Stakeholder inputs:
- Team: <e.g., "5 devs, comfortable with C# + TypeScript">
- Infra: <e.g., "Azure tenant, prefer managed PG">
- NFRs: <e.g., "P95 < 200ms, 99.9% availability, 1000 RPS target">
- Must integrate with: <existing systems>
- Constraints: <deadlines, compliance, budget>

Please:
1. Ask me clarifying questions ONE AT A TIME about:
   - target audience & success metrics
   - architectural trade-offs (speed vs fidelity, cost vs features)
   - tech stack preferences & constraints
   - feature priorities for an MVP
2. Organize the roadmap into 3-4 phases, 2-4 features each,
   with explicit exit criteria per phase.
3. When our conversation is done, create these files in /specs/:
   - mission.md   (what & why — NO tech details)
   - tech-stack.md (how — architecture, libs, NFRs)
   - roadmap.md   (phased feature plan)
4. Flag assumptions with [ASSUMPTION] so I can verify them.
5. Use the templates at /specs/mission.md, /specs/tech-stack.md,
   /specs/roadmap.md as starting points if they exist.
```

---

## 2. Constitution Kickoff (EXISTING/LEGACY project)

```
I'm inheriting this existing codebase and want to bring it under
Spec-Driven Development by REVERSE-ENGINEERING its Constitution.

Inputs available:
- The full source tree (you have read access to this repo)
- README.md (high-level overview)
- TODO.md (open work I'm expected to tackle)
- <any other artifacts: ADRs, old PRDs, wiki dumps, issue tracker export>

Please:
1. Explore the codebase systematically:
   - Read build files / dependency manifests for framework versions
   - Map the project / solution structure
   - Identify main entry points and deployment configs
   - Scan core directories (Controllers/Services/Repositories or
     routers/services/models) to infer architecture
   - Note test frameworks and typical patterns
   - Skim git log for recent hot spots
2. Read README.md, TODO.md, and any /docs files.
3. Draft these files in /specs/ using the templates if present:
   - mission.md    (inferred product intent)
   - tech-stack.md (what IS, not what SHOULD BE)
   - roadmap.md    (group TODO items into logical phases)
4. Flag EVERY inference with [REVERSE-ENGINEERED — please confirm].
5. Ask me:
   - Who are the actual users?
   - What are the real pain points the TODO is trying to fix?
   - Are there non-functional requirements not visible in code?
   - Priorities among the open TODO items?
```

---

## 3. New Feature — Spec Phase

Use this to start a new feature after the Constitution exists.

```
We're implementing feature F<N.N> from /specs/roadmap.md:
"<paste the roadmap bullet>"

Please:
1. Read /specs/mission.md, /specs/tech-stack.md, /specs/roadmap.md first.
2. Create a feature branch: feature/<NNN>-<short-name>
3. Create the folder /specs/features/<NNN>-<short-name>/
4. Draft spec.md using the template. Include:
   - User story (As a X, I want to Y, so that Z)
   - Acceptance criteria in Given/When/Then form (3-6 items)
   - Functional + non-functional requirements
   - Out-of-scope list
   - Dependencies
   - Validation scorecard
5. Ask me about anything ambiguous. Use [NEEDS CLARIFICATION] markers.
6. Stop after spec.md is drafted — I'll review before we plan.
```

---

## 4. Feature — Plan Phase

After `spec.md` is approved.

```
spec.md is approved. Now draft plan.md in the same folder.

Use the plan.md template. Cover:
- Approach (1-2 paragraphs, why this over alternatives)
- Files to create
- Files to modify
- Data model changes (with migration command if relevant)
- API contract (request/response shapes, status codes)
- Risks & mitigations (at least 3)
- Research notes if external docs consulted
- Alternatives considered & rejected

Stop after plan.md — I'll review before tasks.
```

---

## 5. Feature — Tasks Phase

After `plan.md` is approved.

```
plan.md is approved. Now draft tasks.md using the template.

Requirements:
- Atomic tasks (each ≤30-45 min of work)
- Ordered by dependency
- Mark with [P] where tasks are parallelizable (different files, no dependency)
- Include test tasks BEFORE implementation tasks where TDD makes sense
- Include quality gate tasks at the end
- Last tasks: commit, PR, merge, update roadmap

Stop after tasks.md — I'll review before you implement anything.
```

---

## 6. Feature — Implementation Phase

After `tasks.md` is approved.

```
tasks.md is approved. Implement the tasks in order.

Rules:
- One task at a time. Stop after each and show me the diff.
- I'll approve or ask for changes before you proceed to the next task.
- If a task reveals something unexpected, STOP and propose a spec/plan update
  before continuing.
- Update the task checkbox in tasks.md as you complete each one.
- Run the relevant test after any code change; don't proceed if tests fail.
- At the end, run all quality gates and show me the output.
```

---

## 7. Feature — Validation Phase

After implementation is done.

```
Implementation is complete. Now:
1. Walk through validation.md and check each item.
2. Run each acceptance criterion as a real test — show the output.
3. Run the manual smoke test commands from validation.md.
4. Report: which ACs passed, which failed, and any gaps from the plan.
5. Stop before committing — I'll do a final review.
```

---

## 8. Replan (after a feature ships)

```
Feature F<N.N> is merged. Please:
1. Update /specs/roadmap.md — mark F<N.N> as [x] done.
2. Note any lessons learned in /LessonsLearned.md (use the template format).
3. If any patterns emerged that should become project-wide rules,
   propose additions to /specs/tech-stack.md and flag them for my review.
4. Look at the next feature on the roadmap and tell me whether
   anything we learned in F<N.N> should change its approach.
```

---

## 9. Debug Prompt (describe the symptom, not the fix)

```
Bug: <describe behavior, not the fix>

Expected: <what should happen>
Observed: <what actually happens>
Reproduction: <minimal steps>

Environment: <relevant versions / configs>

Error output:
<paste stack trace or logs>

Recent changes: <commit hash or branch>

Please:
1. Read the relevant specs first (/specs/ and the current feature folder).
2. Form 2-3 hypotheses ranked by likelihood.
3. Propose the smallest diagnostic step to narrow down the cause.
4. Don't fix anything yet — tell me the hypothesis first.
```

---

## 10. Refactor Prompt (boundary-respecting)

```
Refactor <TARGET> with these constraints:
- No behavior change (all tests must still pass)
- No public API change
- Stay within <boundary — e.g., "the app/services directory">
- Explain the refactor in 1 sentence before writing code
- Show the full diff; I'll approve before you commit

Respect the conventions in CLAUDE.md and /specs/tech-stack.md.
```

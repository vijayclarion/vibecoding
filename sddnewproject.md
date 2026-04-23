## Step-by-Step: Building Your First SDD Project


### Step 1: Create the skeleton**

```bash
mkdir my-sdd-project && cd my-sdd-project
git init
mkdir -p specs/features
mkdir -p .github
mkdir -p src tests
touch .github/copilot-instructions.md
touch specs/{mission,tech-stack,roadmap}.md
```

### Step 2: Kickoff prompt to Copilot**

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

## Step 3: Answer Copilot's questions**

Expect ~8-12 questions about:
- Target audience
- Success metrics
- Tech preferences (Node vs Python? Postgres vs MongoDB?)
- Architecture style (monolith? microservices?)
- Testing approach
- Roadmap granularity

**Answer every question thoughtfully.** Say "deferred" if unsure.

## Step 4: Copilot drafts the Constitution**

Copy Copilot's output into `/specs/mission.md`, `/specs/tech-stack.md`, `/specs/roadmap.md`.


## Step 5: Set Copilot instructions**

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

## Step 6: Commit**

```bash
git add specs/ .github/
git commit -m "chore: establish project constitution"
```

---

## Step 7: Pick the smallest feature from Phase 1**

Let's say it's "Create order endpoint" (F1.1).

## Step 8: Ask Copilot to help write spec.md**

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

## Step 8: Review & refine spec.md**

Copilot drafts. You review. If something's wrong:
```
The spec says "validate with Zod" but I didn't mention that.
Actually, we use [ValidationLibrary]. Please update.
```

## Step 9: Write plan.md**

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

## Step 10: Write tasks.md**

```
Now draft /specs/features/001-create-order/tasks.md with:
- Atomic tasks (each ≤30 minutes of work)
- Ordered by dependency
- Mark [P] for parallelizable tasks
- Include test tasks before implementation
- End with quality gates + merge tasks

Don't start implementing yet — I'll review this first.
```

## Step 11: Write validation.md**

```
Now draft /specs/features/001-project-scaffold/validation.md with:

Prepare the validation task for each tasks in /specs/features/001-project-scaffold/tasks.md
Generate the file
```

## Step 12: Commit all three files**

```bash
git add specs/features/001-create-order/
git commit -m "feat: F1.1 create order — spec, plan, tasks"
```

---

## Step 13: Implementation (Per-task generation)

**For each task in tasks.md:**

**Step 1: Open the relevant file**

If T3 is "create order.ts model", open the (empty or existing) file in your editor.

## Step 14: Ask Copilot to implement the task**

```
Implement task T1 from /specs/features/001-create-order/tasks.md:

Read:
- /specs/features/001-create-order/plan.md (the contract)
- /specs/tech-stack.md (conventions)
- /specs/.../[other completed tasks] if relevant

Strictly implement on T1

Generate the file. Stop before testing — I'll run it.
```


## Step 15: Ask Copilot to implement the task**

```
Implement task T1 from /specs/features/001-create-order/tasks.md:

Read:
- /specs/features/001-create-order/plan.md (the contract)
- /specs/tech-stack.md (conventions)
- /specs/.../[other completed tasks] if relevant

Strictly implement only T1

Generate the file. Stop before testing — I'll run it.
```

## Step 16: Ask Copilot to validate the task**

```
Validate task T1 from /specs/features/001-create-order/validation.md:

Read:
- /specs/features/001-create-order/plan.md (the contract)
- /specs/tech-stack.md (conventions)
- /specs/.../[other completed tasks] if relevant

Strictly validate only T1

```

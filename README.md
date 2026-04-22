# 🏛️ SDD Starter Kit

Ready-to-use **Spec-Driven Development** templates for .NET and Python projects. Drop these into any new or existing project and start vibe coding with discipline.

## What's inside

```
sdd-starter-kit/
├── dotnet-template/          ← Complete .NET SDD starter
│   ├── CLAUDE.md             ← Agent entry point (reads first)
│   ├── AGENTS.md             ← Generic agent entry (Copilot/Cursor/Gemini)
│   └── specs/
│       ├── mission.md        ← What & why
│       ├── tech-stack.md     ← How (architecture, libraries, NFRs)
│       ├── roadmap.md        ← Phased feature plan
│       └── features/
│           └── 001-example-feature/
│               ├── spec.md
│               ├── plan.md
│               ├── tasks.md
│               └── validation.md
├── python-template/          ← Complete Python SDD starter (same structure)
└── shared/
    ├── LessonsLearned.md     ← Grows over time, language-agnostic
    ├── seed-prompts.md       ← Copy-paste prompts for constitution & features
    └── review-checklists.md  ← Gate-by-gate review checklists
```

## How to use

### For a NEW project
1. Copy the appropriate template folder (`dotnet-template/` or `python-template/`) into your empty repo
2. Rename it (or move its contents to your repo root)
3. Fill in the `<PLACEHOLDERS>` in `mission.md`, `tech-stack.md`, `roadmap.md`
4. Commit: `git commit -m "chore: establish project constitution"`
5. Ask your AI agent: *"Read /specs/ and help me implement F1.1 from roadmap"*

### For an EXISTING project
1. Copy just the `specs/` folder structure (keep the empty template files)
2. Open a fresh AI session at repo root
3. Paste the **reverse-engineering prompt** from `shared/seed-prompts.md`
4. Let the agent draft your Constitution from the existing code
5. Review, correct, commit

## Templates follow these principles

- **`mission.md` has NO tech details** — it's for humans and stakeholders
- **`tech-stack.md` locks in the HOW** — versions, patterns, NFRs
- **`roadmap.md` is phased** — each feature ships in 1-3 days
- **Every feature has 4 artifacts** — spec, plan, tasks, validation
- **All docs are version-controlled** — specs live with the code, forever

## Attribution

Inspired by the DeepLearning.AI × JetBrains course *"Spec-Driven Development with Coding Agents"* and GitHub's [Spec Kit](https://github.com/github/spec-kit).

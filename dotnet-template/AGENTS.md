# AGENTS.md

This file is the generic agent entry point (Cursor, GitHub Copilot, Gemini CLI, Windsurf, etc.). For Claude Code, see `CLAUDE.md`. Contents are equivalent — both files point to the same spec artifacts.

---

See `CLAUDE.md` for the full conventions. Key points:

- Project: **<PROJECT_NAME>** (.NET 8)
- Source of truth: `/specs/mission.md`, `/specs/tech-stack.md`, `/specs/roadmap.md`
- Per-feature artifacts live in `/specs/features/<feature-folder>/`
- Always read specs before making changes
- Follow the Definition of Done before marking any task complete
- Edit spec/plan/tasks files via conversation — never let the human edit them manually

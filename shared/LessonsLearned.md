# Lessons Learned

> A living log of surprises, gotchas, and hard-won wisdom from this project. The agent should propose additions here after every bug, surprise, or "huh, didn't expect that" moment.

## How to use this file

- **Append only.** Don't rewrite history.
- **Date every entry.** `YYYY-MM-DD - <Feature ref> - <Title>`
- **Be specific.** "Don't use X" is weak. "Using X with Y causes Z because..." is useful.
- **Link to evidence.** Commit hash, test name, external doc.
- **When a lesson is generalized into a spec rule, cross-link both directions.**
- **Promote patterns to `tech-stack.md`** once they're clearly universal for this project.

## Template

```markdown
### YYYY-MM-DD — F1.2 — Short title

**Context:** <what we were trying to do>

**What happened:** <what actually happened>

**Root cause:** <the underlying reason>

**Fix:** <what we did>

**Rule going forward:**
- <concrete rule>
- <promoted to: specs/tech-stack.md § Cross-Cutting Concerns>

**References:** <commit / test / link>
```

---

## Entries

### YYYY-MM-DD — Example — Replace this with your first real lesson

**Context:** Adding idempotency to `POST /orders` without thinking through concurrent requests.

**What happened:** Two near-simultaneous requests with the same `Idempotency-Key` both created orders because our check-then-insert wasn't atomic.

**Root cause:** Race condition between our "check if key exists" query and the insert.

**Fix:** Use a unique index on `idempotency_key` and catch the uniqueness violation; the second request returns the first response.

**Rule going forward:**
- Idempotency: always enforce with a unique DB constraint, never application-level check.
- Promoted to: `specs/tech-stack.md` § Cross-Cutting Concerns — "Idempotency".

**References:** commit `abc1234`, test `test_concurrent_idempotent_requests`

---

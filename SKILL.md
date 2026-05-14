---
name: commit-suggester
description: Suggests Conventional Commits v1.0.0 messages at the end of meaningful coding tasks. Trigger proactively after any real code change — new feature, bug fix, refactor, tests, docs, build/CI update, or dependency change — without waiting for the user to ask. Skip for pure Q&A, planning without edits, or when the user has already committed this turn. Outputs ready-to-paste `git commit -m` commands with auto-detected type/scope; suggests multiple commits when changes span unrelated areas.
---

# Commit Suggester

Suggest Conventional Commits v1.0.0 messages at the end of every meaningful coding task.

## When to trigger

**Do trigger** (proactively, without being asked) after:
- New feature, component, endpoint, script, or CLI command
- Bug fix, refactor/rename, performance improvement
- Tests added or updated
- Docs, README, code comments, changelogs updated
- Build config, CI/CD, dependencies, or lockfiles changed
- Formatting or linting passes

**Skip** when:
- No files were edited this turn
- Pure Q&A or planning without resulting edits
- The user already committed mid-conversation
- The user said they'll handle commits themselves

When uncertain, suggest — it's cheap and ignorable.

## Output format

Always English (community standard), regardless of conversation language. Place at the very end of the response after a `---` separator:

```
---

### 📝 Suggested commit

```bash
git commit -m "<type>(<scope>): <description>"
```

Chosen `feat(scope)` because <one sentence reason>.
```

For body or footer, chain `-m` flags (each becomes a git paragraph):
```bash
git commit -m "<subject>" -m "<body>" -m "<footer>"
```

## Picking the right type

Priority order: user's stated intent → files changed → code diff → breaking change signal.

| Signal | Type |
|--------|------|
| "add a feature / new X" | `feat` |
| "fix the bug / wrong output" | `fix` |
| "refactor / clean up / rename" | `refactor` |
| "write tests" | `test` |
| Only `*.md` / `docs/` changed | `docs` |
| Only `package.json`, lockfile | `build` |
| Only `.github/workflows/` | `ci` |
| Public API removed or renamed | add `!`, `BREAKING CHANGE` footer |

See [REFERENCE.md](REFERENCE.md) for the full type table, scope rules, subject-line rules, and body/footer guidance.

## Multiple commits

Suggest separate commits when changes span unrelated types. Order: `refactor` → `feat/fix` → `test` → `docs` → `chore`. Include `git add <files>` lines so the user can stage selectively. Don't split tightly coupled changes (feature + its tests + its docs = one commit).

See [EXAMPLES.md](EXAMPLES.md) for ready-to-copy examples covering features, bug fixes, breaking changes, multiple commits, and edge cases.

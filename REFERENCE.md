# Commit Suggester — Reference

## Full type table

| Type | When to use |
|------|-------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only (README, JSDoc, comments) |
| `style` | Formatting, whitespace — no logic change |
| `refactor` | Code restructured; no new behavior, no bug fixed |
| `perf` | Performance improvement |
| `test` | Tests added or fixed |
| `build` | Build system, bundler, external dependencies |
| `ci` | CI/CD config (GitHub Actions, GitLab CI, etc.) |
| `chore` | Maintenance that fits nowhere else |
| `revert` | Reverts a previous commit |

When the change fits two types, pick the one matching the user's *intent*. A feature shipped with its own tests and docs is one `feat` commit, not three.

## Scope rules

Scope is a short noun in parentheses: `feat(auth):`, `fix(api):`, `docs(readme):`.

Good candidates: module/package name (`auth`, `parser`), feature area (`checkout`), single file in tiny repos (`config`).

Skip scope when the change is cross-cutting, or when scope would just repeat the type (`chore(chore):` — never).

## Subject-line rules

- Imperative mood: "add", "fix", "remove" — not "added", "fixes"
- Lowercase first letter (except proper nouns / acronyms)
- No trailing period
- Keep full subject line under ~72 characters
- Describe *what* and *why*, not *how*

## When to add body / footer

**Body** — add when:
- The "why" isn't obvious from the subject
- Reviewer needs context (tradeoffs, linked issues)
- Change touches multiple things worth listing

**Footer** — add for:
- Breaking changes: `BREAKING CHANGE: <description>` + `!` before colon in subject → `feat(api)!: rename endpoint`
- Issue refs: `Refs: #123` or `Closes: #456`
- Co-authors: `Co-authored-by: Name <email>`

Each `-m` flag in the CLI command becomes a git paragraph (blank-line separated).

## Auto-detection signal priority

1. **User's stated intent** — "fix the bug" → `fix`; "add X feature" → `feat`
2. **Files changed** — `*.md` / `docs/` only → `docs`; `*.test.*` only → `test`; `package.json` / lockfile only → `build`; `.github/workflows/` → `ci`; `.prettierrc` / `.eslintrc` → `chore` or `style`
3. **Code diff** — new exported function/route → `feat`; null-check or off-by-one fix → `fix`; same behavior, different structure → `refactor`
4. **Breaking change** — public API signature, CLI flag, env var, or exported type removed or changed incompatibly → `!` + `BREAKING CHANGE` footer

When signals conflict, prefer the user's stated goal.

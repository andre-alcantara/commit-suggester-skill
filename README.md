# Commit Suggester Skill

A Claude skill that suggests [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) messages at the end of every meaningful coding task, automatically, without you having to ask.

Built for people who don't want to context switch out of coding flow just to think up a good commit subject line.

## What it does

Whenever Claude finishes a real piece of coding work (a feature, a bug fix, a refactor, a test, a docs update) it appends a ready to paste `git commit -m` command at the end of the response, with the type, scope, and description already picked for you.

It covers:

* **The 11 Conventional Commit types**: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
* **Auto detected scope**: module, package, or feature area inferred from the files that changed
* **Imperative mood subjects**: "add", "fix", "remove" (not "added", "fixes")
* **Body and footers when needed**: issue refs (`Refs: #412`), breaking changes (`BREAKING CHANGE:`), co authors
* **Multiple commits when appropriate**: if you mixed an unrelated docs fix into a feature change, it suggests splitting them
* **English commit messages regardless of conversation language**: your chat can be in any language; the commit itself follows the community standard

The output is a single code block you can copy straight into your terminal.

## What's in the box

```
commit-suggester-skill/
├── SKILL.md    The full skill (rules, type table, examples, edge cases)
└── README.md   You're reading it
```

No bundled resources, no scripts. It's a single self contained skill file.

## How to install

Drop the `SKILL.md` into your Claude skills directory inside a folder named `commit-suggester`:

```
/mnt/skills/user/commit-suggester/SKILL.md
```

Claude will pick it up automatically on the next conversation.

## How to use

You don't. That's the point. It triggers itself proactively whenever Claude finishes meaningful code changes. Examples of what counts as "meaningful":

* Implementing a feature, endpoint, component, or CLI command
* Fixing a bug
* Refactoring or renaming
* Writing or updating tests
* Changing build config, CI/CD, dependencies, lockfiles
* Updating documentation
* Formatting, linting, cosmetic changes
* Reverting a previous change

It will **not** trigger for pure Q&A, planning discussions with no edits, or one line suggestions you clearly haven't adopted yet.

## What the output looks like

At the end of a normal Claude response, you'll see:

````
### 📝 Suggested commit

```bash
git commit -m "feat(settings): add dark mode toggle"
```

Adds a new user facing capability in the settings module, so `feat` with `settings` scope.
````

For commits that need a body or footer, it uses the multi `-m` form so it stays copy pasteable:

```bash
git commit -m "feat(api)!: rename getUser endpoint to fetchUser" -m "BREAKING CHANGE: clients calling getUser must update to fetchUser. The old route has been removed."
```

For changes spanning unrelated areas, it suggests multiple commits with the `git add` commands to stage each one separately:

```bash
# Commit 1: invoice export feature
git add src/invoices/ tests/invoices/
git commit -m "feat(invoices): add PDF export"

# Commit 2: unrelated docs update
git add README.md
git commit -m "docs(readme): update setup steps for Node 20"
```

## How the type gets picked

The skill uses signals in this order of strength:

1. **What you asked for**: "fix the bug where…" → `fix`; "add a feature to…" → `feat`
2. **What files changed**: `*.md` only → `docs`; `tests/` only → `test`; `.github/workflows/` → `ci`; etc.
3. **What changed in the code**: new exported function → likely `feat`; null check fix → likely `fix`; same behavior, different structure → `refactor`
4. **Breaking changes**: any incompatible change to a public API, CLI flag, or env var gets `!` in the subject and a `BREAKING CHANGE:` footer

When signals disagree, the stated goal wins over file based heuristics.

## Tips for best results

* **Let it run on the first turn after a change**. It works best when the change is fresh in the conversation context.
* **Override it freely**. If the suggested type or scope feels wrong, just tell Claude to redo it. The skill is a starting point, not a final answer.
* **Combine with your own staging**. For big turns, use the suggested `git add` paths as a hint for what to stage separately.

## Who made this

By [André Alcantara](https://andrealcantara.cc)

If you ship code regularly and want consistent commit history without thinking about it, this will quietly do that for you.

# Commit Suggester — Examples

## Example 1 — simple feature

User: "Add a dark mode toggle to the settings page."
Files changed: `src/settings/ThemeToggle.tsx`, `src/settings/index.tsx`

---

### 📝 Suggested commit

```bash
git commit -m "feat(settings): add dark mode toggle"
```

New user-facing capability in the settings module → `feat(settings)`.

---

## Example 2 — bug fix with issue ref

User: "The cart total is wrong when a coupon stacks with a member discount — fix it (issue #412)."

---

### 📝 Suggested commit

```bash
git commit -m "fix(cart): correct total when coupon stacks with member discount" -m "Refs: #412"
```

Bug fix in the cart module; issue number goes in a footer.

---

## Example 3 — breaking change

User: "Rename the `getUser` API endpoint to `fetchUser` everywhere."

---

### 📝 Suggested commit

```bash
git commit -m "feat(api)!: rename getUser endpoint to fetchUser" -m "BREAKING CHANGE: clients calling getUser must update to fetchUser. The old route has been removed."
```

Public API renamed → `!` in subject + `BREAKING CHANGE` footer.

---

## Example 4 — multiple commits

User: "Implement the new invoice export feature. Also the README setup steps are outdated — fix those."

---

### 📝 Suggested commits

```bash
# Commit 1 — invoice export (feature + its tests)
git add src/invoices/ tests/invoices/
git commit -m "feat(invoices): add PDF export"

# Commit 2 — unrelated docs fix
git add README.md
git commit -m "docs(readme): update setup steps for Node 20"
```

Two unrelated concerns → two commits.

---

## Example 5 — refactor

User: "Clean up `UserService` — the methods are too long."

---

### 📝 Suggested commit

```bash
git commit -m "refactor(user): extract helper methods from UserService"
```

Behavior unchanged, structure reorganized → `refactor`.

---

## Edge cases

| Situation | Action |
|-----------|--------|
| Can't tell what changed | Suggest based on conversation; note "Adjust if your staged diff differs." |
| User says work is unfinished | Use `chore(wip):` or skip with "Looks like a WIP — I'll hold off." |
| Very large cross-cutting change | Suggest splitting; don't cram into one subject line |
| User already committed this turn | Don't re-suggest; only suggest for new changes made after that commit |
| Mix of generated + existing code | Describe the net user-visible change, not the file count |

---
name: production-clean-python
description: Removes print/debug statements, unused imports, unused variables, and unnecessary comments to prepare Python/Django projects for production.
---

# Production Clean — Python / Django

Prepare your Python/Django project for production. Detect, clean, and report all unnecessary code pollution.

## Stack Detection

This skill is for Python and Django projects. Verify `manage.py` or `pyproject.toml` exists before proceeding. If the project contains only `package.json`, use `production-clean-js` skill instead.

---

## STEP 1 — Scan (Do Not Touch Anything Yet)

Scan the entire project. Do not modify anything. Collect all findings into a list.

**print / debug statements to detect:**
```python
print(
pprint(
pp(          ← pretty-print shortcut
ic(          ← icecream debug library
breakpoint(
import pdb
pdb.set_trace()
import ipdb
ipdb.set_trace()
```

**Unused imports to detect:**
- Any module that is imported but never referenced in the file
- `from x import y` where `y` is never used
- `import os` present but no `os.` usage anywhere in the file
- Star imports that shadow names: `from module import *`

**Unused variables to detect:**
- Variables that are assigned but never read afterward
- Function parameters not prefixed with `_` but never used inside the function
- Loop variables that are defined but never used inside the loop body

**Unnecessary comments to detect:**
- Lines containing `# TODO`, `# FIXME`, `# HACK`, `# TEMP`, `# test`, `# debug`
- Commented-out code blocks
- Temporary `# noqa` suppressions
- Meaningless inline comments: `# increment i`, `# return result`

---

## STEP 2 — Ask for Confirmation

Display this summary before touching anything:

```
📋 SCAN RESULTS — Python / Django

┌─────────────────────────────────────┬────────┐
│ Category                            │ Count  │
├─────────────────────────────────────┼────────┤
│ 🔴 print / debug statements        │  XX    │
│ 🟡 Unused imports                  │  XX    │
│ 🟡 Unused variables                │  XX    │
│ 🔵 Unnecessary comments            │  XX    │
├─────────────────────────────────────┼────────┤
│ TOTAL                               │  XX    │
└─────────────────────────────────────┴────────┘

Affected files: XX

Would you like to proceed? (Yes / No / Select by category)
```

- **No** → Stop. Do not modify anything.
- **Select by category** → Ask approval per category.

---

## STEP 3 — Cleanup

Once approved, clean in this exact order:

1. **print / debug statements**
2. **Unused imports**
3. **Unused variables** — re-check after import cleanup
4. **Unnecessary comments**

### Never Touch:
- `settings.py` — never touch under any circumstances
- `migrations/` — never touch anything inside this folder
- `# type: ignore` comments — may be intentional, ask first
- `# noqa` suppressions — may be intentional, ask first
- Docstrings (`"""..."""`) — documentation, never remove

### Django Specific — Extra Caution:
- **`__init__.py`** — imports here may be part of the public API, ask before removing
- **`urls.py`** — imports may be used via lazy URL loading, verify carefully before removing
- **`admin.py`** — `@admin.register(Model)` decorators must never be removed
- **`signals.py` / `apps.py`** — `@receiver` signal handlers may look unused but are active at runtime, never remove
- **`apps.py` → `ready()`** — signal imports here are intentional, never remove

### Before Modifying Each File:
1. Read the current file
2. Apply the change
3. Verify syntax is still valid — if broken, revert immediately

---

## STEP 4 — Final Report

Save as `production-clean-report.md`:

```markdown
# Production Clean Report — Python / Django
**Date:** [date]
**Project:** [project name]

## Summary

| Category              | Detected | Removed | Skipped |
|-----------------------|----------|---------|---------|
| print / debug         | XX       | XX      | XX      |
| Unused imports        | XX       | XX      | XX      |
| Unused variables      | XX       | XX      | XX      |
| Unnecessary comments  | XX       | XX      | XX      |
| **TOTAL**             | **XX**   | **XX**  | **XX**  |

## Affected Files

| File | Changes | Categories |
|------|---------|------------|
| app/views.py       | 3 | print, unused import   |
| app/serializers.py | 1 | unused variable        |

## Skipped / Requires Manual Review

- `app/settings.py` — not touched (by rule)
- `app/migrations/` — not touched (by rule)
- `app/signals.py:14` — @receiver handler, kept intentionally

## Notes
- Total files scanned: XX
- Files modified: XX
- Estimated lines removed: ~XX
```

---

## Error Handling

If a syntax error occurs after modifying a file:
1. Revert that file to its original state
2. Note the error in the report
3. Continue with remaining files
4. Add it to the "requires manual review" list
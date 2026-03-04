---
name: production-clean-js
description: Removes console.log statements, unused imports, unused variables, and unnecessary comments to prepare JavaScript/TypeScript (React, Next.js) projects for production.
---

# Production Clean — JavaScript / TypeScript

Prepare your JS/TS project for production. Detect, clean, and report all unnecessary code pollution.

## Stack Detection

This skill is for JavaScript and TypeScript projects. Verify `package.json` exists before proceeding. If the project contains `manage.py` or `pyproject.toml` instead, use `production-clean-python` skill.

---

## STEP 1 — Scan (Do Not Touch Anything Yet)

Scan the entire project. Do not modify anything. Collect all findings into a list.

**console.log / debug statements to detect:**
```
console.log(
console.debug(
console.info(
debugger;
```

> Do NOT flag `console.error()` or `console.warn()` — these may be intentional production logs.

**Unused imports to detect:**
- Any `import` statement that is never referenced in the file
- `import React from 'react'` — unnecessary for Next.js 13+ and React 17+
- Type-only imports that should use `import type` but don't

**Unused variables to detect:**
- Any `const`, `let`, or `var` that is defined but never read
- Destructured but unused values: `const { a, unusedB } = obj`
- Function parameters that are received but never used

**Unnecessary comments to detect:**
- Lines containing `// TODO`, `// FIXME`, `// HACK`, `// TEMP`, `// test`, `// debug`
- Commented-out code blocks
- Temporary disable comments: `/* eslint-disable */`, `// eslint-disable-next-line` (ask before removing)
- Meaningless inline comments: `// increment i`, `// return result`

---

## STEP 2 — Ask for Confirmation

Display this summary before touching anything:

```
📋 SCAN RESULTS — JavaScript / TypeScript

┌─────────────────────────────────────┬────────┐
│ Category                            │ Count  │
├─────────────────────────────────────┼────────┤
│ 🔴 console.log / debugger           │  XX    │
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

1. **console.log / debugger statements**
2. **Unused imports**
3. **Unused variables** — re-check after import cleanup
4. **Unnecessary comments**

### Never Touch:
- `console.error()` / `console.warn()`
- `// eslint-disable-next-line` — ask first
- JSDoc / TSDoc (`/** */`) comments — documentation
- `index.ts` / `index.js` re-exports — may be public API
- `.d.ts` files
- Config files: `next.config.js`, `vite.config.ts`, `tailwind.config.ts`, etc.
- Test files: `*.test.ts`, `*.spec.ts`, `*.test.tsx` — ask before touching

### Next.js Specific:
- Do not touch `'use client'` / `'use server'` directives
- Be extra careful with: `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`

### TypeScript Specific:
- Before removing a type import, verify it is not used at runtime
- Do not remove generic type parameters just because they appear unused

### Before Modifying Each File:
1. Read the current file
2. Apply the change
3. Verify syntax is still valid — if broken, revert immediately

---

## STEP 4 — Final Report

Save as `production-clean-report.md`:

```markdown
# Production Clean Report — JavaScript / TypeScript
**Date:** [date]
**Project:** [project name]

## Summary

| Category              | Detected | Removed | Skipped |
|-----------------------|----------|---------|---------|
| console.log / debugger | XX      | XX      | XX      |
| Unused imports        | XX       | XX      | XX      |
| Unused variables      | XX       | XX      | XX      |
| Unnecessary comments  | XX       | XX      | XX      |
| **TOTAL**             | **XX**   | **XX**  | **XX**  |

## Affected Files

| File | Changes | Categories |
|------|---------|------------|
| src/components/Button.tsx | 3 | console.log, unused import |
| src/utils/helpers.ts      | 1 | unused variable            |

## Skipped / Requires Manual Review

- `src/utils/logger.ts:45` — console.error() may be intentional
- `src/components/Form.tsx:12` — eslint-disable, needs manual check

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
# claude-clean

> Claude Code skills for production-ready code cleanup.

## Installation

```bash
npx skills add Reyretee/claude-clean --skill production-clean
```

## Available Skills

| Skill | Description | Install |
|---|---|---|
| `production-clean` | Removes debug statements, unused imports, variables and comments. Supports JS/TS and Python/Django. | `npx skills add Reyretee/claude-clean --skill production-clean` |

---

## Usage

After installation, open Claude Code in your project and run:

```
/production-clean
```

Claude will:
1. **Scan** your project for code pollution
2. **Show a summary** and ask for confirmation before touching anything
3. **Clean** only what you approve
4. **Generate** a `production-clean-report.md`

---

## What Gets Cleaned

**JavaScript / TypeScript (React, Next.js):**
- `console.log`, `console.debug`, `debugger` statements
- Unused imports
- Unused variables and destructured values
- Commented-out code and meaningless comments

**Python / Django:**
- `print()`, `pprint()`, `breakpoint()`, `pdb.set_trace()`
- Unused imports
- Unused variables
- Commented-out code blocks

## What Never Gets Touched

- `settings.py` and `migrations/` in Django projects
- `index.ts` / `index.js` re-exports (public API)
- JSDoc / TSDoc documentation comments
- Next.js config and special files (`layout.tsx`, `page.tsx`)
- Django signal handlers and `@admin.register` decorators
- Test files (asked before touching)

---

## License

MIT
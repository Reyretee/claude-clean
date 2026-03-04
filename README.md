# claude-clean

> Claude Code skills for production-ready code cleanup.

## Available Skills

| Skill | Description |
|---|---|
| `production-clean-js` | Cleans JS/TS (React, Next.js) projects |
| `production-clean-python` | Cleans Python/Django projects |

> There is no combined `production-clean` skill. Install the one that matches your stack, or install both.

---

## Installation

**JavaScript / TypeScript (React, Next.js):**
```bash
npx skills add Reyretee/claude-clean --skill production-clean-js
```

**Python / Django:**
```bash
npx skills add Reyretee/claude-clean --skill production-clean-python
```

**Both:**
```bash
npx skills add Reyretee/claude-clean --skill production-clean-js
npx skills add Reyretee/claude-clean --skill production-clean-python
```

---

## Usage

After installation, open Claude Code in your project and run:

```
/production-clean-js
```
or
```
/production-clean-python
```

Claude will:
1. **Scan** your project for code pollution
2. **Show a summary** and ask for confirmation before touching anything
3. **Clean** only what you approve
4. **Generate** a `production-clean-report.md`

---

## What Gets Cleaned

**`production-clean-js` — JavaScript / TypeScript:**
- `console.log`, `console.debug`, `debugger` statements
- Unused imports
- Unused variables and destructured values
- Commented-out code and meaningless comments

**`production-clean-python` — Python / Django:**
- `print()`, `pprint()`, `breakpoint()`, `pdb.set_trace()`
- Unused imports
- Unused variables
- Commented-out code blocks

---

## What Never Gets Touched

**JS/TS:** `console.error/warn`, JSDoc comments, `index.ts` re-exports, `.d.ts` files, config files, Next.js special files

**Python/Django:** `settings.py`, `migrations/`, `@receiver` signal handlers, `@admin.register` decorators, `__init__.py` public imports, `urls.py` lazy imports

---
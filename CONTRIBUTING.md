# Coding Standards

Rules that apply in this repository. Goal: code that is readable, consistent, and easy for anyone on the team to maintain.

## Table of Contents

1. [Naming](#1-naming)
2. [Comments](#2-comments)
3. [Project Structure](#3-project-structure)
4. [Coding Rules](#4-coding-rules)
5. [Tooling and Automation](#5-tooling-and-automation)
6. [Git and Code Review](#6-git-and-code-review)

---

## 1. Naming

### 1.1 General Rules

- Names are written in **English**, consistently across the whole project.
- A name describes **what something is** or **what it does**, not how it works.
- Avoid cryptic abbreviations. Widely known ones are allowed: `id`, `url`, `api`, `i` in a simple loop.
- Banned catch-all names: `data`, `info`, `temp`, `stuff`, `handle`, `process`, `manager`, `helper`, `misc`.
- A name should be pronounceable and searchable in the codebase.

### 1.2 Casing Conventions

| Element | Convention |
|---------|------------|
| Variables, parameters | `camelCase` |
| Functions, methods | `camelCase` (verb) |
| Classes | `PascalCase` (noun) |
| Interfaces | `PascalCase`, **no** `I` prefix |
| Types (`type`) | `PascalCase` |
| Enums and their values | `PascalCase` |
| Generic parameters | `T` or a descriptive name prefixed with `T` |
| Constants (module level) | `UPPER_SNAKE_CASE` |
| React components | `PascalCase` |
| React hooks | `camelCase` with `use` prefix |
| Files (general) | `kebab-case` |
| React component files | `PascalCase` |
| Folders | `kebab-case` |

### 1.3 Detailed Rules

- **Boolean variables** start with `is`, `has`, `can`, or `should`.
- **Functions** are verbs describing an action.
- **Collections** are named in the plural, without `List`/`Array` suffixes.
- **Constants** replace magic numbers and magic strings.
- **Prefixes and suffixes** such as `I` for interfaces or `Type`/`Enum` in names are unnecessary.
- **Event handlers:** `handleXxx` for handler functions, `onXxx` for props.

### 1.4 Types and `any`

- **`any` is forbidden.** If a type is truly unknown, use `unknown` and narrow it.
- `strict` mode in `tsconfig.json` is mandatory.
- Public and exported functions have explicitly declared return types.
- `interface` for object shapes and contracts, `type` for unions, intersections, and aliases.
- Avoid type assertions (`as`) unless you are certain, and explain why in a comment.

---

## 2. Comments

### 2.1 When to Comment

- A non-obvious business or technical decision (why this way and not another).
- Workarounds for library or API bugs, with a link to the issue.
- Complex algorithms or regular expressions.
- Warnings about side effects or limitations.

### 2.2 When NOT to Comment

- When the comment repeats what the code already says.
- Instead of deleting dead code. **Commented-out code gets removed**; the history lives in git.
- To keep a change log inside a file (that is what `git log` is for).

### 2.3 JSDoc / TSDoc

- Use it for **public APIs, libraries, and exported functions**.
- Do not repeat types in the description, since TypeScript already knows them.
- Describe: what the function does, parameters (`@param`), result (`@returns`), thrown errors (`@throws`).

### 2.4 TODO / FIXME

- Always with an author and context (ticket number or reason), in the format `TODO(author): description, ticket #123`.
- A TODO without context does not pass code review.

---

## 3. Project Structure

### 3.1 Principles

- Organize code **by feature (feature-based)**, not by file type.
- One file = one responsibility. A file over ~300 lines is a signal to split it.
- Shared code goes into `shared/`; code specific to a feature stays in its folder.
- Layers do not mix: business logic does not live in UI components or HTTP controllers.
- Dependencies flow one way: `features` → `shared`, never the reverse. Features do not import each other without a clear need.

### 3.2 Directory Layout

```
src/
├── app/                    # app initialization, routing, providers
├── features/
│   └── <feature-name>/
│       ├── components/     # UI components for this feature
│       ├── hooks/          # hooks specific to this feature
│       ├── services/       # business logic, API calls
│       ├── types/          # types and interfaces
│       ├── utils/          # helpers used only by this feature
│       └── index.ts        # public API of the module
├── shared/
│   ├── components/         # shared UI components
│   ├── hooks/              # shared hooks
│   ├── utils/              # shared helper functions
│   ├── types/              # shared types
│   └── constants/          # shared constants
├── config/                 # configuration, environment variables
└── main.ts
```

### 3.3 Imports

- Every module exposes its public API through `index.ts`. Import from `index.ts`, not from the module's internals.
- Use path aliases (`@/features/...`) instead of long relative paths.
- Import order (enforced by lint):
  1. External libraries
  2. Alias modules (`@/...`)
  3. Relative imports (`./`, `../`)
  4. Styles and assets
- No circular dependencies between modules.
- Prefer **named exports** over `export default` (easier refactoring and searching).

### 3.4 Test Files

Tests live next to the code they test, with the `.test.ts` suffix.

---

## 4. Coding Rules

- **One function = one job.** If you describe it using the word "and", split it.
- Keep functions short (roughly up to ~30 lines) with at most 3-4 parameters (more: pass an object).
- **Early return** instead of deeply nested `if` statements.
- `const` by default, `let` only when needed, never `var`.
- Prefer immutability and pure functions.
- Always `===` instead of `==`.
- Handle errors explicitly. Do not swallow exceptions (an empty `catch` is forbidden).
- DRY, but no premature abstraction: three similar places is a signal to extract shared code, two is not yet.
- No `console.log` in production code. Use a logger.

---

## 5. Git and Code Review

### 5.1 Branches

Format: `type/short-description`, where type is one of `feature`, `fix`, `refactor`, `docs`, `chore`.

### 5.2 Commits (Conventional Commits)

- Format: `type: short description in imperative mood`.
- Allowed types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`.

### 5.3 Pull Requests

- Small and focused on a single concern (roughly up to ~400 changed lines).
- Description: **what** changed, **why**, and how to test it.
- At least **1 approval** before merge.
- CI must be green (lint, typecheck, tests).

### 5.4 Code Review Checklist

- [ ] Names are clear and follow the convention
- [ ] Functions are short and do one thing
- [ ] No `any`, magic numbers, or commented-out code
- [ ] Comments explain "why", not "what"
- [ ] Errors are handled
- [ ] New logic has tests
- [ ] Folder structure and imports follow the rules

### 5.5 Definition of Done

- [ ] Code reviewed and merged
- [ ] Documentation updated (if applicable)
- [ ] Acceptance criteria met

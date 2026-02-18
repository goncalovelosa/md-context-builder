# Path-Specific Rules Reference

For larger projects, organize instructions into multiple files using the `.claude/rules/` directory. Rules can be scoped to specific files using YAML frontmatter.

## Basic Structure

```yaml
---
paths:
  - "src/api/**/*.ts"
---
# API Development Rules

- All API endpoints must include input validation
- Use the standard error response format
- Include OpenAPI documentation comments
```

## Glob Pattern Reference

| Pattern                | Matches                                       | Example                                             |
| ---------------------- | --------------------------------------------- | --------------------------------------------------- |
| `**/*.ts`              | All TypeScript files in any directory         | Matches `src/index.ts`, `lib/utils.ts`              |
| `src/**/*`             | All files under `src/` directory              | Matches `src/index.ts`, `src/components/Button.tsx` |
| `*.md`                 | Markdown files in project root                | Matches `README.md`, `CLAUDE.md`                    |
| `src/components/*.tsx` | React components in specific directory        | Matches `src/components/Header.tsx` only            |
| `**/*.{ts,tsx}`        | Both `.ts` and `.tsx` files (brace expansion) | Matches all TypeScript files                        |
| `{src,lib}/**/*.ts`    | Files in `src/` OR `lib/` (brace expansion)   | Matches `src/index.ts`, `lib/utils.ts`              |

## Multiple Patterns

```yaml
---
paths:
  - "src/**/*.ts"
  - "lib/**/*.ts"
  - "tests/**/*.test.ts"
---
# TypeScript/React Rules
- Use strict type checking
- Prefer interfaces for public APIs
- Write tests alongside source files
```

**Rules without `paths` field** are loaded unconditionally and apply to all files.

## Example Rule Files

### TypeScript Rules

```yaml
# .claude/rules/typescript/patterns.md
---
paths:
  - "src/**/*.ts"
---
# TypeScript Development Rules

- Always use strict type checking
- Prefer interfaces for public APIs
- Use async/await over Promise chains
- Avoid `any` type - use `unknown` when type is truly unknown
- Use type guards for runtime type checking
```

### Database Rules

```yaml
# .claude/rules/database/operations.md
---
paths:
  - "src/shared/database/**"
  - "src/modules/*/infra/persistence/**"
---
# Database Operation Rules

- NEVER manually edit migration files
- Always use transactions for multi-step operations
- Use repository pattern for data access
- Prefer raw SQL over ORM for complex queries
```

### API Endpoint Rules

```yaml
# .claude/rules/api/endpoints.md
---
paths:
  - "src/modules/*/infra/http/**"
---

# API Endpoint Rules

- All endpoints must have input validation
- Use standard error response format
- Include OpenAPI documentation
- Rate limit all public endpoints
- Log all external API calls
```

## Directory Structure Example

```
.claude/rules/
├── frontend/
│   ├── react.md
│   └── styles.md
├── backend/
│   ├── api.md
│   └── database.md
└── general.md
```

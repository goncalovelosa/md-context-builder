# CLAUDE.md

<!-- CLAUDE.md Last Updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->

[One-line description - what this project does]

## Package Manager

**IMPORTANT:** Uses [yarn/npm/pnpm] (not [other package managers])

## Common Workflows

### Building & Running

1. `[install command]` - Install dependencies
2. `[dev command]` - Start development server
3. `[build command]` - Build for production
4. `[test command]` - Run tests

### [Project-Specific Workflow - e.g., Adding a Feature]

1. [Step 1 - e.g., Create component in src/components/]
2. [Step 2 - e.g., Add tests in **tests**/]
3. [Step 3 - e.g., Update exports in index.ts]

### [Another Workflow - e.g., Database Migrations]

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Key Files

- `@path/to/entry` - [purpose]
- `@path/to/config` - [purpose]
- `@path/to/key/module` - [purpose]
- `@path/to/another/key/file` - [purpose]

## Non-Obvious Knowledge

<!-- CRITICAL: Capture tribal knowledge Claude can't infer -->
<!-- Example: "Rate limiting at 100 req/min is intentional - not a bug" -->
<!-- Example: "The 'legacy' folder is intentionally deprecated - don't add new code there" -->

- [Tribal knowledge 1]
- [Tribal knowledge 2]
- [Tribal knowledge 3]

## Do Not

- [Critical rule 1 - e.g., "Use `pnpm` exclusively"]
- [Critical rule 2 - e.g., "Preserve tmp/ folder for build caching"]
- [Critical rule 3 - e.g., "Run migrations before deploying"]
- [Critical rule 4]

## Learned Lessons

<!-- Add entries when Claude makes mistakes -->
<!-- 2026-02-15: Claude deleted migration files → Added rule: "Never delete files in migrations/" -->

## Verification

- Run `[test command]` after code changes
- Run `[build command]` before marking complete

## Progressive Disclosure

- Architecture: @.claude/docs/architecture.md
- Development: @.claude/docs/development.md

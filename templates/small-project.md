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

### [Project-Specific Workflow - e.g., Adding an API Endpoint]

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Key Files

- `@path/to/entry` - [purpose]
- `@path/to/config` - [purpose]
- `@path/to/key/module` - [purpose]

## Non-Obvious Knowledge

<!-- CRITICAL: Capture tribal knowledge Claude can't infer -->
<!-- Example: "Rate limiting at 100 req/min is intentional - not a bug" -->
<!-- Example: "User IDs are immutable by design - never add mutation endpoint" -->

- [Tribal knowledge 1]
- [Tribal knowledge 2]

## Do Not

- [Critical rule 1 - e.g., "Use `pnpm` exclusively"]
- [Critical rule 2 - e.g., "Preserve tmp/ folder for build caching"]
- [Critical rule 3]

## Learned Lessons

<!-- Add entries when Claude makes mistakes -->
<!-- 2026-02-15: Claude deleted migration files → Added rule: "Never delete files in migrations/" -->

## Verification

- Run `[test command]` after code changes
- Run `[build command]` before marking complete

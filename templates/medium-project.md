# [Project name]

<!-- Context file last updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->
<!-- If this repo uses AGENTS.md as the source, this content goes there and CLAUDE.md is the
     bridge. @path imports below are Claude-only — in a shared AGENTS.md use markdown links. -->

[One line - what this project does]

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
2. [Step 2 - e.g., Add tests in `__tests__/`]
3. [Step 3 - e.g., Update exports in index.ts]

## Key Files

- `@path/to/entry` - [purpose]
- `@path/to/config` - [purpose]
- `@path/to/key/module` - [purpose]

## Non-Obvious Knowledge

<!-- Tribal knowledge Claude can't infer. e.g. "Rate limiting at 100 req/min is intentional,
     not a bug" · "The legacy/ folder is deprecated on purpose — don't add code there" -->

- [Tribal knowledge 1]
- [Tribal knowledge 2]

## Do Not

- [Critical rule 1 - e.g., "Use `pnpm` exclusively"]
- [Critical rule 2 - e.g., "Preserve tmp/ for build caching"]
- [Critical rule 3 - e.g., "Run migrations before deploying"]

## Learned Lessons

<!-- Add an entry each time Claude gets something wrong.
     2026-02-15: deleted migration files → rule: "Never delete files in migrations/" -->

## Verification

- Run `[test command]` after code changes
- Run `[build command]` before marking complete

## Progressive Disclosure

- Architecture: @.claude/docs/architecture.md
- Development: @.claude/docs/development.md

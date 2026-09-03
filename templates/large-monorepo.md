# [Project name]

<!-- Context file last updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->
<!-- If this repo uses AGENTS.md as the source, this content goes there and CLAUDE.md is the
     bridge. @path imports below are Claude-only — in a shared AGENTS.md use markdown links.
     Consider a nested AGENTS.md per package instead of growing this file. -->

[One line - what this monorepo contains]

## Package Manager

**IMPORTANT:** Uses [yarn/npm/pnpm] workspaces (not [other package managers]).
Monorepo tool: **[Nx/Turborepo/Lerna]** at `[config path]`.

## Common Workflows

### Building & Running

1. `[install command]` - Install all dependencies
2. `[dev command]` - Start development server(s)
3. `[build command]` - Build all packages
4. `[affected:test]` - Test only affected packages

### [Workflow - e.g., Adding a New Package]

1. [Step 1 - e.g., Run `nx g @nx/lib:my-lib`]
2. [Step 2 - e.g., Add to tsconfig paths]
3. [Step 3 - e.g., Update workspace config]

### Cross-Package Changes

1. Change the dependency package, then `[build command]` it
2. Test dependents with `[test command]`
3. Version only after all tests pass

## Key Packages

| Package | Location          | Purpose             |
| ------- | ----------------- | ------------------- |
| [core]  | `packages/[core]` | [shared utilities]  |
| [ui]    | `packages/[ui]`   | [component library] |
| [app]   | `apps/[app]`      | [main application]  |

## Non-Obvious Knowledge

<!-- Tribal knowledge Claude can't infer. e.g. "Package A must be built before Package B" ·
     "Feature flags control rollouts, not feature branches" -->

- [Tribal knowledge 1]
- [Tribal knowledge 2]
- [Tribal knowledge 3]

## Do Not

- [Critical rule 1 - e.g., "Use `pnpm` exclusively"]
- [Critical rule 2 - e.g., "Build packages in dependency order"]
- [Critical rule 3 - e.g., "Never skip `affected:test` before pushing"]
- [Critical rule 4]

## Learned Lessons

<!-- 2026-02-15: built packages in wrong order → added explicit dependency order rule -->

## Verification

- Run `[affected:test]` after code changes, `[build command]` before marking complete

## Progressive Disclosure

- Architecture: @.claude/docs/architecture.md
- Development: @.claude/docs/development.md
- Services: @.claude/docs/services/

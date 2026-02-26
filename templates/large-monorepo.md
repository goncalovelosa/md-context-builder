# CLAUDE.md

<!-- CLAUDE.md Last Updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->

[One-line description - what this monorepo contains]

## Package Manager

**IMPORTANT:** Uses [yarn/npm/pnpm] workspaces (not [other package managers])

## Monorepo Tool

**[Nx/Turborepo/Lerna]** - Configured at `[config path]`

## Common Workflows

### Building & Running

1. `[install command]` - Install all dependencies
2. `[dev command]` - Start development server(s)
3. `[build command]` - Build all packages
4. `[test command]` - Run all tests
5. `[affected:test]` - Test only affected packages (faster)

### [Workflow 1 - e.g., Adding a New Package]

1. [Step 1 - e.g., Run `nx g @nx/lib:my-lib`]
2. [Step 2 - e.g., Add to appropriate tsconfig paths]
3. [Step 3 - e.g., Update workspace config]

### [Workflow 2 - e.g., Working on a Specific App]

1. [Step 1 - e.g., `nx serve app-name`]
2. [Step 2]
3. [Step 3]

### Cross-Package Changes

1. Make changes in dependency package
2. Run `[build command]` for that package
3. Test dependent packages with `[test command]`
4. Update version only after all tests pass

## Key Packages

| Package | Location          | Purpose             |
| ------- | ----------------- | ------------------- |
| [core]  | `packages/[core]` | [shared utilities]  |
| [ui]    | `packages/[ui]`   | [component library] |
| [app]   | `apps/[app]`      | [main application]  |

## Key Files

- `@path/to/root/config` - [monorepo configuration]
- `@path/to/workspace/config` - [workspace settings]
- `@path/to/shared/module` - [shared utilities]

## Non-Obvious Knowledge

<!-- CRITICAL: Capture tribal knowledge Claude can't infer -->
<!-- Example: "Package A must be built before Package B" -->
<!-- Example: "The 'legacy' folder is intentionally deprecated" -->
<!-- Example: "Feature flags control rollouts, not feature branches" -->

- [Tribal knowledge 1]
- [Tribal knowledge 2]
- [Tribal knowledge 3]
- [Tribal knowledge 4]

## Do Not

- [Critical rule 1 - e.g., "Use `pnpm` exclusively"]
- [Critical rule 2 - e.g., "Build packages in dependency order"]
- [Critical rule 3 - e.g., "Never skip `affected:test` before pushing"]
- [Critical rule 4 - e.g., "Preserve tmp/ folder for build caching"]
- [Critical rule 5]
- [Critical rule 6]

## Learned Lessons

<!-- Add entries when Claude makes mistakes -->
<!-- 2026-02-15: Claude built packages in wrong order → Added explicit dependency order rule -->

## Verification

- Run `[affected:test]` after code changes
- Run `[build command]` before marking complete

## Progressive Disclosure

- Architecture: @.claude/docs/architecture.md
- Development: @.claude/docs/development.md
- Services: @.claude/docs/services/

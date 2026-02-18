# CLAUDE.md

<!-- CLAUDE.md Last Updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[One-line description of this monorepo/project]

## Development Focus

*Only include this section if the repository is a git repo with >100 commits*

**Active Development** (since [last update date or "30 days ago"]):
- `path/to/` - Brief description
- `path/to/` - Brief description

**Historical Hotspots** (all-time most changed):
- `path/to/file` - N changes total
- `path/to/file` - N changes total

**File Coupling** (frequently changed together):
- `file1.ts` + `file2.ts`
- `file3.ts` + `file4.ts`

*See `@.claude/docs/git-insights.md` for detailed commit history analysis.*

## Tech Stack

- [Framework 1] [Version]
- [Framework 2] [Version]
- [Language] [Version]
- [Key library 1]
- [Key library 2]
- [Key library 3]
- [Build tool] [Version]

## Package Manager

**IMPORTANT:** Uses [yarn/npm/pnpm] workspaces (not [other package managers])

## Monorepo Tool

**[Nx/Turborepo/Lerna]** - Configured at `[config path]`

## Essential Commands

- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]

## Entry Points

- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]

## Key Directories

- `[dir]/` - [purpose]
- `[dir]/` - [purpose]
- `[dir]/` - [purpose]
- `[dir]/` - [purpose]
- `[dir]/` - [purpose]

## Repository Structure

```
[project-name]/
├── [dir]/           # [purpose]
├── [dir]/           # [purpose]
├── [dir]/           # [purpose]
├── packages/        # [Workspace packages]
│   ├── [pkg1]/      # [purpose]
│   ├── [pkg2]/      # [purpose]
│   └── [pkg3]/      # [purpose]
└── apps/            # [Applications]
    ├── [app1]/      # [purpose]
    └── [app2]/      # [purpose]
```

## Workspace Packages

| Package | Location          | Purpose   |
| ------- | ----------------- | --------- |
| [name]  | `packages/[name]` | [purpose] |
| [name]  | `packages/[name]` | [purpose] |
| [name]  | `apps/[name]`     | [purpose] |

## Key Files

- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]

## Do Not

- [Critical rule 1]
- [Critical rule 2]
- [Critical rule 3]
- [Critical rule 4]
- [Critical rule 5]
- [Critical rule 6]

## Verification

Add project-specific verification:

```markdown
## Verification

- Run `npm test` after code changes
- Run `npm run build` before marking complete
- Run `npm run lint` to check code quality
- Run `npm run typecheck` for TypeScript projects
```

This improves final quality 2-3x by giving Claude a way to verify its work.

## Auto-Format Hook (Recommended)

Prevent format drift by adding a PostToolUse hook to your Claude Code settings:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npm run format || true"
          }
        ]
      }
    ]
  }
}
```

The `|| true` pattern prevents format errors from blocking the session.

## Workflow

For complex tasks: Start in Plan mode, iterate on plan, then implement.

## Additional Context

- Architecture details: `.claude/docs/architecture.md`
- Development guide: `.claude/docs/development.md`
- Service documentation: `.claude/docs/services/`
- File inventory: `.claude/docs/general_index.md`
- Detailed index: `.claude/docs/detailed_index.md`

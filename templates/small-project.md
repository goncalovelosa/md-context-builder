# CLAUDE.md

<!-- CLAUDE.md Last Updated: [DATE: e.g., 2026-02-10T22:00:00Z] -->

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[One-line description of what this project does]

## Recent Activity

*Only include this section if the repository is a git repo with >20 commits and recent activity*

**Since [last update date or "30 days ago"]:**
- `path/to/file` - Brief description (N commits)
- `path/to/file` - Brief description (N commits)

## Tech Stack

- [Framework name] [Version]
- [Language] [Version]
- [Additional tool if critical]

## Package Manager

**IMPORTANT:** Uses [yarn/npm/pnpm] (not [other package managers])

## Essential Commands

- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]
- `[command]` - [what it does]

## Entry Point

- `[path]` - [purpose]

## Key Files

- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]
- `[path]` - [purpose]

## Do Not

- [Critical rule 1 - e.g., "Do not edit dist/ (auto-generated)"]
- [Critical rule 2]
- [Critical rule 3]

## Verification

Add project-specific verification:

```markdown
## Verification

- Run `npm test` after code changes
- Run `npm run build` before marking complete
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

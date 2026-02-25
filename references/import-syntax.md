# Import Syntax Reference

CLAUDE.md files can import additional files using `@path/to/import` syntax. This enables modular documentation organization.

## Basic Syntax

```markdown
See @README.md for project overview and @package.json for available npm commands.

# Additional Instructions

- Git workflow: @docs/git-instructions.md
- Personal overrides: @~/.claude/my-project-instructions.md
```

## Import Rules

- Both relative and absolute paths are allowed
- Home directory imports: `@~/.claude/my-instructions.md`
- Recursive imports up to 5 hops deep
- **Not evaluated inside markdown code spans and code blocks**

```markdown
# This code span will NOT be treated as an import:

`@anthropic-ai/claude-code`
```

## Viewing Loaded Imports

Run `/memory` command to see what memory files are loaded.

## Progressive Disclosure Patterns

### Pattern 1: By Abstraction Level

```markdown
CLAUDE.md (root)
├── Package manager (IMPORTANT)
├── Common workflows (procedural)
├── Non-obvious knowledge
└── Links to detailed docs

.claude/docs/architecture.md
├── System design decisions
├── Non-obvious integration patterns
└── Failure recovery procedures

.claude/docs/development.md
├── Setup instructions (step-by-step)
├── Testing workflow (procedural)
└── Debugging guide
```

### Pattern 2: By Domain/Surface Area

```markdown
CLAUDE.md (root)
├── Universal context
└── Links to domain-specific docs

.claude/docs/api.md
.claude/docs/database.md
.claude/docs/auth.md
.claude/docs/frontend.md
```

### Pattern 3: By Service (Monorepos)

```markdown
CLAUDE.md (root)
├── Monorepo overview
└── Links to service docs

.claude/docs/services/user-service.md
.claude/docs/services/payment-service.md
.claude/docs/services/notification-service.md
```

## Example Usage

```markdown
## Progressive Disclosure

Core patterns: @src/shared/domain/CLAUDE.md
Repository patterns: @src/shared/repositories/CLAUDE.md
Database workflow: @.claude/docs/database-workflow.md
Testing guide: @.claude/docs/testing.md
Error handling: @.claude/docs/error-handling.md
Auth & AuthZ: @.claude/docs/authentication-authorization.md
Task management: @.claude/docs/task-management.md
Personal overrides: @~/.claude/my-project-rules.md
```

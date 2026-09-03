# Auto-Format Hooks Reference

Prevent format drift by adding a PostToolUse hook for automatic formatting.

**When this skill reaches for it:** when the audit finds a `CLAUDE.md` prescribing style rules that
a formatter already enforces. Delete the rules, add the hook — a hook runs regardless of what
Claude decides, an instruction does not.

## Configuration

Add to your Claude Code settings:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit|MultiEdit",
        "hooks": [
          {
            "type": "command",
            "command": "pnpm format || true"
          }
        ]
      }
    ]
  }
}
```

## Why Auto-Formatting?

- Claude's code is usually well-formatted, but inconsistencies creep in
- Manual formatting interrupts flow
- Every file Claude touches gets auto-formatted automatically
- No format drift, no CI failures from linting

## The `|| true` Pattern

The `|| true` pattern prevents format errors from blocking the session. If the formatter fails for any reason, the session continues without interruption.

## Alternative Format Commands

```json
// For npm projects
"command": "npm run format || true"

// For projects with lint --fix — note the bare `--`, without it
// npm eats the flag instead of passing it to the script
"command": "npm run lint -- --fix || true"

// For Python projects
"command": "black . || true"

// For Go projects
"command": "gofmt -w . || true"
```

## Pre-commit Integration

If you use pre-commit hooks, the auto-format hook runs before your pre-commit hooks, ensuring code is formatted before it's even committed.

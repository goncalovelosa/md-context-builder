# Memory Hierarchy Reference

Claude Code supports multiple memory types in a hierarchical structure. Understanding this hierarchy is critical for effective CLAUDE.md generation.

## Memory Types Table

| Memory Type        | Location                                                                    | Purpose                                             | Use Case Examples                                    | Shared With                 |
| ------------------ | --------------------------------------------------------------------------- | --------------------------------------------------- | ---------------------------------------------------- | --------------------------- |
| **Managed policy** | `/Library/Application Support/ClaudeCode/CLAUDE.md` (macOS)                | Organization-wide instructions managed by IT/DevOps | Company coding standards, security policies          | All users in organization   |
| **Managed policy** | `/etc/claude-code/CLAUDE.md` (Linux)                                        | Organization-wide instructions managed by IT/DevOps | Company coding standards, security policies          | All users in organization   |
| **Managed policy** | `C:\Program Files\ClaudeCode\CLAUDE.md` (Windows)                         | Organization-wide instructions managed by IT/DevOps | Company coding standards, security policies          | All users in organization   |
| **Project memory** | `./CLAUDE.md` or `./.claude/CLAUDE.md`                                       | Team-shared instructions for the project            | Project architecture, coding standards, workflows    | Team members via source control |
| **Project rules**  | `./.claude/rules/*.md`                                                      | Modular, topic-specific project instructions        | Language-specific guidelines, testing conventions    | Team members via source control |
| **User memory**    | `~/.claude/CLAUDE.md`                                                       | Personal preferences for all projects               | Code styling preferences, personal tooling shortcuts | Just you (all projects)       |
| **Project local**  | `./CLAUDE.local.md`                                                         | Personal project-specific preferences               | Your sandbox URLs, preferred test data              | Just you (current project)    |

## Key Principles

**Precedence:** Files higher in the hierarchy take precedence and are loaded first, providing a foundation that more specific memories build upon.

## Advanced Features

### Symlink Support

The `.claude/rules/` directory supports symlinks for sharing common rules across projects:

```bash
# Symlink a shared rules directory
ln -s ~/shared-claude-rules .claude/rules/shared

# Symlink individual rule files
ln -s ~/company-standards/security.md .claude/rules/security.md
```

### User-Level Rules

Create personal rules that apply to all your projects in `~/.claude/rules/`:

```bash
~/.claude/rules/
├── preferences.md    # Your personal coding preferences
└── workflows.md      # Your preferred workflows
```

User-level rules are loaded before project rules, giving project rules higher priority.

### Memory Discovery

Claude Code reads memories recursively:

- Starting in the current working directory (cwd)
- Recurses up to (but not including) the root directory `/`
- Reads any CLAUDE.md or CLAUDE.local.md files found
- Also discovers CLAUDE.md nested in subtrees under cwd (loaded on-demand when working with files in those subtrees)

### Organization-Level Memory

Organizations can deploy centrally managed CLAUDE.md files:

1. Create file at Managed policy location (see table above)
2. Deploy via configuration management (MDM, Group Policy, Ansible)

## Quick Reference Commands

```bash
# View loaded memory
/memory

# Initialize new CLAUDE.md
/init

# Edit memory files
/memory

# View current session context
/context
```

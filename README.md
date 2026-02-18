# Project Context Creator

A Claude Code skill for generating CLAUDE.md files and `.claude/rules/` structure following progressive disclosure best practices.

## Features

- **Progressive Disclosure** - Generates concise root CLAUDE.md (< 60 lines) with detailed reference files
- **Project Size Detection** - Automatically detects small/medium/large projects and applies appropriate templates
- **@path Import Syntax** - Uses Claude's import syntax for cross-referencing documentation
- **Path-Specific Rules** - Creates `.claude/rules/` with YAML frontmatter for scoped rules
- **2026 Best Practices** - Aligned with latest community patterns from madigan.dev, groff.dev, and Anthropic's official guides

## Use Cases

1. **New Project Setup** - Create CLAUDE.md for a codebase without AI context documentation
2. **Documentation Audit** - Review existing CLAUDE.md against 2026 best practices
3. **Structure Migration** - Convert flat CLAUDE.md into progressive disclosure structure

## Installation

Clone this repository into your Claude Code skills directory:

```bash
cd ~/.claude/skills
git clone https://github.com/goncalovelosa/project-context-creator.git
```

## Usage

The skill automatically triggers when you ask Claude to:

- "Create a CLAUDE.md for this project"
- "Generate CLAUDE.md"
- "Update CLAUDE.md"
- "Audit my CLAUDE.md for best practices"
- "Set up .claude/rules/"

### Example

```
User: "Create a CLAUDE.md for this Node.js project"

Claude will:
1. Detect project characteristics (package.json, src/ structure)
2. Count files to determine project size
3. Identify package manager (npm/yarn/pnpm)
4. Generate CLAUDE.md with project overview, commands, key files, and do-not rules
```

## Project Size Templates

| Size | Files | Output |
|------|-------|--------|
| Small | < 50 | Single CLAUDE.md (~50 lines) |
| Medium | 50-500 | CLAUDE.md + 2-3 reference files |
| Large | 500+ | CLAUDE.md + comprehensive docs/ + .claude/rules/ |

## Structure

```
project-context-creator/
├── SKILL.md                    # Main skill definition
├── references/                 # Reference documentation
│   ├── memory-hierarchy.md     # Claude memory hierarchy
│   ├── import-syntax.md        # @path syntax guide
│   ├── path-specific-rules.md  # YAML frontmatter patterns
│   ├── git-analysis.md         # Git history analysis
│   ├── auto-formatting.md      # PostToolUse hooks
│   └── best-practices-2026.md  # 2026 community patterns
├── analysis/                   # Analysis patterns
│   ├── detection-prompts.md
│   ├── git-history-analysis.md
│   └── pattern-extraction.md
└── templates/                  # Project size templates
    ├── small-project.md
    ├── medium-project.md
    └── large-monorepo.md
```

## Key Principles

1. **Less is more** - Target < 60 lines for root CLAUDE.md
2. **Progressive disclosure** - Detail in reference files, not root
3. **Import over copy** - Use `@path` syntax for references
4. **Path-specific rules** - Use `.claude/rules/` with YAML frontmatter
5. **Verify and prune** - Remove what doesn't prevent mistakes

## Compliance

This skill follows Anthropic's official skill building guide:
- ✅ Name avoids reserved terms ("claude", "anthropic")
- ✅ SKILL.md under 5,000 words
- ✅ Includes Use Cases, Examples, Troubleshooting, Success Criteria
- ✅ Uses ALWAYS/NEVER rule format
- ✅ Description includes trigger phrases

## References

- [Anthropic Skill Building Guide](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
- [Claude Code Memory Management](https://code.claude.com/docs/en/memory)
- [madigan.dev Best Practices](https://madigan.dev/blog/write-claudemd-files-that-actually-help)
- [groff.dev Agent Skills](https://www.groff.dev/blog/implementing-claude-md-agent-skills)

## License

MIT

---
name: project-context-creator
description: Generates CLAUDE.md files and .claude/rules/ structure following progressive disclosure best practices. Use when user asks to 'create CLAUDE.md', 'generate CLAUDE.md', 'update CLAUDE.md', 'audit documentation', 'set up .claude/rules', or mentions CLAUDE.md, project documentation setup, or AI assistant context files.
---

# Project Context Creator

Generates CLAUDE.md files and .claude/rules/ structure following progressive disclosure best practices.

## Use Cases

1. **New Project Setup** - Create CLAUDE.md for a codebase without AI context documentation
2. **Documentation Audit** - Review existing CLAUDE.md against 2026 best practices and suggest improvements
3. **Structure Migration** - Convert flat CLAUDE.md into progressive disclosure structure with .claude/rules/

## Quick Start

Invoke this skill when:

- Creating a new CLAUDE.md file
- Updating an existing CLAUDE.md file
- Auditing a CLAUDE.md for best practices
- Setting up `.claude/rules/` directory structure

The skill will:

1. Analyze the codebase to detect project characteristics
2. Select the appropriate template based on project size
3. Generate CLAUDE.md following progressive disclosure principles
4. Create supporting documentation files for larger projects
5. Set up path-specific rules with YAML frontmatter (if applicable)

## ALWAYS / NEVER

### ALWAYS
- Always target < 60 lines for root CLAUDE.md
- Always use `@path` syntax for file references
- Always mark package manager as **IMPORTANT**
- Always include "Do Not" section
- Always defer to existing linters/formatters

### NEVER
- Never exceed 5,000 words in any single documentation file
- Never include information Claude can figure out from reading code
- Never prescribe style rules that conflict with existing linters
- Never duplicate information across files

## Analysis Workflow

### 1. Check for Existing CLAUDE.md

- If exists: Offer to update or regenerate
- If not exists: Proceed with creation

Also check for:
- `.claude/rules/` directory (path-specific rules)
- `.claude/CLAUDE.md` (alternative project memory location)
- `CLAUDE.local.md` (local overrides)

### 2. Determine Project Size

| Category       | File Count   | Structure                                        |
| -------------- | ------------ | ------------------------------------------------ |
| Small          | < 50 files   | Single CLAUDE.md                                 |
| Medium         | 50-500 files | CLAUDE.md + 2-3 reference files                  |
| Large/Monorepo | 500+ files   | CLAUDE.md + comprehensive docs/ + .claude/rules/ |

### 3. Detect Project Characteristics

**Package Manager:**
- `package.json` → Check for `yarn.lock`, `package-lock.json`, `pnpm-lock.yaml`
- `pyproject.toml` → Python/poetry
- `go.mod` → Go modules
- `Cargo.toml` → Rust

**Frameworks:** JavaScript/TypeScript (Next.js, React, Vue), Python (Django, FastAPI), etc.

**Monorepo Detection:**
- `nx.json` → Nx monorepo
- `turbo.json` → Turborepo
- `lerna.json` → Lerna
- `package.json` with `workspaces` → Yarn workspaces

### 4. Map Key Directories

- `src/`, `lib/`, `app/` → Source code
- `tests/`, `test/`, `__tests__/` → Test files
- `docs/` → Documentation
- `config/` → Configuration

### 5. Analyze Git History

For repositories with >20 commits, analyze recent activity.

**Reference:** @references/git-analysis.md for comprehensive git analysis commands.

### 6. Identify Linters/Formatters

Check for: `.eslintrc.*`, `.prettierrc.*`, `.pylintrc`, `ruff.toml`, `.golangci.yml`

**Important:** Defer to these tools rather than prescribing style.

## Generation Workflow

### 1. Select Template

Based on project size:
- **Small**: `templates/small-project.md`
- **Medium**: `templates/medium-project.md`
- **Large**: `templates/large-monorepo.md`

### 2. Confirm Structure with User

For medium/large projects, ask about:
- Which reference files to create
- Whether to create `.claude/rules/` with path-specific rules
- Whether to include architecture details

### 3. Generate Root CLAUDE.md

**Target:** 40-60 lines maximum

**Timestamp Comment:** Add at the very top:

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-18T12:00:00Z -->
```

Required sections:
- Project Overview (one line)
- Tech Stack (framework and language)
- Package Manager (marked **IMPORTANT**)
- Essential Commands (3-5 most common)
- Entry Point
- Key Files (3-7 files with `@path` references)
- Do Not (2-5 critical rules)
- Progressive Disclosure links

### 4. Create Reference Files (if applicable)

For medium projects:
- `.claude/docs/architecture.md`
- `.claude/docs/development.md`

For large projects:
- `.claude/docs/services/[service-name].md`
- `.claude/rules/` with path-specific rules

### 5. Use Import Syntax

Reference other documentation using `@path` syntax:

```markdown
## Progressive Disclosure

Domain patterns: @src/shared/domain/CLAUDE.md
Database workflow: @.claude/docs/database-workflow.md
Testing: @.claude/docs/testing.md
```

**Reference:** @references/import-syntax.md for full syntax documentation.

## Templates Reference

### Small Project (< 50 files)

**Output:** Single CLAUDE.md file, ~40-60 lines

```markdown
# My Project

Simple web application.

**Stack:** Node.js, Express, TypeScript

## Commands

`npm run dev|test|build`

**Use `npm` only**

## Key Files

@src/index.ts | @src/routes/index.ts

## Do Not

- Never edit dist/ files
- Never commit .env
```

### Medium Project (50-500 files)

**Output:** CLAUDE.md (~40-50 lines) + 2-3 reference files

### Large Monorepo (500+ files)

**Output:** CLAUDE.md (~40-50 lines) + comprehensive docs/ + .claude/rules/

**Reference:** @templates/ for full template files.

## Token Budget Targets

| Metric | Target | Why |
|--------|--------|-----|
| Root CLAUDE.md | < 2,500 tokens | Loads on every conversation |
| Reference files | < 5,000 words | Progressive disclosure limit |

**Rule:** Every line must earn its place. If removing it wouldn't cause mistakes, delete it.

## Best Practices Checklist

### Root CLAUDE.md
- [ ] Line count < 60 lines
- [ ] "Do Not" section present
- [ ] Package manager marked **IMPORTANT**
- [ ] File references use `@path` format
- [ ] Progressive disclosure uses import syntax

### Progressive Disclosure
- [ ] Task-specific details moved to reference files
- [ ] Clear links from root to reference files using `@path`

### .claude/rules/ (if applicable)
- [ ] YAML frontmatter with `paths` for scoped rules
- [ ] Rules are focused and single-purpose

## Examples

### Example 1: Creating CLAUDE.md for New Project

**User says:** "Create a CLAUDE.md for this Node.js project"

**Actions:**
1. Detect project characteristics (package.json, src/ structure)
2. Count files to determine project size (small/medium/large)
3. Identify package manager (npm/yarn/pnpm)
4. Map key directories and entry points
5. Generate CLAUDE.md using small-project template
6. Add timestamp comment for future incremental updates

**Result:** Single CLAUDE.md file (~50 lines) with project overview, commands, key files, and do-not rules.

### Example 2: Auditing Existing CLAUDE.md

**User says:** "Audit my CLAUDE.md for best practices"

**Actions:**
1. Read existing CLAUDE.md
2. Check line count against 60-line target
3. Verify token budget (< 2,500 tokens)
4. Check for missing sections (Do Not, Progressive Disclosure)
5. Validate @path syntax usage
6. Identify opportunities for progressive disclosure
7. Generate audit report with specific recommendations

**Result:** Audit report listing issues found and suggested improvements.

### Example 3: Setting Up .claude/rules/

**User says:** "Set up .claude/rules/ for my TypeScript/Python monorepo"

**Actions:**
1. Analyze directory structure to identify file type boundaries
2. Create .claude/rules/ directory
3. Generate TypeScript rules with path scope `src/**/*.ts`
4. Generate Python rules with path scope `backend/**/*.py`
5. Create general.md for cross-cutting concerns
6. Add YAML frontmatter with glob patterns

**Result:**
```
.claude/rules/
├── typescript.md    # Scoped to src/**/*.ts
├── python.md        # Scoped to backend/**/*.py
└── general.md       # No scope (applies to all)
```

## Troubleshooting

### Error: "CLAUDE.md is too long"

**Cause:** Root CLAUDE.md exceeds 60 lines or 2,500 tokens

**Solution:**
1. Identify task-specific sections (detailed patterns, API docs)
2. Move to reference files in `.claude/docs/`
3. Replace with `@path` import syntax
4. Keep only universally applicable information in root

### Error: "Claude ignores my CLAUDE.md rules"

**Cause:** Information Claude can figure out from reading code, or conflicting instructions

**Solution:**
1. Remove self-evident practices ("write clean code")
2. Focus on non-obvious rules ("use pnpm not npm")
3. Check for conflicting rules across memory hierarchy
4. Ensure rules are specific to your project, not general advice

### Error: "Path-specific rules not loading"

**Cause:** Incorrect glob pattern in YAML frontmatter

**Solution:**
1. Verify glob pattern syntax (use `**/*.ts` not `**.ts`)
2. Test pattern with `find . -name "pattern"`
3. Ensure YAML frontmatter has correct `---` delimiters
4. Check file is in `.claude/rules/` directory

### Error: "Import syntax not working"

**Cause:** Path incorrect or inside code block

**Solution:**
1. Use relative paths from CLAUDE.md location
2. Ensure import is NOT inside code span or code block
3. Verify file exists at referenced path
4. Check for typos in @path syntax

## Success Criteria

### Quantitative Metrics
- Root CLAUDE.md < 60 lines
- Root CLAUDE.md < 2,500 tokens
- All reference files < 5,000 words
- Zero duplicate information across files

### Qualitative Metrics
- New team member can understand project in < 5 minutes
- Claude follows rules without repeated instruction
- Documentation stays current with codebase changes
- Progressive disclosure prevents context overload

## References

### Reference Files
- **Memory Hierarchy:** @references/memory-hierarchy.md
- **Import Syntax:** @references/import-syntax.md
- **Path-Specific Rules:** @references/path-specific-rules.md
- **Git Analysis:** @references/git-analysis.md
- **Auto-Formatting:** @references/auto-formatting.md
- **Best Practices 2026:** @references/best-practices-2026.md

### Official Documentation
- **Memory Management:** https://code.claude.com/docs/en/memory
- **Best Practices:** https://code.claude.com/docs/en/best-practices
- **Claude Blog:** https://claude.com/blog/using-claude-md-files
- **Skill Building Guide:** https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf

### Community Sources (2026)
- **madigan.dev:** https://madigan.dev/blog/write-claudemd-files-that-actually-help
- **groff.dev:** https://www.groff.dev/blog/implementing-claude-md-agent-skills
- **shanraisshan GitHub:** https://github.com/shanraisshan/claude-code-best-practice/blob/main/CLAUDE.md
- **kylestratis.com:** https://kylestratis.com/posts/a-better-practices-guide-to-using-claude-code/

### Key Principles

1. **Less is more** - Target < 60 lines for root CLAUDE.md
2. **Progressive disclosure** - Detail in reference files, not root
3. **Import over copy** - Use `@path` syntax for references
4. **Path-specific rules** - Use `.claude/rules/` with YAML frontmatter
5. **Verify and prune** - Remove what doesn't prevent mistakes

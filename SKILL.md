---
name: project-context-creator
description: Guides creation of CLAUDE.md files and .claude/rules/ structure following progressive disclosure best practices. Use when user asks to 'create CLAUDE.md', 'generate CLAUDE.md', 'update CLAUDE.md', 'audit documentation', 'set up .claude/rules', or mentions CLAUDE.md, project documentation setup, or AI assistant context files.
---

# Project Context Creator

Guides human authoring of CLAUDE.md files and .claude/rules/ structure following progressive disclosure best practices.

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
2. Assess whether CLAUDE.md is actually needed (see "When to Skip")
3. Generate a CLAUDE.md DRAFT following progressive disclosure principles
4. Create supporting documentation files for larger projects
5. **PROMPT HUMAN REVIEW** - Ask user to add tribal knowledge and verify content
6. Set up path-specific rules with YAML frontmatter (if applicable)

**Important:** Generated content is a STARTING POINT. Human review and addition of tribal knowledge is REQUIRED for effectiveness. Research shows LLM-generated content hurts (-3%) while human-written helps (+4%).

## When to Skip CLAUDE.md - Decision Tree

Not every project needs CLAUDE.md. Research shows 16 of 84 tasks had NEGATIVE impact from Skills.

**Answer these questions:**

1. **Does README.md have setup instructions?** → YES: Skip setup section
2. **Does CONTRIBUTING.md exist?** → YES: Skip workflow sections
3. **Is it a standard framework?** (Create React App, Next.js defaults) → YES: Skip entirely or ultra-minimal only
4. **Is project < 10 files?** → YES: Skip entirely
5. **Is project well-documented?** (docs/, wiki) → YES: Generate ultra-minimal only

**Scoring:**

- 0-1 YES: Generate standard
- 2-3 YES: Generate minimal
- 4-5 YES: Skip or generate ultra-minimal

**Default:** When in doubt, generate LESS, not more.

**CLAUDE.md helps most when:**

- Domain-specific business logic Claude won't know
- Non-standard architecture or unusual conventions
- "Is this intentional?" product decisions
- Complex build/deployment procedures
- Past mistakes that need explicit rules to prevent

**Rule of thumb:** If removing CLAUDE.md wouldn't cause Claude to make mistakes, it's not needed.

## ALWAYS / NEVER

### ALWAYS

- Always target < 60 lines for root CLAUDE.md (prefer 30-40)
- Always prioritize PROCEDURAL knowledge (how-to) over DECLARATIVE (what-is)
- Always capture "Is this intentional?" type tribal knowledge
- Always focus on what Claude CANNOT infer from code
- Always use `@path` syntax for file references
- Always mark package manager as **IMPORTANT**
- Always add rules when Claude fails at a task (failure-driven)
- Always frame rules POSITIVELY when possible ("Use X" not "Don't use Y")
- Always defer to existing linters/formatters

### NEVER

- Never auto-generate entire CLAUDE.md without human review
- Never include information Claude can figure out from reading code
- Never exceed 5,000 words in any single documentation file
- Never write comprehensive overviews - focused content outperforms
- Never use negative framing when positive works
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

### 3. Check for Existing Documentation (REDUNDANCY CHECK)

**Context:** Research shows LLM-generated files are redundant with existing docs. When docs removed, LLM files HELP (+2.7%). Always check first.

**Check for:**

- `README.md` with setup instructions → Skip setup sections
- `CONTRIBUTING.md` with workflows → Skip workflow sections
- `docs/` directory with architecture → Skip architecture sections
- Existing `CLAUDE.md` → Offer to update, not replace

**Decision Matrix:**

| Existing Docs                  | Action                          |
| ------------------------------ | ------------------------------- |
| README + CONTRIBUTING          | Generate ULTRA-MINIMAL (tools only) |
| README only                    | Generate minimal (tools + non-obvious) |
| None                           | Generate standard               |
| Existing CLAUDE.md             | Offer incremental update        |

**Output:** Documentation gap analysis - what's missing that should be added

### 4. Detect Project Characteristics

**Tool Detection (HIGH VALUE per research):**

| Tool Type      | Detection Files                            | Example Output              |
| -------------- | ------------------------------------------ | --------------------------- |
| Package Manager | lock files                                 | pnpm-lock.yaml → "Use pnpm" |
| Test Runner    | config files                               | jest.config.js, pytest.ini  |
| Linter         | config files                               | .eslintrc, ruff.toml        |
| Formatter      | config files                               | .prettierrc, .black         |
| Build Tool     | config files                               | vite.config.ts, turbo.json  |

**Output:** Tool matrix showing what tools to document

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

### 5. Map Key Directories

- `src/`, `lib/`, `app/` → Source code
- `tests/`, `test/`, `__tests__/` → Test files
- `docs/` → Documentation
- `config/` → Configuration

### 6. Analyze Git History

For repositories with >20 commits, analyze recent activity.

**Reference:** @references/git-analysis.md for comprehensive git analysis commands.

### 7. Identify Linters/Formatters

Check for: `.eslintrc.*`, `.prettierrc.*`, `.pylintrc`, `ruff.toml`, `.golangci.yml`

**Important:** Defer to these tools rather than prescribing style.

## Generation Workflow

### 1. Select Template

Based on project size and existing documentation:

- **Ultra-Minimal**: `templates/ultra-minimal.md` - When README+CONTRIBUTING exist, or standard framework
- **Small**: `templates/small-project.md` - < 50 files, minimal existing docs
- **Medium**: `templates/medium-project.md` - 50-500 files
- **Large**: `templates/large-monorepo.md` - 500+ files

### 2. Confirm Structure with User

For medium/large projects, ask about:

- Which reference files to create
- Whether to create `.claude/rules/` with path-specific rules
- Whether to include architecture details

### 3. Generate Root CLAUDE.md DRAFT

**Target:** 30-40 lines maximum (research shows focused content outperforms)

**Timestamp Comment:** Add at the very top:

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-18T12:00:00Z -->
```

Required sections (procedural-first order):

1. **Package Manager** (marked **IMPORTANT**) - Most common source of errors
2. **Common Workflows** (procedural steps, not just command lists) - PRIMARY value
3. **Key Files** (3-7 files with `@path` references)
4. **Non-Obvious Knowledge** (tribal knowledge Claude can't infer)
5. **Do Not** (2-5 critical rules, positively framed)
6. **Learned Lessons** (placeholder for failure-driven documentation)
7. **Verification** (how to validate changes)
8. **Progressive Disclosure links** (for medium/large projects)

**NOT included (inferable from code):**

- Tech Stack (inferable from package.json)
- Entry Points (discoverable from code)
- Project Structure overviews (research shows these don't help)

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

### 6. Prompt Human Review (REQUIRED)

After generating the draft, explicitly ask the user to:

1. **Add tribal knowledge** - "Is this intentional?" decisions only they know
2. **Fill in Non-Obvious Knowledge section** - Domain-specific info Claude can't infer
3. **Review Do Not rules** - Ensure they match actual project constraints
4. **Verify workflows** - Confirm procedural steps are correct

**Example prompt:**

```
I've generated a CLAUDE.md draft. For it to be effective, please:

1. Add any "Is this intentional?" knowledge to Non-Obvious Knowledge
   (e.g., "Rate limiting at 100 req/min is expected, not a bug")

2. Review the Common Workflows - are the steps accurate?

3. Add any critical Do Not rules I might have missed

Research shows human-written content (+4%) outperforms LLM-generated (-3%).
```

## Content Quality Principles

### What to Include (Research-Backed)

1. **Non-obvious procedural knowledge** - Things Claude can't infer from code
2. **Tribal knowledge** - "Is this intentional?" type decisions
3. **Failure patterns** - Rules added because Claude made mistakes
4. **Package manager/toolchain specifics** - Marked IMPORTANT

### What to Exclude (Wastes Tokens)

1. **Obvious project facts** - "We use React" (inferable from package.json)
2. **Codebase overviews** - Research shows these don't help
3. **Comprehensive documentation** - Focused content outperforms
4. **Self-evident practices** - "Write clean code"

### Procedural > Declarative

```markdown
# GOOD (Procedural - HOW)

## Adding API Endpoints

1. Create route in src/routes/[domain].ts
2. Add types in src/types/api/[domain].ts
3. Run npm run test:api before committing

# BAD (Declarative - WHAT)

## Tech Stack

- We use TypeScript
- We use React
```

**Reference:** @references/effective-content.md for research-backed examples.

## Templates Reference

### Ultra-Minimal (Existing docs, standard framework)

**Output:** Single CLAUDE.md file, ~15-20 lines

Use when: README.md + CONTRIBUTING.md exist, or standard framework with no customization

```markdown
# My Project

## Tooling

**Package Manager:** pnpm (IMPORTANT)
**Test Runner:** jest
**Build Tool:** vite

## Commands

- `pnpm install` - Install dependencies
- `pnpm test` - Run tests
- `pnpm build` - Build for production

## Non-Obvious

- Rate limiting at 100 req/min is intentional - not a bug
```

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

| Metric          | Target         | Why                          |
| --------------- | -------------- | ---------------------------- |
| Root CLAUDE.md  | < 2,500 tokens | Loads on every conversation  |
| Reference files | < 5,000 words  | Progressive disclosure limit |

**Rule:** Every line must earn its place. If removing it wouldn't cause mistakes, delete it.

## Cost Impact Warning

**Research shows context files increase agent cost by 20%+**

Every line in CLAUDE.md:

- Increases tokens loaded on every conversation
- Increases agent exploration and testing
- May reduce task success if redundant

**Guidelines:**

- Ultra-minimal (15-20 lines): ~5% cost increase
- Minimal (30-40 lines): ~10% cost increase
- Standard (50-60 lines): ~15% cost increase
- Comprehensive (100+ lines): 20%+ cost increase

**Rule:** If a line doesn't prevent a specific mistake, remove it.

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

- Root CLAUDE.md < 60 lines (prefer 30-40)
- Root CLAUDE.md < 2,500 tokens
- All reference files < 5,000 words
- Zero duplicate information across files

### Qualitative Metrics

- New team member can understand project in < 5 minutes
- Claude follows rules without repeated instruction
- Documentation stays current with codebase changes
- Progressive disclosure prevents context overload

## Validation Workflow (Optional)

After generating CLAUDE.md, consider testing effectiveness:

1. **Identify a task** Claude previously struggled with
2. **Revert any changes** Claude made
3. **Start fresh session** with new CLAUDE.md
4. **Re-run the task** and compare results
5. **If no improvement:** CLAUDE.md may not have the right content

This "revert → re-run → verify" pattern ensures documentation actually helps.

## References

### Reference Files

- **Memory Hierarchy:** @references/memory-hierarchy.md
- **Import Syntax:** @references/import-syntax.md
- **Path-Specific Rules:** @references/path-specific-rules.md
- **Git Analysis:** @references/git-analysis.md
- **Auto-Formatting:** @references/auto-formatting.md
- **Best Practices 2026:** @references/best-practices-2026.md
- **Effective Content:** @references/effective-content.md

### Official Documentation

- **Memory Management:** https://code.claude.com/docs/en/memory
- **Best Practices:** https://code.claude.com/docs/en/best-practices
- **Claude Blog:** https://claude.com/blog/using-claude-md-files
- **Skill Building Guide:** https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf

### Research Sources (2026)

- **SkillsBench:** https://arxiv.org/pdf/2602.12670
- **Evaluating AGENTS.md:** https://arxiv.org/pdf/2602.11988
- **Hacker News Discussion:** https://news.ycombinator.com/item?id=47034087

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

# Project Context Creator

A Claude Code skill that **guides human authoring** of CLAUDE.md files following research-backed best practices.

## Research-Backed Approach

This skill incorporates findings from 2026 research:

| Source | Key Finding | Application |
|--------|-------------|-------------|
| [SkillsBench](https://arxiv.org/pdf/2602.12670) | Curated Skills: +16.2pp improvement | Focus on procedural knowledge |
| [Evaluating AGENTS.md](https://arxiv.org/pdf/2602.11988) | LLM-generated: -3%, Human-written: +4% | Guide humans, don't auto-generate |
| [Hacker News Discussion](https://news.ycombinator.com/item?id=47034087) | "4% improvement is massive!" | Even small gains matter |

**Core insight:** LLM-generated context files hurt performance. This skill generates drafts for human review, not final content.

## Features

- **Human Review Required** - Generates drafts with explicit prompts for tribal knowledge
- **Documentation Redundancy Check** - Skips content already in README/CONTRIBUTING
- **Decision Tree** - Helps determine if CLAUDE.md is even needed
- **Ultra-Minimal Option** - 15-20 line template for well-documented projects
- **Cost Warning** - Alerts that context files increase agent cost by 20%+
- **Progressive Disclosure** - Root CLAUDE.md < 60 lines with reference files

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
1. Check for existing documentation (README, CONTRIBUTING)
2. Detect project characteristics and tooling
3. Apply decision tree to determine template
4. Generate draft CLAUDE.md
5. Prompt human review for tribal knowledge
```

## Project Size Templates

| Size | Files | Output | When to Use |
|------|-------|--------|-------------|
| Ultra-Minimal | Any | ~15-20 lines | README + CONTRIBUTING exist, standard framework |
| Small | < 50 | ~40-60 lines | Minimal existing docs |
| Medium | 50-500 | CLAUDE.md + 2-3 refs | Complex workflows |
| Large | 500+ | CLAUDE.md + docs/ + rules/ | Monorepo |

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
│   ├── best-practices-2026.md  # 2026 community patterns
│   └── effective-content.md    # Research-backed content examples
├── analysis/                   # Analysis patterns
└── templates/                  # Project size templates
    ├── ultra-minimal.md        # 15-20 lines, tools only
    ├── small-project.md
    ├── medium-project.md
    └── large-monorepo.md
```

## Key Principles

1. **Less is more** - Target < 60 lines (prefer 30-40)
2. **Procedural > Declarative** - Focus on HOW, not WHAT
3. **Non-obvious only** - What Claude can't infer from code
4. **Human-written > LLM-generated** - Guide, don't replace
5. **Failure-driven** - Add rules when Claude makes mistakes

## When to Skip CLAUDE.md

Use the decision tree before generating:

1. README.md has setup instructions? → Skip setup section
2. CONTRIBUTING.md exists? → Skip workflow sections
3. Standard framework? → Skip or ultra-minimal only
4. < 10 files? → Skip entirely
5. Well-documented (docs/, wiki)? → Ultra-minimal only

**Scoring:** 4-5 YES → Skip; 2-3 YES → Minimal; 0-1 YES → Standard

## Cost Impact

Research shows context files increase agent cost by 20%+:

- Ultra-minimal (15-20 lines): ~5% cost increase
- Minimal (30-40 lines): ~10% cost increase
- Standard (50-60 lines): ~15% cost increase
- Comprehensive (100+ lines): 20%+ cost increase

**Rule:** If a line doesn't prevent a specific mistake, remove it.

## Compliance

This skill follows Anthropic's official skill building guide:

- Name avoids reserved terms ("claude", "anthropic")
- SKILL.md under 5,000 words
- Includes Use Cases, Examples, Troubleshooting, Success Criteria
- Uses ALWAYS/NEVER rule format
- Description includes trigger phrases

## References

### Research Sources (2026)
- [SkillsBench](https://arxiv.org/pdf/2602.12670) - Systematic evaluation of curated Skills
- [Evaluating AGENTS.md](https://arxiv.org/pdf/2602.11988) - Context file effectiveness study
- [Hacker News Discussion](https://news.ycombinator.com/item?id=47034087) - Practitioner insights

### Official Documentation
- [Anthropic Skill Building Guide](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)
- [Claude Code Memory Management](https://code.claude.com/docs/en/memory)

### Community Sources (2026)
- [madigan.dev Best Practices](https://madigan.dev/blog/write-claudemd-files-that-actually-help)
- [groff.dev Agent Skills](https://www.groff.dev/blog/implementing-claude-md-agent-skills)
- [shanraisshan GitHub](https://github.com/shanraisshan/claude-code-best-practice/blob/main/CLAUDE.md)
- [kylestratis.com Guide](https://kylestratis.com/posts/a-better-practices-guide-to-using-claude-code/)

## License

MIT

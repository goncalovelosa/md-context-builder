# Best Practices 2026 Reference

Modern patterns from 2026 community sources for CLAUDE.md generation.

## Independent Thought Section (madigan.dev)

Include a dedicated "Independent Thought" section that encourages Claude to:

1. Analyze the problem before proposing solutions
2. Consider multiple approaches
3. Explain reasoning before implementation

```markdown
## Independent Thought

Before implementing:
1. Analyze the requirements and constraints
2. Consider 2-3 alternative approaches
3. Explain your reasoning
4. Only then proceed with implementation
```

## 3-Tier Architecture Pattern (groff.dev)

Structure documentation in three tiers:

| Tier | Content | Location |
|------|---------|----------|
| **Tier 1** | Universal context - always loaded | Root CLAUDE.md |
| **Tier 2** | Domain-specific context | .claude/docs/*.md |
| **Tier 3** | Task-specific context | .claude/rules/*.md |

## ALWAYS/NEVER Rule Format

Standardize rules using explicit ALWAYS/NEVER format:

```markdown
## Code Rules

### ALWAYS
- Always use TypeScript strict mode
- Always validate inputs at API boundaries
- Always write tests for new features

### NEVER
- Never commit directly to main
- Never use `any` type
- Never skip error handling
```

## Skill Safety Checklist (groff.dev)

Before implementing changes:

- [ ] Does this change affect security?
- [ ] Does this change affect data integrity?
- [ ] Does this change affect performance?
- [ ] Does this change affect backwards compatibility?
- [ ] Should this change require user confirmation?

## Command → Agent → Skills Pattern (shanraisshan)

Organize automation in hierarchy:

1. **Commands** - Simple, repeatable actions (`/commit`, `/test`)
2. **Agents** - Complex, multi-step autonomous tasks
3. **Skills** - Domain expertise and guidance

## Token Budget Awareness

| Metric | Target | Rationale |
|--------|--------|-----------|
| Root CLAUDE.md | < 2,500 tokens | Loads on every conversation start |
| Reference files | < 5,000 words each | Progressive disclosure limit |
| Total documentation | < 15,000 tokens | Prevent context rot |

**Rule of thumb:** At 32,000 tokens, models drop below 50% accuracy on recall tasks.

## Progressive Disclosure Principle

Every line must earn its place. Ask: "Would removing this cause Claude to make mistakes?"

If the answer is no, delete it or move it to a reference file.

## Living Documentation Rituals

1. **PR Integration** - Add `.claude` tag to PRs for discoveries
2. **Monthly Audit** - Schedule regular CLAUDE.md reviews
3. **Learnings Section** - Document mistakes to prevent repetition
4. **Team Contribution** - Make updating CLAUDE.md part of PR review

## References

- madigan.dev: https://madigan.dev/blog/write-claudemd-files-that-actually-help
- groff.dev: https://www.groff.dev/blog/implementing-claude-md-agent-skills
- shanraisshan: https://github.com/shanraisshan/claude-code-best-practice/blob/main/CLAUDE.md
- kylestratis.com: https://kylestratis.com/posts/a-better-practices-guide-to-using-claude-code/

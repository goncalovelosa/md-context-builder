# Best Practices 2026 Reference

Modern patterns from 2026 community sources and research for CLAUDE.md generation.

## Research-Backed Evidence Summary

**Citations verified: 2026-09-03** against arxiv 2602.11988 **v2** and 2602.12670 **v4**. Both
papers were substantially revised in June 2026; anything quoted here from an earlier version is
marked. Re-check before quoting these figures elsewhere.

| Content type                          | Effect                    | Source |
| ------------------------------------- | ------------------------- | ------ |
| Developer-written vs LLM-generated    | **+7% relative, p=0.038** | Evaluating AGENTS.md v2 (arxiv 2602.11988) |
| Developer-written, absolute           | +2.4% (p=0.21, **n.s.**)  | Evaluating AGENTS.md v2 |
| LLM-generated, absolute               | −0.5% / −2% (p=0.87 / 0.37, **n.s.**) | Evaluating AGENTS.md v2 |
| Context files, inference cost         | **> +20%**                | Evaluating AGENTS.md |
| LLM files where they are the ONLY docs| **+2.7%**                 | Evaluating AGENTS.md |
| Curated procedural Skills             | **+16.6pp**               | SkillsBench v4 (arxiv 2602.12670) |
| Comprehensive Skills                  | **−2.9pp**                | SkillsBench |
| Focused (2-3 modules)                 | **+18.6pp**               | SkillsBench |
| Self-generated Skills                 | **−1.3pp**                | SkillsBench |

**Read the significance column.** The absolute effects are not statistically significant, and the
paper notes no single repository showed a significant effect. The one solid finding is the
**relative** gap: developer-written context files beat LLM-generated ones. That is what justifies a
skill that guides a human instead of generating for them — not a claimed +4%.

⚠️ Earlier versions of this skill quoted **+4% / −3%**. Those are **v1** figures; v2 (2026-06-23)
removed them. SkillsBench's "16 of 84 tasks" is likewise v1/v2 — **v4 is 87 tasks**.

### Key Research Findings

**From SkillsBench (arxiv 2602.12670):**

- 2-3 Skills/modules are OPTIMAL; 4+ show diminishing returns
- Models CANNOT self-author procedural knowledge (-1.3pp)
- Skills are not universally helpful — a fifth of tasks showed negative deltas (16 of 84 in v1;
  v4 re-ran on 87 tasks)

**From Evaluating AGENTS.md (arxiv 2602.11988):**

- Context files do NOT provide effective repository overviews
- Instructions ARE followed - lack of improvement isn't from ignoring rules
- When docs removed from codebase, LLM-generated files HELP (+2.7%)

**From Hacker News Practitioners (id=47034087):**

- Practitioners read even a few points as worth it — *"4% is yuuuge. In hard projects, 1% is the
  difference…"*, *"4% improvement from a simple markdown file means it's a must-have."*
- Best use case: domain knowledge model is NOT aware of
- "Is this intentional?" type questions are most valuable
- Add rules ONLY when agent has failed at a task
- Comprehensive AGENTS.md files often HURT

## Procedural vs Declarative Knowledge

**Procedural (HOW)** — this skill's strong preference, and consistent with SkillsBench's finding that focused procedural Skills outperform comprehensive documentation. No published ratio quantifies it; treat "procedural beats declarative" as a direction, not a multiplier.

```markdown
## Adding API Endpoints

1. Create route in src/routes/[domain].ts
2. Add types in src/types/api/[domain].ts
3. Register in src/routes/index.ts
4. Run npm run test:api before committing
```

**Declarative (WHAT)** - Inferable from code, wastes tokens

```markdown
## Tech Stack

- We use TypeScript ← Claude can see package.json
- We use React ← Claude can see imports
```

**Rule:** If Claude can infer it from reading files, don't write it.

## Failure-Driven Documentation

The most effective rules come from mistakes, not theory.

```markdown
## Learned Lessons

<!-- Add entries when Claude makes mistakes -->

- 2026-02-15: Claude deleted migration files → Added rule: "Never delete files in migrations/"
- 2026-02-20: Claude used npm instead of pnpm → Emphasized package manager as IMPORTANT
- 2026-02-25: Claude missed rate limiting was intentional → Added "Rate limiting returns 429 is EXPECTED"
```

**Pattern:** When Claude fails → Add specific rule → Test with revert → Verify improvement

## Tribal Knowledge (Non-Obvious)

Capture "Is this intentional?" knowledge that can't be inferred:

```markdown
## Product Decisions

- Rate limiting at 100 req/min is intentional - not a bug
- User IDs are immutable by design - never add mutation endpoint
- The "legacy" folder is intentionally deprecated
- We use feature flags, not feature branches for rollouts
```

## When CLAUDE.md Helps Most

**High Value:**

- Domain knowledge Claude's training data lacks
- Product decisions that violate typical patterns
- Non-obvious build/deployment procedures
- Failure patterns from past mistakes

**Low Value:**

- Standard framework patterns (Next.js, React, etc.)
- Information inferable from package.json, config files
- General coding best practices
- Comprehensive project overviews

## Validation Workflow

After generating CLAUDE.md, test effectiveness:

1. **Identify a task** Claude previously struggled with
2. **Revert any changes** Claude made
3. **Start fresh session** with new CLAUDE.md
4. **Re-run the task** and compare results
5. **If no improvement:** CLAUDE.md may not have the right content

---

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

| Tier       | Content                           | Location                        |
| ---------- | --------------------------------- | ------------------------------- |
| **Tier 1** | Universal context - always loaded | Root `CLAUDE.md`                |
| **Tier 2** | Invoked expertise                 | `.claude/skills/<name>/SKILL.md`|
| **Tier 3** | Deep reference, read on demand    | `docs/agent-guides/<name>.md`   |

Those are Groff's tiers as published. This skill puts Tier 2/3 material in `.claude/docs/` and
`.claude/rules/` instead — a deliberate divergence, not his recommendation.

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

## Adopting Third-Party Skills Safely (groff.dev)

Before installing a skill someone else wrote:

- [ ] Read the raw `SKILL.md` — all of it, not the README
- [ ] Audit for exfiltration patterns and anything that phones home
- [ ] Check it matches your conventions rather than fighting them
- [ ] Restrict its permissions (`allowed-tools`) to what it actually needs
- [ ] Remove skills that have gone stale

## Change Impact Checklist

Not from Groff — this skill's own. Before implementing a change:

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

| Metric              | Target             | Rationale                         |
| ------------------- | ------------------ | --------------------------------- |
| Root CLAUDE.md      | < 2,500 tokens     | Loads on every conversation start |
| Reference files     | < 5,000 words each | Progressive disclosure limit      |
| Total documentation | < 15,000 tokens    | Prevent context rot               |

These targets are this skill's own convention, not measured findings. The one measured number here
is the inference-cost one: context files cost over 20% more. Claude Code's own guidance is under
200 lines per `CLAUDE.md`, with a hard 4 MiB skip.

## Progressive Disclosure Principle

Every line must earn its place. Ask: "Would removing this cause Claude to make mistakes?"

If the answer is no, delete it or move it to a reference file.

## Living Documentation Rituals

1. **PR Integration** - Add `.claude` tag to PRs for discoveries
2. **Monthly Audit** - Schedule regular CLAUDE.md reviews
3. **Learnings Section** - Document mistakes to prevent repetition
4. **Team Contribution** - Make updating CLAUDE.md part of PR review

## References

- madigan.dev: <https://madigan.dev/blog/write-claudemd-files-that-actually-help>
- groff.dev: <https://www.groff.dev/blog/implementing-claude-md-agent-skills>
- shanraisshan: <https://github.com/shanraisshan/claude-code-best-practice/blob/main/CLAUDE.md>
- kylestratis.com: <https://kylestratis.com/posts/a-better-practices-guide-to-using-claude-code/>

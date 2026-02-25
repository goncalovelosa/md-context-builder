# Effective Content Reference

Research-backed guidance on what content helps vs hurts in CLAUDE.md files.

## Evidence Summary

| Content Type                | Effect  | Evidence Source                         |
| --------------------------- | ------- | --------------------------------------- |
| Curated procedural Skills   | +16.2pp | SkillsBench (arxiv 2602.12670)          |
| Human-written AGENTS.md     | +4%     | Evaluating AGENTS.md (arxiv 2602.11988) |
| LLM-generated AGENTS.md     | -3%     | Evaluating AGENTS.md                    |
| Comprehensive documentation | -2.9pp  | SkillsBench                             |
| Focused (2-3 modules)       | +18.6pp | SkillsBench                             |
| Detailed/compact Skills     | +18.8pp | SkillsBench                             |

## Good Content Examples

### Example 1: Non-Obvious Procedural Knowledge

```markdown
## Adding API Endpoints

1. Create route in src/routes/[domain].ts
2. Add types in src/types/api/[domain].ts
3. Register in src/routes/index.ts
4. Run npm run test:api before committing
```

**Why it works:** Specific procedure Claude can't infer from file structure. Captures the actual workflow, not just what files exist.

### Example 2: Tribal Knowledge

```markdown
## Product Decisions

- Rate limiting returns 429 for >100 req/min - this is EXPECTED, not a bug
- User IDs are immutable by design - never add mutation endpoint
- The "legacy" folder is intentionally deprecated, don't add new code there
- We use feature flags for rollouts, not feature branches
```

**Why it works:** Captures "Is this intentional?" knowledge that Claude cannot infer from code.

### Example 3: Failure-Driven Rule

```markdown
## Learned Lessons

- 2026-02-15: Claude deleted migration files → Added rule: "Never delete files in migrations/"
- 2026-02-20: Claude used npm instead of pnpm → Emphasized package manager as IMPORTANT
```

**Why it works:** Rules derived from actual mistakes, not theoretical concerns.

### Example 4: Build Procedure

```markdown
## Build Process

1. Run `pnpm install` (NOT npm)
2. Run `pnpm run generate:types` BEFORE building
3. Run `pnpm build`
4. If step 2 fails, check API server is running at localhost:3001
```

**Why it works:** Captures non-obvious dependencies and failure recovery.

## Bad Content Examples

### Example 1: Obvious Declarative (Wastes Tokens)

```markdown
## Tech Stack

- We use TypeScript
- We use React
- We use PostgreSQL
```

**Why it fails:** Claude can infer this from package.json, imports, and schema files. Zero value added.

### Example 2: Comprehensive Overview (Hurts Performance)

```markdown
## Project Structure

src/
├── components/ # React components
│ ├── Button.tsx # Button component
│ ├── Input.tsx # Input component
│ └── Modal.tsx # Modal component
├── hooks/ # Custom hooks
│ ├── useAuth.ts # Authentication hook
│ └── useApi.ts # API fetching hook
├── utils/ # Utility functions
│ ├── format.ts # Formatting utilities
│ └── validate.ts # Validation utilities
...
```

**Why it fails:** Research shows overviews don't help agents find files faster. This is 30+ lines of zero value.

### Example 3: Generic Best Practices

```markdown
## Code Quality

- Write clean code
- Follow DRY principles
- Use meaningful variable names
- Add comments for complex logic
```

**Why it fails:** Claude already knows this. These are general principles, not project-specific rules.

### Example 4: Negative Framing

```markdown
## Do Not

- Don't delete tmp/ folder
- Don't use npm
- Don't skip tests
```

**Why it fails:** Negative framing can backfire ("don't delete tmp" → deletes tmp). Better:

```markdown
## Do Not

- Use `pnpm` exclusively (not npm or yarn)
- Preserve tmp/ folder for build caching
- Run tests before committing
```

## Decision Framework

### Should I include X?

Ask these questions:

1. **Can Claude infer this from reading files?** → NO, don't include
2. **Is this domain-specific knowledge Claude won't know?** → YES, include
3. **Did Claude make a mistake that this rule would prevent?** → YES, include
4. **Is this a "Is this intentional?" product decision?** → YES, include
5. **Is this general coding advice?** → NO, don't include

### How much detail?

- **Root CLAUDE.md:** 30-40 lines max
- **Reference files:** < 5,000 words
- **Number of modules:** 2-3 optimal, 4+ diminishing returns

## Positive vs Negative Framing

| Negative (Backfires) | Positive (Works)                 |
| -------------------- | -------------------------------- |
| Don't use npm        | Use `pnpm` exclusively           |
| Don't delete tmp/    | Preserve tmp/ for build caching  |
| Don't skip tests     | Run tests before committing      |
| Don't commit .env    | Add secrets to .env.example only |

## When CLAUDE.md Helps Most

**High Impact Projects:**

- Domain-specific business logic
- Non-standard architectures
- Complex build/deployment pipelines
- Projects with unusual conventions

**Low Impact Projects:**

**Low Impact Projects:**

- Standard framework apps (Create React App, Next.js defaults)
- Well-documented open source patterns
- Simple CRUD applications

## References

- SkillsBench: <https://arxiv.org/pdf/2602.12670>
- Evaluating AGENTS.md: <https://arxiv.org/pdf/2602.11988>
- Hacker News Discussion: <https://news.ycombinator.com/item?id=47034087>

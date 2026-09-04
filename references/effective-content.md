# Effective Content Reference

Research-backed guidance on what content helps vs hurts in CLAUDE.md files.

## Evidence Summary

All numbers, their significance and the version caveats live in one place:
@references/best-practices-2026.md. The short version — the measured finding is **relative**
(developer-written context files beat LLM-generated ones by ~7%), not the absolute +4% this skill
used to quote.

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

## Is It Even a Pattern?

Before documenting a convention you spotted in the code, three checks:

1. **Does it hold?** If a pattern has many exceptions, it is not a pattern — do not document it.
2. **Does it matter?** Ask "what happens if someone doesn't follow this?"
   - Nothing → don't document
   - Confusion → consider documenting
   - Breakage → document it as a **Do Not**
3. **Is it ours?** Personal preferences, one-off occurrences, framework defaults and anything a
   linter or formatter already enforces all fail this. So does the self-evident ("functions do
   things").

The root file is for context Claude does not already have. Project-specific and non-obvious, or it
does not go in.

## State Is Not a Rule

The rule: **if it changes when the work advances, it does not belong in a context file.**

| Belongs | Belongs elsewhere |
| --- | --- |
| "Use `pnpm` exclusively" | "- [ ] Migrate the remaining 3 packages to pnpm" |
| "Rate limiting at 100 req/min is intentional" | "2026-02-15: raised the limit to 100" |
| A lesson: what broke, and the rule it produced | A changelog of everything that changed |

Tells to look for in an audit: checkbox lists · sections named *Pendentes* / *TODO* / *Next steps* /
*Registo* / *Changelog* · dated entries in reverse-chronological order · anything carrying a status
marker.

Why it accumulates: a rules file has no mechanism that forces a TODO to close. Nothing goes red.
So closed items stay, and they are paid on every session forever. One audit measured 80 of 407
lines — a fifth of the file — in a document whose own first line said it held "only the binding
rules".

`Learned Lessons` is the deliberate exception. A lesson is a rule with an origin story, and it
never closes.

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

- Standard framework apps (Create React App, Next.js defaults)
- Well-documented open source patterns
- Simple CRUD applications

## References

- SkillsBench: <https://arxiv.org/pdf/2602.12670>
- Evaluating AGENTS.md: <https://arxiv.org/pdf/2602.11988>
- Hacker News Discussion: <https://news.ycombinator.com/item?id=47034087>

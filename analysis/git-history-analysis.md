# Git History Analysis

This document provides guidance for analyzing git repositories to enhance CLAUDE.md generation with commit-based insights.

## Why Git History Analysis Matters

Git history provides valuable insights that static analysis cannot capture:

- **Developer Activity**: Shows what developers are actively working on (vs. AI activity tracked by claude-mem)
- **Hotspots**: Identifies files that change frequently (potential complexity or instability)
- **Coupling Detection**: Files that change together may have hidden dependencies
- **Commit Patterns**: Team conventions for commit messages, PR workflows
- **Recent Changes**: What's been actively developed (vs. legacy code)

**Complementary to claude-mem**: claude-mem tracks AI file reads/writes. Git history tracks developer commits. Together they provide complete project activity visibility.

## Repository Detection

First, confirm this is a git repository:

```bash
# Check if .git directory exists
git rev-parse --git-dir 2>/dev/null

# Get commit count
git rev-list --count HEAD 2>/dev/null
```

**Health Indicators:**

| Metric    | Healthy    | Caution    | Skip Analysis |
| --------- | ---------- | ---------- | ------------- |
| Commits   | > 100      | 20-100     | < 10          |
| Activity  | Last 30d   | Last 90d   | > 90d stale   |
| Depth     | Full clone | Shallow    | N/A           |

**When to skip:**
- Not a git repository
- Empty/new repo (< 10 commits)
- Shallow clone (note limited history in output)
- User explicitly opts out

## State Tracking: Last Updated Timestamp

Store a hidden HTML comment at the top of CLAUDE.md to enable incremental updates:

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-10T22:00:00Z -->
```

**Why this matters:**
- On first generation: Show commits from last 90 days
- On subsequent updates: Show ONLY commits since last update
- Makes "Recent Activity" sections meaningful (only NEW changes)
- Complements claude-mem's AI activity tracking

**Git command for state tracking:**

```bash
# Extract last update timestamp from existing CLAUDE.md
LAST_UPDATE=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')

# Get commits only since last update (or 90 days for first generation)
if [ -n "$LAST_UPDATE" ]; then
  git log --name-only --pretty=format:"%h|%ad|%s" --date=format:"%Y-%m-%d %H:%M" \
    --since="$LAST_UPDATE" | head -50
else
  git log --name-only --pretty=format:"%h|%ad|%s" --date=format:"%Y-%m-%d %H:%M" \
    --since="90 days ago" | head -50
fi
```

## Analysis Commands

### Recent Activity (First Generation)

For projects without an existing CLAUDE.md timestamp:

```bash
# Files changed in last 30-90 days
git log --name-only --pretty=format: --since="90 days ago" | \
  sort | uniq -c | sort -rn | head -20
```

### Recent Activity (Subsequent Updates)

For projects with existing CLAUDE.md timestamp:

```bash
# Get only NEW commits since last update
LAST_UPDATE=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')

if [ -n "$LAST_UPDATE" ]; then
  # Show commits with hash, date, message, and files
  git log --name-only --pretty=format:"%h|%ad|%s" \
    --date=format:"%Y-%m-%d %H:%M" \
    --since="$LAST_UPDATE" | head -50
else
  # Fallback to 90 days if no timestamp found
  git log --name-only --pretty=format:"%h|%ad|%s" \
    --date=format:"%Y-%m-%d %H:%M" \
    --since="90 days ago" | head -50
fi
```

### Historical Hotspots (All Time)

```bash
# Most frequently changed files
git log --name-only --pretty=format: | sort | uniq -c | sort -rn | head -30
```

### File Coupling Analysis

```bash
# Files that change together (within same commit)
git log --name-only --pretty=format:COMMIT_SEPARATOR --since="90 days ago" | \
  awk '/COMMIT_SEPARATOR/ {
    if (count > 1) print prev;
    count = 0;
    prev = ""
  }
  NF {
    if ($0 != "COMMIT_SEPARATOR") {
      files[count++] = $0;
      prev = prev $0 " "
    }
  }' | \
  grep -v '^$' | sort | uniq -c | sort -rn | head -20
```

### Commit Message Patterns

```bash
# Most common commit message patterns
git log --pretty=format:%s --since="6 months ago" | sort | uniq -c | sort -rn | head -20

# Commit by author
git log --pretty=format:"%an" --since="6 months ago" | sort | uniq -c | sort -rn
```

### Detailed Commit List with Context

```bash
# Commits since last update with full context
LAST_UPDATE=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')

if [ -n "$LAST_UPDATE" ]; then
  git log --since="$LAST_UPDATE" --pretty=format:"%h|%ad|%an|%s" \
    --date=format:"%Y-%m-%d %H:%M" --name-only | head -100
else
  git log --since="90 days ago" --pretty=format:"%h|%ad|%an|%s" \
    --date=format:"%Y-%m-%d %H:%M" --name-only | head -100
fi
```

## Fallback Strategies

| Situation                        | Action                                  |
| -------------------------------- | --------------------------------------- |
| Not a git repo                   | Skip git analysis, use static only      |
| Empty repo (< 10 commits)        | Omit git sections from output           |
| Shallow clone                    | Note limited history, adjust time window |
| Stale repo (> 90d no activity)   | Include historical data if meaningful   |
| No CLAUDE.md timestamp exists    | Use 90-day window for first generation  |

## CLAUDE.md Template Examples

### Small Project: Recent Activity Section

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-10T22:00:00Z -->

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[One-line description of what this project does]

## Recent Activity

*Last 30 days* - Active development in:
- `src/auth/` - Authentication flow updates
- `src/api/` - New user endpoints
- `tests/integration/` - Test coverage improvements

## Tech Stack
...
```

### Medium Project: Recent Activity Section

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-10T22:00:00Z -->

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[One-line description of what this project does]

## Recent Activity

**Since last update (Feb 10, 2026):**
- `src/components/Button.tsx` - Added variant prop (3 commits)
- `src/lib/api.ts` - Fixed error handling (2 commits)
- `src/pages/dashboard/` - New analytics views (5 commits)

**Historical hotspots** (most changed files):
- `src/types.ts` - 47 changes
- `src/lib/utils.ts` - 32 changes
- `src/components/Layout.tsx` - 28 changes

## Tech Stack
...
```

### Large Monorepo: Development Focus Section

```markdown
<!-- CLAUDE.md Last Updated: 2026-02-10T22:00:00Z -->

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

[One-line description of this monorepo/project]

## Development Focus

**Active Development** (since Feb 10, 2026):
- `packages/auth/` - OAuth integration work
- `services/api-gateway/` - Rate limiting improvements
- `apps/admin/` - Dashboard redesign

**Historical Hotspots** (all-time most changed):
- `packages/shared/types/` - Core type definitions
- `packages/database/migrations/` - Schema evolution
- `services/user-service/src/models/` - User domain model

**File Coupling** (frequently changed together):
- `packages/auth/src/types.ts` + `packages/auth/src/validation.ts`
- `services/api-gateway/routes/` + `services/api-gateway/middleware/`

See `@.claude/docs/git-insights.md` for detailed commit history analysis.

## Tech Stack
...
```

## Integration with Analysis Workflow

### When to Analyze Git History

1. **After static analysis completes** (directory structure, configs, patterns)
2. **Before template selection** (git history may influence template choice)
3. **After user confirmation** (some users may prefer to skip git analysis)

### Analysis Steps

```bash
# Step 1: Confirm git repository
if ! git rev-parse --git-dir >/dev/null 2>&1; then
  echo "Not a git repository - skipping git analysis"
  exit 0
fi

# Step 2: Check commit count
COMMIT_COUNT=$(git rev-list --count HEAD 2>/dev/null || echo "0")
if [ "$COMMIT_COUNT" -lt 20 ]; then
  echo "Repository has fewer than 20 commits - skipping git sections"
  exit 0
fi

# Step 3: Extract or generate timestamp
TIMESTAMP=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')
if [ -z "$TIMESTAMP" ]; then
  TIMESTAMP="90 days ago"
fi

# Step 4: Run analysis commands
# [Insert analysis commands from above]
```

### Conditional Section Inclusion

| Condition            | Include Section                      |
| -------------------- | ------------------------------------ |
| > 20 commits         | Recent Activity (small/medium)       |
| > 100 commits        | Historical Hotspots (medium/large)   |
| > 200 commits        | File Coupling (large only)           |
| Recent activity < 90d | Active Development (all sizes)     |

## Output Formatting Guidelines

### Recent Activity Section

```markdown
## Recent Activity

**Since [last update date]:**
- `path/to/file` - Brief description (N commits)
- `path/to/file` - Brief description (N commits)
```

### Development Focus Section (Large Projects)

```markdown
## Development Focus

**Active Development** (since [date]):
- `path/` - Description
- `path/` - Description

**Historical Hotspots:**
- `path/` - N changes total
- `path/` - N changes total

**File Coupling:**
- `file1.ts` + `file2.ts`
- `file3.ts` + `file4.ts`
```

## Best Practices

1. **Don't overwhelm**: Limit to top 10-20 items per section
2. **Use relative paths**: Keep paths concise (not `./packages/frontend/src/...`)
3. **Group by area**: Related files should be grouped together
4. **Provide context**: Explain why a file is a hotspot or changed recently
5. **Link to details**: For large projects, link to detailed analysis file
6. **Update timestamp**: Always update the timestamp comment when regenerating

## Example: Full Analysis Output

```bash
#!/bin/bash
# Complete git history analysis for CLAUDE.md generation

# Check if git repo
if ! git rev-parse --git-dir >/dev/null 2>&1; then
  echo "NOT_A_GIT_REPO"
  exit 0
fi

# Get commit count
COMMIT_COUNT=$(git rev-list --count HEAD 2>/dev/null || echo "0")
echo "COMMIT_COUNT:$COMMIT_COUNT"

# Get last update timestamp
TIMESTAMP=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')
if [ -z "$TIMESTAMP" ]; then
  TIMESTAMP="90 days ago"
fi
echo "SINCE:$TIMESTAMP"

# Recent activity
echo "---RECENT_ACTIVITY---"
if [ "$TIMESTAMP" = "90 days ago" ]; then
  git log --name-only --pretty=format: --since="90 days ago" | \
    sort | uniq -c | sort -rn | head -20
else
  git log --name-only --pretty=format:"%h|%ad|%s" \
    --date=format:"%Y-%m-%d %H:%M" --since="$TIMESTAMP" | head -50
fi

# Hotspots (if enough commits)
if [ "$COMMIT_COUNT" -gt 50 ]; then
  echo "---HOTSPOTS---"
  git log --name-only --pretty=format: | sort | uniq -c | sort -rn | head -30
fi

# Coupling (for large repos)
if [ "$COMMIT_COUNT" -gt 200 ]; then
  echo "---COUPLING---"
  git log --name-only --pretty=format:COMMIT_SEPARATOR --since="90 days ago" | \
    awk '/COMMIT_SEPARATOR/ {if (count > 1) print prev; count=0; prev=""} \
    NF {if ($0 != "COMMIT_SEPARATOR") {files[count++]=$0; prev=prev$0" "}}' | \
    grep -v '^$' | sort | uniq -c | sort -rn | head -20
fi
```

## Troubleshooting

### Issue: "fatal: not a git repository"

**Cause**: Directory is not initialized as a git repository
**Solution**: Skip git analysis gracefully, proceed with static analysis only

### Issue: "fatal: bad revision HEAD"

**Cause**: Empty repository with no commits
**Solution**: Check commit count first, skip if < 10

### Issue: "--since" parsing errors

**Cause**: Malformed timestamp from CLAUDE.md
**Solution**: Validate timestamp format, fallback to "90 days ago"

### Issue: Analysis takes too long

**Cause**: Large repository with extensive history
**Solution**: Add `--max-count` or narrow time window

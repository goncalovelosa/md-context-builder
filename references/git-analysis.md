# Git History Analysis Reference

Commands for analyzing git history to identify important files and patterns during CLAUDE.md generation.

## Timestamp Extraction

```bash
# Extract last update timestamp for incremental updates
LAST_UPDATE=$(grep "CLAUDE.md Last Updated:" CLAUDE.md 2>/dev/null | sed 's/.*: //')
```

## Recent Activity Analysis

```bash
# Get commits since last update (or 90 days for first generation)
if [ -n "$LAST_UPDATE" ]; then
  git log --name-only --pretty=format:"%h|%ad|%s" --date=format:"%Y-%m-%d %H:%M" \
    --since="$LAST_UPDATE" | head -50
else
  git log --name-only --pretty=format:"%h|%ad|%s" --date=format:"%Y-%m-%d %H:%M" \
    --since="90 days ago" | head -50
fi
```

## Historical Hotspots

For medium+ projects, identify frequently modified files:

```bash
git log --name-only --pretty=format: | sort | uniq -c | sort -rn | head -30
```

## File Coupling Analysis

For large projects, find files that frequently change together:

```bash
git log --name-only --pretty=format: --since="90 days ago" | awk 'BEGIN { RS = "" } {
  n = 0
  for (i = 1; i <= NF; i++) f[++n] = $i
  if (n < 2 || n > 20) next          # skip solo commits and sweeping refactors
  for (i = 1; i < n; i++) for (j = i + 1; j <= n; j++)
    print (f[i] < f[j] ? f[i] " + " f[j] : f[j] " + " f[i])
  delete f
}' | sort | uniq -c | sort -rn | head -20
```

Paragraph mode (`RS = ""`) is what splits the stream into commits — `--pretty=format:` leaves a
blank line between them. The pair is sorted before printing so `a + b` and `b + a` collapse into
one count. Commits touching more than 20 files are skipped: a sweeping refactor would otherwise
emit hundreds of meaningless pairs and dominate the ranking. Filenames containing spaces are split
on whitespace and will produce garbage rows; rare enough to live with, obvious enough to spot.

## Conditional Section Inclusion

Only spend root-file lines on git-derived sections when there is enough history to mean anything:

| Condition              | Include section                    |
| ---------------------- | ---------------------------------- |
| > 20 commits           | Recent Activity (small/medium)     |
| > 100 commits          | Historical Hotspots (medium/large) |
| > 200 commits          | File Coupling (large only)         |
| Recent activity < 90d  | Active Development (all sizes)     |

These sections are declarative and go stale on every commit. Prefer leaving them out of the root
file entirely and regenerating them on demand; include them only when the repo is large enough that
"which files move together" is genuinely non-obvious.

## Repository Health Indicators

| Commit Count | Health Status | Action |
|--------------|---------------|--------|
| > 100        | Healthy       | Proceed with git analysis |
| 20-100       | Caution       | Limited history, use with care |
| < 20         | Skip          | Not enough history for meaningful analysis |

## Git Repository Detection

```bash
# Check if git repo
git rev-parse --git-dir

# Get commit count
git rev-list --count HEAD
```

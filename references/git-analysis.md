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
git log --name-only --pretty=format:COMMIT_SEPARATOR --since="90 days ago" | \
  awk '/COMMIT_SEPARATOR/ {if (count > 1) print prev; count=0; prev=""} \
  NF {if ($0 != "COMMIT_SEPARATOR") {files[count++]=$0; prev=prev$0" "}}' | \
  grep -v '^$' | sort | uniq -c | sort -rn | head -20
```

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

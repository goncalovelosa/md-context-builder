# Memory Hierarchy Reference

Claude Code has **two** memory systems, both loaded at the start of every session:

- **CLAUDE.md files** — instructions you write.
- **Auto memory** — notes Claude writes itself from your corrections.

This skill is about the first. Know the second exists so you don't move team rules into it: auto
memory is machine-local and does not travel with the repo.

## Where CLAUDE.md files live

Listed in **load order**, broadest first.

| Scope                | Location                                                                                                 | Shared with                   |
| -------------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------- |
| **Managed policy**   | macOS `/Library/Application Support/ClaudeCode/CLAUDE.md` · Linux/WSL `/etc/claude-code/CLAUDE.md` · Windows `C:\Program Files\ClaudeCode\CLAUDE.md` | Everyone in the organization |
| **User**             | `~/.claude/CLAUDE.md`, `~/.claude/rules/`                                                                 | Just you, every project       |
| **Project**          | `./CLAUDE.md` or `./.claude/CLAUDE.md`, `./.claude/rules/`                                                | The team, via source control  |
| **Project local**    | `./CLAUDE.local.md`                                                                                       | Just you, this project        |

## They concatenate — nothing overrides anything

This is the part people get backwards. Every discovered file is **appended into context**, ordered
from the filesystem root down to your working directory, so **instructions closer to where you
launched Claude are read last**. Within a directory, `CLAUDE.local.md` comes after `CLAUDE.md`.

There is no precedence mechanism and no override. Two files that contradict each other produce a
contradiction, and Claude may pick either one — which is why the skill's redundancy check matters
more than any ordering rule. Managed policy is the one exception: it cannot be excluded.

Run `/context` to see which files actually loaded. Run `/memory` to open and edit them.

## Discovery

- Every directory from the working directory upward is read at launch — see **Ancestor files**
  below for what that costs and how to get out of it.
- `CLAUDE.md` in **subdirectories** below the working directory loads **on demand**, when Claude
  reads a file in that directory. This is the lazy-loading lever, along with `paths:` rules.
- `--add-dir` directories do **not** contribute memory files unless
  `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1` is set.

## Ancestor files — the case you can't see from inside

A `CLAUDE.md` above your working directory loads at launch, for that project **and every other one
beneath it**. Two things follow that don't apply to a project's own file:

1. **It is invisible from inside the project.** Nothing in the repo mentions it; `/context` is the
   only runtime view, so an audit has to walk the tree deliberately.
2. **Its cost multiplies** — every project below it, in every session, for as long as it exists.

Four exits, cheapest first:

| Exit | When |
| --- | --- |
| **Start Claude lower** | The fastest lever, and it needs no edit: launched at the repo root, only the root's file loads; launched in `packages/api/`, that directory's file *and every ancestor* load. To focus on one package, start there. |
| **Relocate** | The content applies to one project, not all of them. Move it down; that is where it was always cheapest. |
| **Shrink** | It genuinely applies everywhere, but not all of it earns the multiplied cost. |
| **`claudeMdExcludes`** | It is useful to a human but not to the agent one directory down — or it is another team's file and not yours to edit. |

Splitting works too, with a caveat: a repo-root `.claude/rules/` with `paths:` is the documented
answer for a monorepo root, and rules do load lazily. What a rule **cannot** do is reach upward —
rules are per-project, so a rules directory never tames a file above the repo.

⚠️ `claudeMdExcludes` in `.claude/settings.local.json` is machine-local and gitignored by design.
It fixes the file for you and for nobody else — the same "does it travel" failure described in
@references/agents-md-uniformity.md. Prefer the first three exits when the file is shared.

Ask who reads it, too. A file addressed to a tool that never loads it is pure cost.

## Size

Guidance is **under 200 lines** per file; this skill targets 30-60. The only hard limit is **4
MiB**, above which Claude Code skips the file entirely. Imports load at launch and therefore do
**not** reduce context — only lazy-loaded files do.

## Rules directories

`.claude/rules/*.md` load with the same priority as `.claude/CLAUDE.md`. Files carrying `paths:`
frontmatter load only when Claude reads a matching file. Both project and user rules directories
support symlinks:

```bash
ln -s ~/shared-claude-rules .claude/rules/shared
ln -s ~/company-standards/security.md .claude/rules/security.md
```

User rules (`~/.claude/rules/`) load before project rules.

See @references/path-specific-rules.md for the frontmatter, and
@references/agents-md-uniformity.md for why nested `AGENTS.md` often beats `.claude/rules/` when
more than one agent works the repo.

## Excluding files you don't want

In a monorepo where other teams' files get picked up, `claudeMdExcludes` skips them by absolute-path
glob. Put it in `.claude/settings.local.json` so the exclusion stays on your machine:

```json
{ "claudeMdExcludes": ["**/monorepo/CLAUDE.md", "/home/user/monorepo/other-team/.claude/rules/**"] }
```

Managed policy files cannot be excluded.

## Auto memory, in one paragraph

Claude writes it, not you. It lives in `~/.claude/projects/<project>/memory/` — a `MEMORY.md` index
whose first 200 lines (or 25 KB) load every session, plus topic files Claude reads on demand. The
project key comes from the git repository, so **all worktrees of one repo share it**. It is
machine-local: not in the repo, not shared with the team, not on your other machines. Toggle it in
`/memory` or with `autoMemoryEnabled`.

## Commands

| Command    | What it does                                            |
| ---------- | ------------------------------------------------------- |
| `/context` | Which memory files actually loaded — the only real check |
| `/memory`  | List and edit memory files; toggle auto memory           |
| `/init`    | Generate a starting CLAUDE.md                            |
| `/doctor`  | Propose trims for a checked-in CLAUDE.md                 |

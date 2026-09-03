# One Source of Truth: AGENTS.md + CLAUDE.md

Claude Code reads `CLAUDE.md`. It does not read `AGENTS.md`. Cursor, Codex and Copilot read
`AGENTS.md`. When a repo keeps both as real files they diverge, and each agent obeys a different
version of the rules.

**Rule: `AGENTS.md` is the source. `CLAUDE.md` is a pointer to it. Never two real files.**

If a repo has `AGENTS.md` and no `CLAUDE.md`, Claude Code still loads user, managed and ancestor
memory — but none of that repo's own instructions. The bridge below is the fix.

## Pick the bridge

| Bridge | Command | Use when |
| ------- | -------------------------- | ---------------------------------------------------------- |
| Symlink | `ln -s AGENTS.md CLAUDE.md` | No Claude-only content needed. Zero duplication, no drift.  |
| Import  | `CLAUDE.md` = `@AGENTS.md` | You need Claude-only rules, or the team includes Windows.   |

With the import you can add Claude-only content below it:

```markdown
@AGENTS.md

## Claude Code

Use plan mode for changes under `src/billing/`.
```

With the symlink there is no room for Claude-only content in `CLAUDE.md` — put it in
`.claude/CLAUDE.md` instead, which is a documented project-memory location and loads alongside.

On Windows a symlink needs Administrator or Developer Mode, and a checkout with
`core.symlinks=false` materialises a committed symlink as a text file containing the path. Both
are reasons to prefer the import on mixed teams.

Do not use `/import` for this: it appends a **one-time copy** of `AGENTS.md` into `CLAUDE.md`,
which is exactly the divergence you are preventing. It is a migration tool, not a bridge.

**Verify in a new session** — the bridge is read at startup, so `/context` in the session where you
created it proves nothing. Start a session and confirm `CLAUDE.md` appears under **Memory files**.

## The trap: `CLAUDE.md` is often gitignored

Developers commonly put `CLAUDE.md` and `.claude/` in their global gitignore
(`core.excludesFile`) to keep agent config personal. The bridge then works on their machine and
exists for nobody else — not teammates, not CI agents, and not git worktrees, which are fresh
checkouts. Check before writing it:

```bash
git check-ignore -v --no-index CLAUDE.md .claude/rules/x.md
```

No output and exit status 1 means nothing is ignored — that is the good case. A printed rule means
the file will be silently uncommittable. `--no-index` matters: without it, a file that is already
tracked is reported as clean even when a rule matches it.

Repo-level `.gitignore` takes precedence over the global file, so re-include explicitly:

```gitignore
!CLAUDE.md
```

A committed symlink is stored as git mode `120000` — the target path, a few bytes, not a copy.
Confirm with `git ls-files -s CLAUDE.md`. Worktrees materialise it correctly.

To also commit `.claude/rules/` without dragging in personal settings:

```gitignore
!.claude/
.claude/*
!.claude/rules/
```

## `@path` imports do not travel

`@path` expansion is a Claude Code feature. Cursor, Codex and Copilot read an `@path` line in
`AGENTS.md` as literal text and never open the file. Progressive disclosure built on `@path`
therefore reaches Claude only.

In a shared `AGENTS.md`:

- Keep it self-contained, or reference other docs as ordinary markdown links
  (`see [testing](docs/testing.md)`), which every agent can follow when it needs to.
- Reserve `@path` for the Claude-only section **below** the `@AGENTS.md` import, where being
  Claude-specific is the point.

A symlinked bridge leaves nowhere Claude-only, so any `@path` you add lands in the shared file.
That is the strongest argument for the import bridge on a repo with reference docs.

## Path-scoped context, uniformly

Two mechanisms load context only when it is relevant:

| Mechanism                                         | Granularity        | Read by                          | Committable            |
| ------------------------------------------------- | ------------------ | -------------------------------- | ---------------------- |
| `.claude/rules/` with `paths:`                    | glob               | Claude only                      | needs gitignore surgery |
| Nested `AGENTS.md` + sibling `CLAUDE.md` symlink  | directory          | Claude, Cursor, Codex, Copilot   | yes, like any file      |

Prefer nested `AGENTS.md` when more than one agent works the repo. Claude loads a `CLAUDE.md` in a
subdirectory on demand, when it reads files in that directory — the same lazy behaviour as a
`paths:` rule, and every other agent picks up the nested `AGENTS.md` too:

```text
src/api/AGENTS.md
src/api/CLAUDE.md -> AGENTS.md
```

The Windows and `core.symlinks` caveats apply to nested links as well; on a mixed team make each
nested `CLAUDE.md` a one-line `@AGENTS.md` import instead.

Keep `.claude/rules/` for genuinely Claude-only mechanics (plan mode, hooks, tool permissions) or
when you need glob precision a directory boundary cannot express.

## Generating into an AGENTS.md repo

The templates in this skill are unchanged — the content is the same either way. What changes is
where it lands:

1. Write the drafted content to `AGENTS.md`, not `CLAUDE.md`.
2. Create the bridge, and un-ignore it if needed.
3. Skip the `<!-- CLAUDE.md Last Updated -->` comment or relabel it: Claude Code strips block HTML
   comments before loading, other agents may not.
4. Apply this skill's size target to `AGENTS.md` — under 60 lines, not the 200 the Claude Code docs
   allow. It is the file that loads every session.

To invert an existing CLAUDE.md-only repo: `git mv CLAUDE.md AGENTS.md`, then bridge back.

## `/init` and `/doctor` write to CLAUDE.md

Both built-in commands edit `CLAUDE.md` directly. Against a symlinked bridge they either write
through into `AGENTS.md` or replace the link with a regular file, re-creating the divergence. Run
them before establishing the bridge, or move their output into `AGENTS.md` by hand afterwards and
re-create the link.

## Audit checklist

1. `AGENTS.md` exists, no `CLAUDE.md` → Claude Code loads none of this repo's rules. One-line fix.
2. Both exist as real files → diff them, fold into `AGENTS.md`, replace `CLAUDE.md` with a bridge.
3. `CLAUDE.md` only, other agents in use → invert: rename to `AGENTS.md`, bridge back.
4. Bridge exists but is gitignored → it exists for one machine. Fix the ignore rule, not the file.
5. `@path` imports inside `AGENTS.md` → only Claude follows them. Convert to markdown links.
6. `AGENTS.md` over ~60 lines → split by directory into nested `AGENTS.md`, not more root content.

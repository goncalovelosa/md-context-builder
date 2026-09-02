# One Source of Truth: AGENTS.md + CLAUDE.md

Claude Code reads `CLAUDE.md`. It does **not** read `AGENTS.md`. Cursor, Codex and Copilot read
`AGENTS.md`. If a repo has both as real files, they diverge — this is the single most common
context-file defect in multi-agent repos.

**Rule: `AGENTS.md` is the source. `CLAUDE.md` is a pointer to it. Never two real files.**

## Pick the bridge

| | Bridge | Use when |
| --- | --- | --- |
| **A. Symlink** | `ln -s AGENTS.md CLAUDE.md` | No Claude-only content needed. Zero duplication, zero drift, nothing to maintain. |
| **B. Import** | `CLAUDE.md` = `@AGENTS.md` + a Claude-only section | You need Claude-only rules (plan mode, hooks, skills), or the team has Windows. |

Windows symlinks need Administrator or Developer Mode — use B there.

Do **not** use `/import` (Claude Code v2.1.213+) for this: it appends a **one-time copy** of
`AGENTS.md` into `CLAUDE.md`. That is exactly the divergence you are trying to prevent. It is a
migration tool, not a bridge.

Verify with `/context` — `CLAUDE.md` must appear under **Memory files**.

## 🔴 The trap: `CLAUDE.md` is often gitignored

Many developers put `CLAUDE.md` and `.claude/` in their **global** gitignore (`core.excludesFile`)
to keep agent config personal. The bridge then works on their machine and **exists for nobody
else** — not for teammates, not for CI agents, and not in git worktrees, which are fresh checkouts.

Always check before writing the bridge:

    git check-ignore -v --no-index CLAUDE.md .claude/rules/x.md

Repo-level `.gitignore` beats the global file, so re-include explicitly:

    !CLAUDE.md

Committed symlinks are stored as git mode `120000` (the path, ~9 bytes), not a copy, and worktrees
materialise them correctly. Verify with `git ls-files -s CLAUDE.md`.

To also commit `.claude/rules/` without dragging in personal settings:

    !.claude/
    .claude/*
    !.claude/rules/

## Path-scoped context, uniformly

Two mechanisms load context only when it is relevant:

| | `.claude/rules/` with `paths:` | Nested `AGENTS.md` + sibling `CLAUDE.md` symlink |
| --- | --- | --- |
| Granularity | glob (`src/**/*.tsx`) | directory |
| Read by | Claude only | Claude, Codex, Cursor, Copilot |
| Committable | needs `.gitignore` surgery | yes, like any file |

**Prefer nested `AGENTS.md` when more than one agent works the repo.** Claude loads a `CLAUDE.md`
in a subdirectory on demand, when it reads files in that directory — the same lazy behaviour as a
`paths:` rule, and every other agent gets it too:

    src/api/AGENTS.md
    src/api/CLAUDE.md -> AGENTS.md

Keep `.claude/rules/` for genuinely Claude-only mechanics (plan mode, hooks, tool permissions) or
when you need glob precision a directory boundary can't express.

## Audit checklist

1. `AGENTS.md` exists but no `CLAUDE.md` → Claude Code loads **nothing**. Highest-value one-line fix.
2. Both exist as real files → diff them; fold into `AGENTS.md`, replace `CLAUDE.md` with a bridge.
3. `CLAUDE.md` exists, `AGENTS.md` doesn't, other agents are in use → invert: rename to `AGENTS.md`, bridge back.
4. Bridge exists but is gitignored → it exists for one machine only. Fix the ignore, not the file.
5. `AGENTS.md` over ~200 lines → split by directory into nested `AGENTS.md`, not into more root content.

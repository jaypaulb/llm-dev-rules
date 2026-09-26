# AGENTS.md — llm-dev-rules

> READ FIRST. Context map for agents reviewing or navigating this repo.
> A deeper, Claude-specific version lives in `CLAUDE.md` — this file is the
> concise reviewer-facing entry point and coexists with it deliberately
> (the review fleet keys on AGENTS.md).

## What this repo is

A **documentation / standards template repository** — no executable application
code. It packages development standards and AI-agent rules to be copied or
`git subtree`'d into other projects. Contents are Markdown (`.md`), Cursor rule
files (`.mdc`), and a few YAML configs. There is nothing to build, run, or test
here; "correctness" for a reviewer means: internally consistent guidance, no
dangerous/insecure advice, and no stale instructions that point at things that
do not exist.

## Layout — what is LIVE vs LEGACY/SCRATCH

Reviewers: judge the LIVE trees. Do not report the legacy/scratch trees as if
they were canonical — they are retained history and known to be superseded.

| Path | Status | Owns |
|---|---|---|
| `.cursor/rules/` | LIVE | Cursor IDE rule set (37 `.mdc` files: `core/`, `languages/`, `workflows/`, `project/`, `examples/`) |
| `.claude/` | LIVE | Claude Code Agent OS config: `agents/` (8), `commands/` (7), `skills/` (15 `SKILL.md`) |
| `agent-os/profiles/default/` | LIVE | Agent OS profile: `standards/` (41 `.md`), `workflows/` (4 `.yml`), configs |
| `scripts/` | LIVE | `deploy-llm-rules.sh`, `rollback-llm-rules.sh` (the only shell scripts) |
| `README.md`, `CLAUDE.md`, `PRD.md` | LIVE | Top-level docs |
| `.cursor/rules/dross/` | SCRATCH | Old conversion/migration reports, superseded |
| `dross/` | SCRATCH | Conversion logs and profile-structure notes, superseded |
| `rules-old/` | LEGACY | Numbered pre-refactor rule set (`1.1-core.mdc` …), superseded by `.cursor/rules/` |
| `CHAT_CONTEXT.md`, `PROFILE_SYSTEM_COMPLETE.md` | SCRATCH | Session-recovery / status snapshots, not authoritative |

## Known documentation drift (verify counts before trusting prose)

The narrative docs carry stale numbers. Actual on-disk counts (verified):

- **Skills**: 15 `SKILL.md` files. README says "16", `CLAUDE.md` says both
  "16" and "18" in different sections — all three disagree with disk.
- **Cursor core rules**: `core/` holds **12** `.mdc` files, but README/CLAUDE
  describe a "7-file, ~728-token" always-apply core. The `~728 tokens` budget
  claim is asserted, not measured here.
- **Standards**: 41 `.md` files on disk; docs say "49".
- **Missing referenced files**: `CLAUDE.md` and README reference `TASKS.md` and
  a root `ATOMIC-DESIGN-PLAN.md` — neither exists at root (`ATOMIC-DESIGN-PLAN.md`
  is under `dross/`).

These are candidate review findings (stale-instruction / internal-contradiction
class), not blockers to navigation.

## Rule file convention

Each `.mdc` / standard file uses YAML frontmatter (`description`, `globs`,
`alwaysApply`) followed by actionable Markdown. When reviewing, check that
`globs`/`alwaysApply` claims match how README's "Rule Selection Guide" says the
file is applied, and that security guidance (input validation, secret handling,
injection prevention) is sound and not contradicted elsewhere.

## Guardrails for anyone editing here

- This is a **template consumed by other repos** — changing a rule's stated
  contract (globs, alwaysApply, naming/quality-gate mandates) ripples into every
  downstream project that subtree-pulled it. Treat contract changes as
  higher-stakes than the docs-only surface suggests.
- Keep LIVE and LEGACY/SCRATCH separate; do not resurrect `rules-old/` or
  `dross/` content into the live trees.

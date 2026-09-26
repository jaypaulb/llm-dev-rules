# AGENTS.md — CursorRules

**READ THIS FIRST** before reviewing or editing anything in this repo.

## What this repo is

A **content repository** of reusable Cursor Rules — modular development standards
(`.mdc` files) plus supporting Markdown docs. There is **no executable code, no
build, no tests**. The "product" is the prose guidance itself: rules that Cursor
IDE / AI agents load to shape how they write code in a consumer project.

Reviewers should evaluate this repo as **documentation**: internal
contradictions, dangerous or wrong guidance, and staleness — not compile errors.

## Layout

```
.cursor/rules/            ← ACTIVE rule set (the thing that ships)
├── core/                 always-apply rules: security, quality-gates,
│                         file-operations, command-execution, error-handling,
│                         naming-conventions, tdd-methodology, code-structure,
│                         project-structure, dev-env, documentation, performance
├── workflows/            local-agent, background-agent, code-review, ci-cd
│   └── specialized/      refactor, refresh, reflect, research
├── languages/            per-language: python, golang, rust, cpp, typescript,
│                         mern, flutter, canvus, esp32, rest/graphql/websocket-api
├── project/              tasks, issues, project-management
├── examples/             minimal-python, minimal-command-execution
├── templates/            scaffolding templates
├── dross/                META-DOCS about the rules (not rules themselves):
│                         MIGRATION_GUIDE, RULE_SELECTION_GUIDE,
│                         VALIDATION_REPORT, DUPLICATION_CLEANUP, DEPLOYMENT
└── README.md             describes the "atomic" refactored structure

rules-old/                LEGACY pre-refactor rules (numbered 1.1-core.mdc etc.)
                          Kept for reference; superseded by .cursor/rules/.
                          Expect duplication/divergence vs the active set.

Root docs: README.md, PRD.md, TASKS.md, ATOMIC-DESIGN-PLAN.md,
           master-ide-rule.md
```

## Rule file format

Each `.mdc` is Markdown with YAML frontmatter:

```
---
description: <what the rule covers>
globs: *.py,*.js        # file patterns that trigger it (optional)
alwaysApply: true/false # load regardless of file type
---
# Rule body...
```

Two loading modes: `alwaysApply: true` (core + workflow rules, always in
context) vs. glob-scoped (language/project rules, load when matching files are
edited). Design goal stated in README: keep total loaded context under ~1k
tokens per call.

## Known staleness / contradiction signals (verify, flag to reviewers)

These are pre-existing tensions a content reviewer should weigh — not
necessarily defects to fix, but worth surfacing:

- **Root `README.md` references `.cursor/rules-atomic/`** repeatedly as the "new
  atomic structure." That directory **does not exist** — the atomic rules were
  moved into `.cursor/rules/`. Paths and "legacy - see languages/…" notes in the
  root README are stale relative to the actual tree.
- **Root README metadata says Version 1.0.0 / Last Updated January 2024**, but
  files were last modified November 2025. The dating is unreliable.
- **`rules-old/` duplicates concepts in `.cursor/rules/`.** When the same rule
  exists in both places, treat `.cursor/rules/` as authoritative and check for
  contradictory guidance between the two.
- **`dross/` contains meta-reports** (VALIDATION_REPORT, DUPLICATION_CLEANUP)
  that describe an intended end-state; verify claims there still match the tree
  before trusting them.

## What matters in review

- **Internal contradictions**: two rules giving opposing instructions (e.g. an
  always-apply core rule vs. a language rule), or a rule contradicting the README.
- **Dangerous guidance**: shell commands, security advice, or "always do X"
  directives that are unsafe or would harm a consumer project.
- **Staleness**: broken internal path references, dead links, outdated tool/API
  guidance, references to files/dirs that no longer exist.
- **Frontmatter correctness**: valid `globs`, sensible `alwaysApply`, accurate
  `description`.

## Scope / non-goals

Do not treat this as an application. There is nothing to run or index for
behavior — CodeGraph will find essentially no symbols here. Edits should be
prose/guidance changes only.

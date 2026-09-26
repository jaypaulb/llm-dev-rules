# llm-dev-rules

**llm-dev-rules** is a comprehensive development standards repository that provides:

- **Cursor IDE rules** (`.cursor/rules/`) - Rules for Cursor IDE development environment
- **Claude Code configuration** (`.claude/` directory) - Agent OS profile system with agents, commands, and skills
- **Development standards** (`.agent-os/profiles/default/standards/`) - Modular standards for global, backend, frontend, and testing concerns
- **TDD-enforced architecture** using atomic design patterns (atoms → molecules → organisms → pages)
- **Token-optimized rule system** (~728-928 tokens per session, well under 1K limit)

This is a **standards template repository**, not a runtime application. It's designed to be copied or subtree'd into other projects to establish consistent development practices.

## Important Distinction

- **`.cursor/rules/`** - Cursor IDE rules (not used by Claude Code)
- **`.claude/`** - Claude Code configuration (agents, commands, skills for Claude Code)
- **`agent-os/`** - Development standards used by both Cursor and Claude Code systems

## Common Commands

### Rule Management
```bash
# View rule system documentation
cat .cursor/rules/README.md

# Check rule structure and token budget
cat .cursor/rules/README.md | grep -A 10 "Token"

# List all rule files
find .cursor/rules -name "*.mdc" -type f | sort
```

### Documentation Updates
```bash
# View the main README
cat README.md

# View Product Requirements Document
cat PRD.md

```

### Agent OS Configuration
```bash
# View Agent OS version and settings
cat agent-os/config.yml

# View Claude Code agents
ls .claude/agents/agent-os/

# View Claude Code commands
ls .claude/commands/agent-os/

# View Claude Code skills
ls .claude/skills/

# View development standards
ls agent-os/profiles/default/standards/
```

### Validation & Deployment
```bash
# Validate rule structure (check for duplication)
grep -r "^##" .cursor/rules --include="*.mdc" | wc -l

# Count total rule files
find .cursor/rules -name "*.mdc" | wc -l

# Check git subtree status
git log --oneline -5
```

## Architecture Overview

### Directory Structure

```
llm-dev-rules/
├── .cursor/rules/                    # Cursor IDE rule system
│   ├── core/                         # Always-apply rules (7 files, ~728 tokens)
│   │   └── command-execution, file-operations, quality-gates,
│   │       error-handling, naming-conventions, tdd-methodology, code-structure
│   ├── languages/                    # Language-specific rules (12 files)
│   │   └── python, golang, mern, cpp, typescript, rust, flutter, esp32,
│   │       canvus, rest-api, graphql-api, websocket-api
│   ├── workflows/                    # Workflow rules
│   │   ├── local-agent-workflow.mdc  # Human + AI development
│   │   ├── background-agent-workflow.mdc
│   │   ├── ci-cd.mdc
│   │   ├── code-review.mdc
│   │   └── specialized/              # Refactor, refresh, reflect, research
│   └── project/                      # Project-specific rules
│       └── tasks, project-management, issues
│
├── .claude/                          # Claude Code Agent OS profile system
│   ├── agents/agent-os/              # 8 agent definitions (spec-initializer, shaper, writer, verifier, product-planner, tasks-list-creator, implementer, implementation-verifier)
│   ├── commands/agent-os/            # 7 Claude Code commands (plan-product, shape-spec, write-spec, create-tasks, orchestrate-tasks, implement-tasks, improve-skills)
│   └── skills/                       # 15 reusable skills (backend-api, backend-models, backend-migrations, backend-queries, frontend-components, frontend-css, frontend-accessibility, frontend-responsive, testing-test-writing, + 6 global skills)
│
├── agent-os/                         # Agent OS profile system
│   ├── config.yml                    # Agent OS v2.1.1 configuration
│   └── profiles/
│       └── default/
│           ├── profile-config.yml    # Profile definition with inheritance
│           ├── standards/            # Development standards (41 files incl. README + CONVERSION_LOG)
│           │   ├── global/ (19 files)
│           │   ├── backend/ (10 files)
│           │   ├── frontend/ (7 files)
│           │   └── testing/ (3 files)
│           ├── workflows/ (4 files)
│           ├── agents/agents-config.yml
│           └── commands/commands-config.yml
│
├── README.md                         # Main documentation (deployment methods, rule selection)
├── PRD.md                           # Product Requirements Document
└── CLAUDE.md                        # Claude Code guidance (this file)
```

### Core Architectural Principles

#### 1. **Atomic Design Hierarchy**
All code follows this composition pattern (enforced in `code-structure.mdc`):

```
Atoms (< 100 lines) — No imports except stdlib/external
  ↓ imports only
Molecules (< 200 lines) — Compose atoms
  ↓ imports atoms
Organisms (< 500 lines) — Compose molecules/atoms
  ↓ imports molecules/atoms
Pages (< 500 lines) — Compose organisms/molecules/atoms
  ↓ imports organisms/molecules/atoms
```

Benefits:
- Small, independently testable components
- Clear composition rules prevent tangled dependencies
- Easy to reason about and refactor
- Scales well as projects grow

#### 2. **Test-Driven Development (TDD)**
Enforced via `tdd-methodology.mdc`. All projects must follow:

1. **Red**: Write failing test first
2. **Green**: Write minimal code to pass test
3. **Refactor**: Improve code while keeping tests green

Requirements:
- Test coverage >80% (>100% for critical paths/public APIs)
- Tests mirror source code structure (atoms → molecules → organisms → pages)
- All quality gates must pass before any code is merged

#### 3. **Modular Rule System**
Token-optimized structure with two rule types:

**Always-Apply Rules** (~728 tokens):
- Core rules (7 files): command-execution, file-operations, quality-gates, error-handling, naming-conventions, tdd-methodology, code-structure
- Workflow rules (1 file): local-agent-workflow

These minimal, universal rules are applied to every interaction.

**Context-Specific Rules** (~120-200 tokens, load only when needed):
- Language rules: Applied when file matches language glob (e.g., `*.py` for Python)
- Project rules: Applied when file matches project glob (e.g., `TASKS.md`)
- Workflow rules: Applied when context matches (e.g., background-agent-workflow)

Total per session: **~728-928 tokens** (validated to be under 1K limit).

#### 4. **Claude Code Agent OS Profile System**
Located in `.claude/` and `agent-os/profiles/default/`, enables autonomous feature development:

- **Spec-driven workflow**: Specs → Tasks → Implementation → Verification
- **8-agent pipeline** (`.claude/agents/agent-os/`):
  1. `spec-initializer`: Initialize specification from raw idea
  2. `spec-shaper`: Gather detailed requirements through targeted questions
  3. `spec-writer`: Create detailed specification document
  4. `spec-verifier`: Verify spec completeness before implementation
  5. `product-planner`: Create product documentation and roadmap
  6. `tasks-list-creator`: Break spec into implementation tasks
  7. `implementer`: Execute tasks following the spec
  8. `implementation-verifier`: Verify end-to-end implementation

- **7 Claude Code commands** (`.claude/commands/agent-os/`) for workflow automation
- **15 reusable skills** (`.claude/skills/`) mapped to development concerns (backend-models, frontend-accessibility, global-validation, etc.)
- **41 development standards files** (`.agent-os/profiles/default/standards/`) organized by domain

### Key Standards

#### Naming Conventions (enforced in `naming-conventions.mdc`)
- **Variables/functions**: `lowercase_with_underscores` (Python/Go) or `camelCase` (JS/TS)
- **Classes**: `PascalCase`
- **Constants**: `UPPER_CASE`
- **Files**: lowercase with hyphens for multi-word names (e.g., `user-service.py`)
- **Test files**: Mirror source structure with `test_` prefix or `_test` suffix

#### Quality Gates (enforced in `quality-gates.mdc`)
All projects must pass:
- ✅ Tests pass (TDD mandatory, >80% coverage)
- ✅ Type checking passes (if applicable)
- ✅ Linting passes
- ✅ No secrets in code
- ✅ Security scans pass (OWASP Top 10 compliance)

#### Security Standards (enforced in rules)
- Input validation (allowlists, not blocklists)
- SQL injection prevention
- XSS prevention
- CSRF protection
- No secrets in code
- Secure error handling (no stack traces to users)

### Deployment Methods for New Projects

#### Method 1: GitHub Template (Recommended for New Projects)
```bash
# Use "Use this template" on GitHub
git clone <your-new-repo>
cp -r llm-dev-rules/.cursor <your-project>/
```

#### Method 2: Git Subtree (Recommended for Rule Updates)
```bash
# One-time setup
git subtree add --prefix=.cursor https://github.com/jaypaulb/llm-dev-rules.git main --squash

# Update rules when new versions available
git subtree pull --prefix=.cursor https://github.com/jaypaulb/llm-dev-rules.git main --squash
```

#### Method 3: Manual Clone (Simple Setup)
```bash
git clone https://github.com/jaypaulb/llm-dev-rules.git
cp -r llm-dev-rules/.cursor .
rm -rf llm-dev-rules
```

## Language-Specific Patterns

### Python Projects
- **Linting**: `ruff check .`
- **Formatting**: `black --check .`
- **Type checking**: `mypy .`
- **Testing**: `pytest --cov` (>80% coverage required)
- **Rule**: `.cursor/rules/languages/python.mdc`

### Go Projects
- **Formatting**: `go fmt ./...`
- **Static analysis**: `go vet ./...`
- **Testing**: `go test ./... -cover`
- **Rule**: `.cursor/rules/languages/golang.mdc`

### MERN/JavaScript Projects
- **Linting**: `npm run lint` (ESLint)
- **Formatting**: `npm run format` (Prettier)
- **Testing**: `npm test` (Jest, >80% coverage)
- **Rule**: `.cursor/rules/languages/mern.mdc`

### Other Languages
- **C++**: `.cursor/rules/languages/cpp.mdc`
- **TypeScript**: `.cursor/rules/languages/typescript.mdc`
- **Rust**: `.cursor/rules/languages/rust.mdc`
- **Flutter/Dart**: `.cursor/rules/languages/flutter.mdc`
- **ESP32**: `.cursor/rules/languages/esp32.mdc`

## Development Workflows

### Local Agent Workflow (Human + AI)
Use when working interactively with Claude Code:

1. **Plan**: Break down task into steps
2. **Research**: Explore codebase to understand context
3. **Implement**: Make changes following quality gates
4. **Test**: Run tests and validate quality gates
5. **Review**: Self-review and escalate if needed

Rule: `.cursor/rules/workflows/local-agent-workflow.mdc`

### Background Agent Workflow
Use for autonomous AI agents working through task queues:

1. **Auto-select**: Pick next task from queue
2. **Self-coordinate**: Ensure no conflicts with other agents
3. **Auto-recover**: Implement retry logic with exponential backoff
4. **Self-heal**: Rollback on failure and report issues

Rule: `.cursor/rules/workflows/background-agent-workflow.mdc`

### Specialized Workflows
- **Refactor**: Large-scale code refactoring (`.cursor/rules/workflows/specialized/refactor.mdc`)
- **Refresh**: Bug hunting and problem-solving (`.cursor/rules/workflows/specialized/refresh.mdc`)
- **Reflect**: Post-development analysis (`.cursor/rules/workflows/specialized/reflect.mdc`)
- **Research**: Project inception and research (`.cursor/rules/workflows/specialized/research.mdc`)

## Important Files & Conventions

### README.md
Main project documentation. Update with:
- Quick-start guide for new developers
- Deployment method selection guide
- Rule selection matrix for different project types

### PRD.md
Product Requirements Document. Describes:
- Project goals and vision
- Feature requirements
- Success criteria

### agent-os/config.yml
Agent OS configuration (v2.1.1):
- Compilation timestamp
- Profile settings
- Enabled features (Claude Code commands, subagents, skills)

### agent-os/profiles/default/
The default Agent OS profile containing:
- Profile configuration with inheritance support
- 41 development standards files organized by domain
- 4 workflows for different development scenarios
- Agent and command configurations

## Common Patterns

### Adding New Rules
1. Create file in appropriate directory (`.cursor/rules/core/`, `languages/`, `workflows/`, etc.)
2. Add YAML frontmatter with `description`, `globs`, `alwaysApply`
3. Keep to ~25-200 lines (depends on rule type)
4. Use references to core rules instead of duplication
5. Add example commands or code snippets
6. Update `.cursor/rules/README.md`

Example frontmatter:
```markdown
---
description: Brief description of what this rule covers
globs: *.py,*.pyi
alwaysApply: false
---
# Rule Title
```

### Using Claude Code Commands
Located in `.claude/commands/agent-os/`:
```bash
/plan-product          # Start with product planning
/shape-spec            # Gather requirements
/write-spec            # Create detailed spec
/create-tasks          # Generate implementation tasks
/orchestrate-tasks     # Run multi-agent implementation
/implement-tasks       # Execute tasks
/improve-skills        # Enhance skill definitions
```

### Using Claude Code Subagents
Located in `.claude/agents/agent-os/`:
- General-purpose agents for complex tasks
- Specialized agents for specific domains
- Orchestrated via multi-agent commands

### Using Claude Code Skills
Located in `.claude/skills/`:
- 15 reusable skills for common development concerns
- Automatically applied based on file patterns and context
- Examples: backend-models, frontend-accessibility, global-validation

## Quick Reference

| Task | Location | Command/Tool |
|------|----------|--------------|
| View rules | `.cursor/rules/` | `find .cursor/rules -name "*.mdc"` |
| Check token budget | `.cursor/rules/README.md` | Search for "Token Optimization" |
| Run quality gates | Any project | Depends on language (pytest, npm test, go test) |
| View standards | `agent-os/standards/` | Read global/, backend/, frontend/ |
| Create new feature | `.claude/commands/` | Run `/plan-product` or `/shape-spec` |
| Check version | `agent-os/config.yml` | View Agent OS version |
| View all agents | `.claude/agents/agent-os/` | List all `*.md` files |
| View all skills | `.claude/skills/` | List all `SKILL.md` files |

## Notes for Future Work

- **Rule system is token-optimized**: Always-apply rules are minimal (~728 tokens), context-specific rules load on demand (~120-200 tokens). Total per session stays under 1K tokens.
- **TDD is non-negotiable**: All projects must follow Red-Green-Refactor cycle with >80% test coverage.
- **Atomic design is structural**: Code organization follows atoms → molecules → organisms → pages hierarchy with strict file size limits.
- **Security is built-in**: All rules enforce OWASP Top 10 compliance and input validation.
- **Agent OS is extensible**: New agents, commands, and skills can be added following the established patterns in `.claude/` and `agent-os/profiles/default/`.
- **Cursor and Claude Code are separate**: Cursor IDE uses `.cursor/rules/`, while Claude Code uses `.claude/` configuration and `agent-os/` standards.

---

**Last Updated**: 2025-11-19
**Agent OS Version**: 2.1.1
**Repository**: https://github.com/jaypaulb/llm-dev-rules

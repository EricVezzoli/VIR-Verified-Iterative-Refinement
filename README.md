# VIR — Verified Iterative Refinement

A documentation-first, verification-centric methodology for building complex software systems with AI coding agents. VIR provides a structured framework for managing requirements, implementation plans, bug tracking, and operational documentation — optimized for projects where correctness is critical.

## Why VIR?

Modern AI coding agents (Claude Code, Cursor, Copilot Workspace, etc.) are powerful but need structured context to produce reliable results. VIR solves this by:

1. **Establishing a single source of truth** — a root PRD that AI agents can reference for authoritative decisions
2. **Embedding verification into every step** — acceptance tests live alongside implementation plans, not in separate files
3. **Creating institutional memory** — bugs, warnings, and issues tell complete stories that prevent repeat failures
4. **Enabling precise cross-references** — file paths, function names, and section numbers eliminate ambiguity

## Core Principles

### P1: Document Authority Hierarchy

All truth flows from a **root document** (typically a PRD). Extension documents are subordinate and must not contradict the root. On any conflict, the root wins.

```
Root PRD (axiomatic source of truth)
  ├── Extension Specs (extend but never override root)
  ├── Implementation Plans (derived from specs, serial execution)
  └── Operational Docs (bugs, issues, warnings, tests, deployment)
```

### P2: Status-Driven Tracking

Every documented problem (bug, issue, warning) carries an explicit **status** reflecting three dimensions:

- **Scope** — which component or file is affected
- **Timeline** — when it was discovered, resolved, or superseded
- **Confidence** — resolved, pending confirmation, mitigated, superseded, or accepted

### P3: Verification-First Development

Implementation plans include **embedded acceptance tests** in each step. Tests are runnable code (bash/Python), not prose descriptions. Steps are executed serially — each step must verify the previous step's state before proceeding.

### P4: Iterative Cycles with Reconciliation

- Steps are serial, not parallel
- Each step depends on verification of the previous step
- "Do not skip reconciliation" is a core rule
- Completed work is reconciled against specs before moving forward

### P5: Narrative Problem Solving

Every problem tells a complete story: **discovery → diagnosis → fix → validation → integration notes**. This creates institutional memory that prevents repeat failures and helps AI agents understand not just *what* was done, but *why*.

### P6: Precise Cross-References

All cross-links use file paths, line numbers, function names, and section numbers. Generic references ("the code", "somewhere in the handler") are avoided. This enables both human developers and AI agents to locate and verify referenced information.

### P7: Acceptance Tests as Documentation

Tests are runnable code embedded in `.md` files. They serve as both verification and specification. This eliminates the gap between "what the doc says" and "what the code does."

## Document Types

VIR defines several document types, each with a specific purpose and format:

| Type | Purpose | Template |
|------|---------|----------|
| **Root PRD** | Axiomatic source of truth for the system | [`templates/PRD.md`](templates/PRD.md) |
| **Extension Spec** | Detailed specification for a subsystem | [`templates/SPEC.md`](templates/SPEC.md) |
| **Implementation Plan** | Step-by-step build guide with acceptance tests | [`templates/PLAN.md`](templates/PLAN.md) |
| **Bug Tracker** | Production issues with full narratives | [`templates/BUGS.md`](templates/BUGS.md) |
| **Issue Backlog** | Feature gaps and improvements | [`templates/ISSUES.md`](templates/ISSUES.md) |
| **Warnings** | Known limitations and mitigations | [`templates/WARNINGS.md`](templates/WARNINGS.md) |
| **Integration Tests** | Manual test suite with pass/fail criteria | [`templates/TESTS.md`](templates/TESTS.md) |
| **Deployment Runbook** | Production setup and operations guide | [`templates/DEPLOYMENT.md`](templates/DEPLOYMENT.md) |
| **Agent Onboarding** | AI agent context file (CLAUDE.md / AGENTS.md) | [`templates/AGENTS.md`](templates/AGENTS.md) |

## Quick Start

1. Copy the templates you need from `templates/` into your project's `docs/` directory
2. Start with the **Root PRD** — this is your single source of truth
3. Add **Extension Specs** as your system grows in complexity
4. Create **Implementation Plans** before building each major component
5. Track production issues in **BUGS.md**, feature gaps in **ISSUES.md**, and known limitations in **WARNINGS.md**
6. Write an **Agent Onboarding** file (`CLAUDE.md` or equivalent) so AI agents have full project context

## Directory Structure

```
your-project/
├── CLAUDE.md              # Agent onboarding (or AGENTS.md)
├── docs/
│   ├── PRD.md             # Root PRD — axiomatic source of truth
│   ├── BUGS.md            # Bug tracker (VIR format)
│   ├── ISSUES.md          # Issue backlog (VIR format)
│   ├── WARNINGS.md        # Known limitations (VIR format)
│   ├── TESTS.md           # Integration test suite
│   ├── DEPLOYMENT.md      # Production runbook
│   └── spec/              # Extension specifications
│       ├── PRD-auth.md    # Example: auth subsystem spec
│       ├── PRD-api.md     # Example: API layer spec
│       └── strategies/    # Example: per-strategy specs
│           └── TEMPLATE.md
```

## How VIR Works with AI Agents

VIR is designed to give AI coding agents the context they need to make correct decisions:

1. **The agent reads `CLAUDE.md`** (or equivalent) to understand the project structure, conventions, and current state
2. **The root PRD** provides authoritative answers to architectural questions
3. **BUGS.md and WARNINGS.md** prevent the agent from reintroducing known issues
4. **Implementation Plans** guide the agent through complex multi-step tasks with built-in verification
5. **Precise cross-references** let the agent locate exactly the code or documentation being discussed

### Key Rules for AI Agents

- Trust the root PRD over any other document when conflicts arise
- Read BUGS.md and WARNINGS.md before modifying affected components
- Follow Implementation Plans serially — do not skip steps or reconciliation
- Run embedded acceptance tests after completing each step
- Use precise cross-references (file:line, §section) in all documentation updates

## Status Labels

VIR uses consistent status labels across all document types:

| Status | Meaning |
|--------|---------|
| **Open** | Not yet addressed |
| **In Progress** | Work underway |
| **Done / Resolved** | Completed and validated |
| **Resolved (pending confirmation)** | Fixed in code, awaiting production validation |
| **Partially resolved** | Some aspects fixed, others remain |
| **Mitigated** | Impact reduced but root cause not eliminated |
| **Superseded** | No longer applicable due to architectural change |
| **Accepted limitation** | Intentional design choice, documented trade-off |

## Contributing

Contributions welcome. Please follow the VIR methodology when submitting changes — include clear problem statements, root cause analysis, and validation steps.

## License

MIT — see [LICENSE](LICENSE).

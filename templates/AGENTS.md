# CLAUDE.md — Agent Onboarding Guide

> This file provides AI coding agents with the context they need to work effectively on this project. It is the agent's primary reference — the "briefing document" that establishes project conventions, architecture, and current state.

## Project Summary

[2-3 paragraphs covering:]
- What the system does and its current state
- Key architectural decisions
- What is complete, in progress, and not yet started
- Language, framework, and infrastructure choices

**Language:** [e.g., Python 3.13]
**Framework:** [e.g., Flask, FastAPI, etc.]
**Database:** [e.g., PostgreSQL 16]
**CI/CD:** [e.g., GitHub Actions, none yet]

## Repository Layout

```
├── CLAUDE.md                    # Agent onboarding guide (this file)
├── .env / .env_example          # Environment config (not checked in)
├── docs/
│   ├── PRD.md                   # Root spec — axiomatic source of truth (VIR)
│   ├── BUGS.md                  # Bug tracker (VIR)
│   ├── ISSUES.md                # Issue backlog (VIR)
│   ├── WARNINGS.md              # Known limitations (VIR)
│   ├── TESTS.md                 # Integration test suite (VIR)
│   ├── DEPLOYMENT.md            # Production runbook (VIR)
│   └── spec/                    # Extension specifications
│       └── PRD-<subsystem>.md   # Per-subsystem specs
├── src/                         # Application source code
│   ├── __init__.py
│   ├── config.py                # Configuration (PRD §Config)
│   ├── db.py                    # Database layer (PRD §Data)
│   └── ...
├── tests/                       # Test files
└── scripts/                     # Utility scripts
```

## Critical Conventions

[List the non-obvious rules that an AI agent MUST follow. Examples:]

- **`docs/PRD.md` is the authoritative spec.** Sub-documents extend but do not override it.
- **All configuration is centralized in `.env`** — `config.py` is the parser/validator, not the source of truth.
- **Tag convention:** [lowercase/uppercase/enum — whatever your project uses]
- **No [specific thing to avoid]** — [reason why]

## Module Reference

[For each major module, describe:]

**`module.py`** — [One-line description]. [Key classes/functions]. [Important patterns or constraints].

**`other_module.py`** — [One-line description]. [Key methods grouped by purpose].

## Dependencies

```
package-a          # [purpose]
package-b          # [purpose]
```

## Environment Variables

```env
DATABASE_URL=      # PostgreSQL connection string (required)
API_KEY=           # External API key (required for production)
DEBUG=false        # Enable debug mode (optional, default false)
```

## Validation

[How the agent should verify its changes work:]

1. Verify Python syntax: `python3 -c "import <module>"` for each modified file
2. Run integration tests: see `docs/TESTS.md`
3. [Any project-specific validation steps]

## Key Architecture Details

[Import graph, data flow, or other structural information the agent needs:]

```
config.py → (imported by all modules)
db.py → config
service.py → db, config
main.py → service
```

## Current State

[What is working, what is broken, what is in progress. Keep this section updated.]

- **Working:** [list]
- **Not working:** [list with brief explanation]
- **In progress:** [list]
- **Not started:** [list]

## Notes

- [Any gotchas, tips, or context that doesn't fit elsewhere]
- Trust these instructions. Only search the codebase if information here is incomplete or found to be incorrect.

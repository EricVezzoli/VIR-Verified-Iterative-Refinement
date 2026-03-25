# VIR Methodology — Complete Guide

## What is VIR?

**Verified Iterative Refinement (VIR)** is a documentation-first methodology for building complex software systems. It was designed for projects where:

- **Correctness is critical** — financial systems, infrastructure, safety-critical code
- **AI coding agents are part of the workflow** — agents need structured context to produce reliable results
- **The system evolves iteratively** — requirements change, bugs are discovered, architecture shifts

VIR is not a project management framework (like Agile or Scrum). It is a **documentation and verification discipline** that sits underneath whatever project management approach you use.

## The Three Pillars

### 1. Document Authority

Every VIR project has a **document hierarchy** with a single root document (the PRD) that is the axiomatic source of truth:

```
Root PRD
  │
  ├── Extension Specs (extend but never override root)
  │     ├── PRD-auth.md
  │     ├── PRD-api.md
  │     └── PRD-database.md
  │
  ├── Implementation Plans (derived from specs)
  │     ├── plan-auth.md
  │     └── plan-api.md
  │
  └── Operational Docs (track reality vs spec)
        ├── BUGS.md
        ├── ISSUES.md
        ├── WARNINGS.md
        ├── TESTS.md
        └── DEPLOYMENT.md
```

**Rules:**
- The root PRD is always right. If a sub-document contradicts it, the root wins.
- Extension specs must explicitly state which PRD section they extend.
- Plans must reference the spec they are derived from.
- Operational docs reference specific PRD sections when relevant.

### 2. Verification at Every Step

VIR embeds verification into the development process itself:

- **Implementation Plans** include runnable acceptance tests in every step
- **Steps are executed serially** — you must verify Step N before starting Step N+1
- **Reconciliation is mandatory** — after completing all steps, compare deliverables against the spec
- **Bugs and Warnings** include reproduction steps and validation commands

This is not "write tests after the fact." The tests are written *as part of the plan*, before any code is written.

### 3. Narrative Problem Tracking

Every bug, issue, and warning tells a **complete story**:

```
Discovery → Diagnosis → Root Cause → Fix → Validation → Integration Notes
```

This serves two purposes:
1. **Prevents repeat failures** — anyone (human or AI) can understand *why* something was fixed, not just *what* was changed
2. **Creates institutional memory** — the project's history of decisions and mistakes informs future work

## The Development Loop

### Phase 1: Specify

1. Write or update the **root PRD** with the system's requirements
2. Create **extension specs** for complex subsystems
3. Define **success criteria** — measurable outcomes that determine when the work is done

### Phase 2: Plan

1. Derive an **implementation plan** from the spec
2. Break work into **serial steps** — each with a clear goal, deliverables, and acceptance test
3. Identify **reuse opportunities** from existing code
4. Include **reconciliation checkpoints** at the end

### Phase 3: Build

1. Execute steps **serially** — do not skip ahead
2. Run each step's **acceptance test** before proceeding
3. If a test fails, fix the issue before moving to the next step
4. If the plan needs to change, update the plan document first

### Phase 4: Reconcile

1. Compare all deliverables against the spec
2. Run the full test suite
3. Record **success criteria results** (PASS / FAIL with notes)
4. Update the agent onboarding file (CLAUDE.md) with new module documentation
5. File any discovered issues in ISSUES.md

### Phase 5: Operate

1. Track production bugs in **BUGS.md** with full narratives
2. Track known limitations in **WARNINGS.md** with mitigation status
3. Track feature gaps in **ISSUES.md** with proposed fixes
4. Update the root PRD when the system's behavior diverges from the spec

## Working with AI Agents

VIR is particularly effective when working with AI coding agents. Here's how:

### The Agent Onboarding File

Every VIR project has an agent onboarding file (typically `CLAUDE.md`) that gives the agent:

- **Project summary** — what the system does and its current state
- **Repository layout** — where everything is
- **Critical conventions** — non-obvious rules the agent must follow
- **Module reference** — what each file does
- **Validation steps** — how to verify changes work

This file is the agent's "briefing document." It should be comprehensive enough that the agent can work effectively without reading every file in the repository.

### How Agents Use VIR Documents

| Agent Task | VIR Document | How It Helps |
|------------|--------------|--------------|
| Understanding the system | CLAUDE.md + PRD | Provides authoritative architecture and conventions |
| Implementing a feature | Plan + Spec | Step-by-step guide with embedded tests |
| Fixing a bug | BUGS.md + WARNINGS.md | Prevents reintroducing known issues |
| Adding a new component | PRD §Extension Points + SPEC template | Consistent patterns for new code |
| Deploying changes | DEPLOYMENT.md | Exact commands and verification steps |
| Validating changes | TESTS.md | Runnable test commands with expected output |

### Key Rules for Agents

1. **Trust the root PRD** over any other document when conflicts arise
2. **Read BUGS.md and WARNINGS.md** before modifying affected components
3. **Follow Plans serially** — do not skip steps or reconciliation
4. **Run acceptance tests** after completing each step
5. **Use precise cross-references** (file:line, §section) in all documentation updates
6. **Update CLAUDE.md** when adding new modules or changing conventions

## Status Labels

VIR uses consistent status labels across all document types:

| Status | Meaning | Used In |
|--------|---------|---------|
| **Open** | Not yet addressed | Issues |
| **In Progress** | Work underway | Issues, Plans |
| **Done / Resolved** | Completed and validated | Issues, Bugs |
| **Resolved (pending confirmation)** | Fixed in code, awaiting production validation | Bugs |
| **Partially resolved** | Some aspects fixed, others remain | Bugs |
| **Mitigated** | Impact reduced but root cause not eliminated | Warnings |
| **Superseded** | No longer applicable due to architectural change | Bugs, Warnings |
| **Accepted limitation** | Intentional design choice, documented trade-off | Warnings |

## Cross-Reference Conventions

VIR documents use precise cross-references to eliminate ambiguity:

| Reference Type | Format | Example |
|---------------|--------|---------|
| File | `module.py` | `db.py` |
| File + line | `module.py:123` | `db.py:456` |
| Function | `module.py` → `function()` | `db.py` → `get_record()` |
| PRD section | §N | §5 Config |
| Document | `docs/spec/PRD-auth.md` | Full relative path |
| Bug/Issue | B1, I3 | Short ID for inline references |
| Warning | W7 | Short ID for inline references |

## Frequently Asked Questions

### Do I need all the document types?

No. Start with what you need:
- **Minimum:** Root PRD + CLAUDE.md
- **Small project:** Add BUGS.md and ISSUES.md
- **Growing project:** Add Plans, Specs, WARNINGS.md, TESTS.md
- **Production system:** Add DEPLOYMENT.md

### How do I keep documents in sync with code?

1. Update CLAUDE.md whenever you add or change a module
2. When fixing a bug, update BUGS.md with the resolution
3. After major changes, reconcile the PRD against the actual implementation
4. Use the reconciliation phase to catch drift

### Can I use VIR with automated tests?

Absolutely. VIR's embedded acceptance tests in Plans complement (not replace) automated test suites. The embedded tests are for step-by-step verification during development; automated tests are for ongoing regression protection.

### How is VIR different from just writing good docs?

VIR adds three things to "just writing docs":
1. **Authority hierarchy** — clear rules for which document wins on conflicts
2. **Embedded verification** — tests live in the documentation, not separate files
3. **Narrative problem tracking** — bugs tell complete stories, not just symptoms and fixes

### Can multiple people use VIR on the same project?

Yes. The root PRD provides a shared source of truth. Extension specs let different people own different subsystems. BUGS.md and ISSUES.md provide a shared view of known problems.

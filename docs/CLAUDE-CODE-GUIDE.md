# Using VIR with Claude Code

This guide explains how to set up and use the VIR methodology with [Claude Code](https://docs.anthropic.com/en/docs/claude-code), Anthropic's CLI agent for software engineering.

---

## Why VIR + Claude Code?

Claude Code reads your project files, executes commands, and writes code autonomously. But without structured context, it makes assumptions — sometimes wrong ones. VIR solves this by giving Claude Code:

1. **A single source of truth** — the root PRD answers "how should this work?" definitively
2. **Institutional memory** — BUGS.md and WARNINGS.md prevent reintroducing known issues across conversations
3. **Step-by-step plans** — multi-step tasks are broken into verifiable steps with acceptance tests
4. **Precise conventions** — CLAUDE.md tells Claude Code exactly how your project works, what to avoid, and how to validate changes

Without VIR, each new Claude Code conversation starts from scratch. With VIR, Claude Code inherits the full history of decisions, mistakes, and conventions from day one.

---

## Setup

### Step 1: Create your CLAUDE.md

Claude Code automatically reads `CLAUDE.md` from your project root at the start of every conversation. This is the single most important file for effective Claude Code usage.

Copy `templates/AGENTS.md` to your project root as `CLAUDE.md`:

```bash
cp templates/AGENTS.md ./CLAUDE.md
```

Fill in every section. The more complete your CLAUDE.md, the fewer mistakes Claude Code will make. Key sections:

| Section | Why it matters |
|---------|---------------|
| **Project Summary** | Claude Code needs to understand what the system does before touching any code |
| **Repository Layout** | Prevents Claude Code from creating files in wrong locations or missing existing code |
| **Critical Conventions** | Non-obvious rules that Claude Code will violate without explicit instruction |
| **Module Reference** | Saves Claude Code from reading every file to understand the codebase |
| **Validation** | Tells Claude Code how to verify its own changes work |

### Step 2: Create your Root PRD

```bash
mkdir -p docs
cp templates/PRD.md docs/PRD.md
```

Reference it in your CLAUDE.md:

```markdown
## Critical Conventions

- **`docs/PRD.md` is the authoritative spec.** Sub-documents in `docs/spec/` extend but do not override it.
```

### Step 3: Add operational docs as needed

```bash
cp templates/BUGS.md docs/BUGS.md
cp templates/ISSUES.md docs/ISSUES.md
cp templates/WARNINGS.md docs/WARNINGS.md
```

Reference them in CLAUDE.md so Claude Code knows they exist:

```markdown
## Repository Layout

├── docs/
│   ├── PRD.md             # Root spec — axiomatic source of truth (VIR)
│   ├── BUGS.md            # Bug tracker (VIR)
│   ├── ISSUES.md          # Issue backlog (VIR)
│   └── WARNINGS.md        # Known limitations (VIR)
```

---

## Workflows

### Building a new feature

**1. Write the spec first** — create an extension spec in `docs/spec/`:

```
You: "Read the PRD and create a spec for the authentication subsystem
      in docs/spec/PRD-auth.md. Follow the template in templates/SPEC.md."
```

**2. Create an implementation plan** — derive a step-by-step plan from the spec:

```
You: "Create an implementation plan for the auth subsystem based on
      docs/spec/PRD-auth.md. Follow the template in templates/PLAN.md.
      Each step must have an acceptance test."
```

**3. Execute the plan** — tell Claude Code to follow the plan serially:

```
You: "Execute Step 0 of docs/plan-auth.md. Run the acceptance test
      before moving to Step 1."
```

**4. Reconcile** — after all steps are complete:

```
You: "All steps in docs/plan-auth.md are done. Reconcile: verify all
      acceptance tests pass, compare against docs/spec/PRD-auth.md,
      and update CLAUDE.md with the new module documentation."
```

### Fixing a bug

**1. Claude Code reads BUGS.md** to check for related known issues:

```
You: "Users are getting a 500 error on the /dashboard endpoint.
      Check docs/BUGS.md for related issues, then investigate."
```

**2. After fixing, Claude Code records the bug** with full narrative:

```
You: "Add this bug and the fix to docs/BUGS.md following the VIR
      format — include root cause, solution, and validation steps."
```

### Investigating a problem

When Claude Code encounters unexpected behavior, VIR documents help it understand context:

```
You: "The cache is returning stale data. Check docs/WARNINGS.md
      for known limitations and docs/ISSUES.md for related items
      before proposing a fix."
```

### Updating docs after changes

After any significant change, keep VIR docs in sync:

```
You: "I've merged the auth feature. Update:
      1. CLAUDE.md — add the auth module to the module reference
      2. docs/PRD.md — mark §Auth as implemented
      3. docs/ISSUES.md — close I4 (auth was the blocker)"
```

---

## CLAUDE.md Best Practices

### Keep it comprehensive but current

CLAUDE.md should be a living document. Update it when:
- A new module is added
- A convention changes
- A common mistake keeps happening (add it to Critical Conventions)
- A module's interface changes significantly

### Use the "trust these instructions" pattern

End your CLAUDE.md with:

```markdown
## Notes

- Trust these instructions. Only search the codebase if information
  here is incomplete or found to be incorrect.
```

This tells Claude Code to use CLAUDE.md as its primary reference instead of re-reading the entire codebase each conversation.

### Document what's NOT obvious

Don't waste CLAUDE.md space on things Claude Code can figure out from the code. Focus on:

- **Why decisions were made** — "We use GTD orders instead of GTC because..."
- **What to avoid** — "Do NOT rename this file — imports reference the exact name"
- **Non-obvious dependencies** — "Module A must be initialized before Module B"
- **Validation steps** — "After any change to X, run Y to verify"

### Include import/dependency graphs

Claude Code benefits enormously from seeing how modules relate:

```markdown
## Key Architecture Details

config.py → (imported by all modules)
db.py → config
service.py → db, config, external_client
main.py → service
```

### Reference VIR docs explicitly

Tell Claude Code where to find operational context:

```markdown
## Critical Conventions

- Check `docs/BUGS.md` before modifying error handling — several
  edge cases have been discovered and fixed in production.
- Check `docs/WARNINGS.md` for known limitations that affect
  the component you're working on.
- Follow plans in `docs/plan-*.md` serially per VIR methodology.
```

---

## Working with Plans in Claude Code

### Entering Plan Mode

Claude Code has a built-in Plan Mode (Shift+Tab to toggle). VIR plans complement this:

- **Claude Code's Plan Mode** — lightweight, conversation-scoped, for immediate task planning
- **VIR Plans** — persistent `.md` files, cross-referenced to specs, with embedded acceptance tests

For complex multi-step work, use VIR plans. For quick tasks, Claude Code's built-in planning is sufficient.

### Serial execution matters

VIR plans are designed for serial execution. When working with Claude Code:

```
You: "Execute docs/plan-api.md Step 3. Do NOT skip to Step 4 until
      the Step 3 acceptance test passes."
```

This prevents Claude Code from optimistically jumping ahead and building on unverified foundations.

### Acceptance tests as checkpoints

Each plan step's acceptance test serves as a gate:

```
You: "Run the acceptance test for Step 2. If it passes, proceed to
      Step 3. If it fails, diagnose and fix before continuing."
```

---

## Multi-Conversation Continuity

One of VIR's biggest benefits with Claude Code is **continuity across conversations**. Each Claude Code conversation starts fresh — but VIR documents persist:

| What persists | Where | Claude Code reads it |
|--------------|-------|---------------------|
| Architecture & conventions | `CLAUDE.md` | Automatically at conversation start |
| System requirements | `docs/PRD.md` | When referenced or asked |
| Known bugs | `docs/BUGS.md` | When modifying affected components |
| Known limitations | `docs/WARNINGS.md` | When modifying affected components |
| Open issues | `docs/ISSUES.md` | When asked about backlog |
| Implementation progress | `docs/plan-*.md` | When continuing multi-step work |

### Resuming work across conversations

```
You: "I'm continuing work on the auth feature. Read docs/plan-auth.md
      to see where we left off. Steps 0-3 are complete. Continue
      from Step 4."
```

Claude Code reads the plan, sees the completed steps and their acceptance tests, and picks up exactly where the previous conversation ended.

### Recording decisions for future conversations

When Claude Code makes a significant decision during a conversation, persist it:

```
You: "Good call on using connection pooling. Add that decision and
      the rationale to docs/PRD.md §Database and update CLAUDE.md."
```

The next conversation will see this decision in CLAUDE.md and won't second-guess it.

---

## Common Pitfalls

### 1. CLAUDE.md gets stale

**Problem:** CLAUDE.md describes the codebase as it was 3 weeks ago. Claude Code follows outdated conventions.

**Fix:** After every significant change, ask Claude Code to update CLAUDE.md:

```
You: "Update CLAUDE.md to reflect the changes we just made."
```

### 2. PRD and code diverge

**Problem:** The PRD says one thing, the code does another. Claude Code doesn't know which to trust.

**Fix:** VIR's authority rule is clear — the PRD wins. But if the code is intentionally different, update the PRD:

```
You: "The code now uses retry logic that isn't in the PRD. Update
      docs/PRD.md §Error Handling to match the implementation."
```

### 3. Bug fixes without BUGS.md entries

**Problem:** A bug is fixed but not recorded. A future conversation reintroduces it.

**Fix:** Always record bugs, even trivial ones:

```
You: "Add this fix to docs/BUGS.md with the root cause so we don't
      hit this again."
```

### 4. Plans too detailed or too vague

**Problem:** Over-specified plans are brittle and need constant revision. Under-specified plans leave too much to interpretation.

**Fix:** Each plan step should have:
- A one-sentence goal (clear outcome)
- Concrete deliverables (what files/artifacts)
- A runnable acceptance test (how to verify)
- No implementation details that constrain Claude Code's approach

---

## Example Project Structure

Here's what a mature VIR project looks like with Claude Code:

```
my-project/
├── CLAUDE.md                    # Agent onboarding — auto-read by Claude Code
├── docs/
│   ├── PRD.md                   # Root spec (v2.1)
│   ├── BUGS.md                  # 12 resolved, 2 open
│   ├── ISSUES.md                # 8 items (3 open, 5 done)
│   ├── WARNINGS.md              # 6 known limitations
│   ├── TESTS.md                 # 5-step integration suite
│   ├── DEPLOYMENT.md            # Production runbook
│   ├── plan-api-v2.md           # Completed plan (all steps done)
│   ├── plan-notifications.md    # In progress (Step 3 of 7)
│   └── spec/
│       ├── PRD-api.md           # API subsystem spec
│       ├── PRD-auth.md          # Auth subsystem spec
│       └── PRD-notifications.md # Notifications spec
├── src/
│   └── ...
└── tests/
    └── ...
```

Claude Code reads CLAUDE.md automatically, then navigates to the relevant VIR document based on what you ask it to do. The result: consistent, verified, well-documented code — conversation after conversation.

# PLAN: [Component Name]

**Derived from:** `docs/spec/PRD-[subsystem].md` v1.0
**Created:** YYYY-MM-DD
**Status:** Ready for development | In progress (Step N) | Complete

> **VIR Authority:** This plan is derived from `docs/spec/PRD-[subsystem].md`, which extends `docs/PRD.md`. Steps must be executed serially per VIR §Development Loop. Each step includes embedded acceptance tests. Do not skip reconciliation.

---

## Step 0: [Foundation / Scaffold]

**Goal:** [One-sentence outcome — what exists after this step]

**Deliverables:**
- [File or artifact 1]
- [File or artifact 2]
- [File or artifact 3]

**Interface:**

```python
# [Key interface definition for this step]
class Component:
    def __init__(self, config):
        ...
```

**Key reuse from [existing code]:** [What can be migrated or adapted, and what must change]

**Acceptance test:**

```bash
# Run from project root
python3 -c "
from package.module import Component
c = Component()
assert hasattr(c, 'method_name'), 'method_name not found'
print('Step 0: PASS')
"
```

---

## Step 1: [Core Logic]

**Goal:** [One-sentence outcome]

**Deliverables:**
- [File or artifact]

**PRD reference:** §[N] — [brief description of the spec section this implements]

**Implementation notes:**
- [Key decision or constraint]
- [Migration note from existing code]

**Acceptance test:**

```bash
python3 -c "
from package.module import Component

c = Component(test_config)
result = c.process(test_input)
assert result.status == 'success', f'Expected success, got {result.status}'
assert len(result.items) > 0, 'No items returned'
print('Step 1: PASS')
"
```

---

## Step 2: [Next Layer]

**Goal:** [One-sentence outcome]

**Deliverables:**
- [File or artifact]

**Depends on:** Step 1 (requires [specific output from Step 1])

**Acceptance test:**

```bash
python3 -c "
# [Test that validates Step 2 AND verifies Step 1 still works]
print('Step 2: PASS')
"
```

---

## Step N: [Final Integration]

**Goal:** [One-sentence outcome]

**Deliverables:**
- [Final artifacts]

**Acceptance test:**

```bash
# End-to-end validation
python3 -c "
# [Test that exercises the full pipeline from Step 0 through Step N]
print('Step N: PASS — all steps validated')
"
```

---

## Reconciliation

After all steps are complete:

1. **Verify all acceptance tests pass** in sequence
2. **Compare deliverables against PRD spec** — identify any gaps or deviations
3. **Update CLAUDE.md** (or agent onboarding file) with new module documentation
4. **Record any issues discovered** in `docs/ISSUES.md`
5. **Update root PRD** if the implementation revealed spec gaps

### Success Criteria Results

| # | Criterion | Result | Notes |
|---|-----------|--------|-------|
| 1 | [From PRD success criteria] | PASS / FAIL | [Details] |
| 2 | [...] | PASS / FAIL | [...] |

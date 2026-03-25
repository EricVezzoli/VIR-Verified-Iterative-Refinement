# Issues

Feature gaps, improvements, and technical debt. Each issue includes a clear problem statement, proposed fix, and validation plan.

---

## I1: [Short descriptive title]

**Status:** Open | In Progress | Done
**Priority:** Critical | High | Medium | Low
**Component:** `module.py` → `function_name()`

### Problem

[What is missing, broken, or suboptimal. Include concrete examples.]

**Root cause:** [Why this gap exists — oversight, changing requirements, technical constraint, etc.]

### Fix

[Proposed implementation with specific code changes.]

```python
# In module.py, before the existing call (~line N):
new_value = compute_something()
existing_call(new_param=new_value)
```

### Affected Files

| File | Change |
|------|--------|
| `module_a.py` | Add new parameter to `function_x()` |
| `module_b.py` | Update caller to pass new parameter |
| `docs/PRD.md` | Update §N to reflect new behavior |

### Validation

[How to verify the fix works.]

```bash
# Verification command
python3 -c "
# [Test that confirms the issue is resolved]
"
```

### Notes

[Edge cases, backward compatibility, dependencies on other issues]

---

<!--
TEMPLATE — copy this block for new entries:

## IN: [Title]

**Status:** Open
**Priority:** [Critical | High | Medium | Low]
**Component:** `file.py` → `function()`

### Problem

[Description]

### Fix

[Proposed implementation]

### Validation

[How to verify]
-->

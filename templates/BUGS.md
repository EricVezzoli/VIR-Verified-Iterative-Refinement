# Bug Tracker

Issues discovered during development, testing, or production operation. Each entry tells a complete story: discovery → diagnosis → fix → validation.

---

## YYYY-MM-DD: [Short descriptive title]

**Status:** Open | Resolved (YYYY-MM-DD) | Partially resolved | Superseded

**Error:** `[Error message or exception if applicable]`

**Context:** [Where and how the bug was discovered — production, testing, code review, etc.]

**Stack trace:** *(if applicable)*
```
[Relevant stack trace]
```

### Problem

[What went wrong. Be specific — include actual vs expected behavior.]

**Root cause:** [Confirmed technical explanation of why it happened]

**Impact:** [Severity and consequences — data loss, incorrect behavior, crash, UX issue, etc.]

### Solution (implemented YYYY-MM-DD)

[What was changed to fix it. Include file names and function names.]

**Changes:** `module_a.py` (`function_x`), `module_b.py` (`function_y`)

### Validation

[How the fix was verified — manual test, automated test, production observation, etc.]

```bash
# Reproduction / verification command
python3 -c "
# [Test that confirms the bug is fixed]
"
```

### Notes

[Edge cases, related issues, future risks, backward compatibility concerns]

---

<!--
TEMPLATE — copy this block for new entries:

## YYYY-MM-DD: [Title]

**Status:** Open

### Problem

[Description]

**Root cause (suspected):** [Theory]

**Impact:** [Severity]

### Solution

[Not yet implemented]

### Validation

[How to verify the fix]
-->

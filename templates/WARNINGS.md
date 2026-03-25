# Known Warnings

Documented issues that are not runtime-crashing bugs but affect correctness, robustness, or edge-case handling. Ordered by severity.

---

## W1. [Short descriptive title]

**File:** `module.py` — `function_name()`
**Status:** Partially mitigated | Mitigated | Superseded | Accepted limitation

[Description of the issue — what could go wrong and under what conditions.]

**Mitigation applied:** [What was done to reduce the impact, if anything]

**Residual risk:** [What remains even after mitigation]

**Recommended fix:** [Code change or architectural change to fully resolve]

```python
# Example fix:
if value <= 0:
    return default_value
```

---

## W2. [Short descriptive title]

**File:** `module.py` — `function_name()`
**Status:** Accepted limitation

[Description]

**Impact:** [How this affects the system in practice]

**Workaround:** [What users or operators should do to avoid the issue]

---

<!--
TEMPLATE — copy this block for new entries:

## WN. [Title]

**File:** `module.py` — `function_name()`
**Status:** [Partially mitigated | Mitigated | Superseded | Accepted limitation]

[Description]

**Mitigation applied:** [If any]

**Residual risk:** [What remains]

**Recommended fix:** [Proposed code change]
-->

# Integration Tests

Manual integration test suite. Each test includes setup, execution, expected output, and pass/fail criteria.

> **When to run:** Before every deploy, after schema changes, and when modifying core workflows.

---

## Step 1 — [Foundation / Config Validation]

### How to run

```bash
# From project root:
python3 -c "
import os
import sys

# ── Check configuration loads correctly ──
print('=== Config ===')
from package.config import cfg
print(f'  PARAM_A={cfg.PARAM_A}: OK')
print(f'  PARAM_B={cfg.PARAM_B}: OK')

# ── Check schema has all expected tables ──
print()
print('=== Schema ===')
# [Schema validation logic]
print('  All tables present: OK')
"
```

### Expected output

```
=== Config ===
  PARAM_A=value: OK
  PARAM_B=value: OK

=== Schema ===
  All tables present: OK
```

### Pass criteria

- All config parameters load with correct types and defaults
- Schema contains all expected tables and columns

### Common failures

| Symptom | Cause | Fix |
|---------|-------|-----|
| `ImportError` | Missing dependency | `pip install <package>` |
| `RuntimeError: missing VAR` | `.env` not configured | Copy `.env_example` to `.env` and fill in values |

---

## Step 2 — [Database Operations]

### How to run

```bash
python3 -c "
from package.db import insert_record, get_record

# Insert test data
insert_record(name='test', value=42)

# Verify retrieval
record = get_record(name='test')
assert record is not None, 'Record not found'
assert record['value'] == 42, f'Wrong value: {record[\"value\"]}'
print('Step 2: PASS')
"
```

### Pass criteria

- Records insert and retrieve correctly
- Upserts handle conflicts as expected
- Batch operations complete without errors

---

## Step 3 — [Core Workflow End-to-End]

### How to run

```bash
python3 -c "
# [Full end-to-end test of the main workflow]
# This test exercises the complete pipeline
print('Step 3: PASS')
"
```

### Pass criteria

- [Specific measurable outcomes]
- [Data integrity checks]
- [Performance thresholds if applicable]

---

<!--
TEMPLATE — copy this block for new test steps:

## Step N — [Test Name]

### How to run

```bash
python3 -c "
# [Test code]
print('Step N: PASS')
"
```

### Expected output

```
Step N: PASS
```

### Pass criteria

- [Criterion 1]
- [Criterion 2]

### Common failures

| Symptom | Cause | Fix |
|---------|-------|-----|
| [Error] | [Cause] | [Fix] |
-->

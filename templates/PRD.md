# PRD: [Project Name]

**Version:** 1.0
**Date:** YYYY-MM-DD
**Status:** Draft | Approved for Plan Development | Approved for Implementation

> **Document Authority:** This is the root PRD — the axiomatic source of truth for the system per VIR methodology. Sub-documents extend but do not override this file. On any conflict, this document wins.

### Document Map

| Document | Scope |
|----------|-------|
| `docs/PRD.md` (this file) | Architecture, schema, config, core workflows |
| `docs/spec/PRD-<subsystem>.md` | [Subsystem] specification |
| `docs/spec/strategies/TEMPLATE.md` | Template for new component documents |

---

## 1. Overview

[1-3 paragraph summary of what the system does, its key constraints, and its current state.]

---

## 2. Architecture

[High-level architecture diagram (ASCII or Mermaid). Show major components, data flows, and process boundaries.]

```
component_a  →  component_b  →  database
     ↑               ↓
component_c  ←  component_d
```

**Key architectural decisions:**
- [Decision 1 and rationale]
- [Decision 2 and rationale]

---

## 3. Data Model

[Database schema with table definitions, relationships, and indexes. Use actual SQL DDL.]

```sql
CREATE TABLE example (
    id          SERIAL PRIMARY KEY,
    name        TEXT NOT NULL,
    status      TEXT NOT NULL DEFAULT 'active',
    created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

Total: **N tables**.

---

## 4. Configuration

[All configuration variables with defaults, descriptions, and validation rules.]

```env
# ── Database ──────────────────────────────────────────────────────────────────
DATABASE_URL=postgresql://user:pass@host:5432/dbname

# ── Feature Flags ─────────────────────────────────────────────────────────────
FEATURE_X_ENABLED=false          # master switch (default: disabled)

# ── Tuning ────────────────────────────────────────────────────────────────────
MAX_RETRIES=3                    # max retry attempts before giving up
TIMEOUT_SEC=30                   # request timeout in seconds
```

---

## 5. Core Workflow

[Step-by-step description of the main processing loop or request lifecycle.]

### Step 0: Initialization
[What happens on startup]

### Step 1: [First Action]
[What this step does, its inputs, outputs, and error handling]

### Step 2: [Second Action]
[...]

---

## 6. API / Interface Contracts

[Public interfaces, API endpoints, message formats. Include request/response examples.]

---

## 7. Error Handling

[How errors are classified, logged, and recovered from. Include error codes if applicable.]

---

## 8. Success Criteria

1. [Measurable criterion 1]
2. [Measurable criterion 2]
3. [Measurable criterion 3]

---

## 9. Deployment

[How the system is deployed, monitored, and operated. Reference `docs/DEPLOYMENT.md` for full runbook.]

---

## 10. Extension Points

[How new components, strategies, or plugins are added. Reference extension spec template.]

> **Adding a new component:** See `docs/spec/TEMPLATE.md`.

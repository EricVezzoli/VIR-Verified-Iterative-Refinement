# PRD-[Subsystem]: [Subsystem Name] Specification

**Version:** 1.0
**Date:** YYYY-MM-DD
**Extends:** `docs/PRD.md` §[N] — does not override

> **VIR Authority:** This document extends the root PRD. On any conflict with `docs/PRD.md`, the root PRD wins.

---

## 1. Overview

[What this subsystem does and how it fits into the overall architecture. Reference the parent PRD section.]

---

## 2. Interface

[Public API of this subsystem — classes, methods, data structures.]

```python
class ExampleService:
    """[One-line description]"""

    def __init__(self, config: Config, db: Database):
        """[Constructor parameters and their purpose]"""

    def process(self, input: InputType) -> OutputType:
        """[What this method does, its contract, and error conditions]"""
```

---

## 3. Data Flow

[How data moves through this subsystem. Include sequence diagrams or ASCII flows.]

```
Input → Validate → Transform → Persist → Respond
                       ↓
                   Side-effect (log, notify, etc.)
```

---

## 4. State Management

[What state this subsystem owns, how it's persisted, and consistency guarantees.]

---

## 5. Error Handling

[Subsystem-specific error cases and recovery strategies.]

| Error | Cause | Recovery |
|-------|-------|----------|
| [Error 1] | [What triggers it] | [How the system recovers] |

---

## 6. Configuration

[Subsystem-specific configuration parameters. Reference parent PRD §Config for shared params.]

| Parameter | Default | Description |
|-----------|---------|-------------|
| `PARAM_A` | `10` | [What it controls] |

---

## 7. Dependencies

[What this subsystem depends on — other modules, external services, databases.]

---

## 8. Testing

[How to test this subsystem in isolation. Reference `docs/TESTS.md` for integration tests.]

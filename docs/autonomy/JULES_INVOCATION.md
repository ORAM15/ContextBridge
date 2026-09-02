# JULES INVOCATION PROTOCOL

## ContextBridge

**Document Type:** AI Autonomy Policy
**Status:** FINAL

---

# 1. Purpose

This document defines the constraints and protocols for invoking the Jules AI agent within the ContextBridge project.

# 2. Constraints

Jules must strictly adhere to the `AUTONOMOUS_EXECUTION_CONTRACT.md` and the `PROJECT_STATE.md` to ensure safe, evidence-driven, and correct execution of tasks.

# 3. Invocation Steps

1. Read `docs/project-state/PROJECT_STATE.md` to determine the current state.
2. Read `docs/project-state/HANDOFF_RECORD.md` to understand context and previous work.
3. Validate against `docs/autonomy/AUTONOMOUS_EXECUTION_CONTRACT.md`.
4. Execute permitted actions within the bounds of the current checkpoint.
5. Update state documents before completing the task or handing off.

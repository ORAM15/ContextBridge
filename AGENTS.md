# ContextBridge Agent Rules

The ContextBridge project strictly enforces an autonomous engineering contract, project state, and a checkpoint system. You must NOT deviate from these policies.

Before modifying code, you MUST establish the correct state from `docs/project-state/PROJECT_STATE.md` and `docs/project-state/HANDOFF_RECORD.md`.

You MUST read the following foundation documents for detailed execution rules:
- `docs/autonomy/AUTONOMOUS_EXECUTION_CONTRACT.md`
- `docs/autonomy/JULES_INVOCATION.md`
- `docs/project-state/PROJECT_STATE.md`
- `docs/project-state/CHECKPOINT_LOG.md`
- `docs/project-state/DECISION_LOG.md`
- `docs/project-state/VALIDATION_RECORD.md`
- `docs/project-state/HANDOFF_RECORD.md`

All implementation MUST be checked into a dedicated branch and verified against the established validation criteria. You must NEVER merge directly into `main`.

A checkpoint is NOT complete until its specific acceptance criteria in the checkpoint logs are met and verifiable evidence is recorded.

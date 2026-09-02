# FINAL PROJECT STATE AND HANDOFF PROTOCOL

## ContextBridge

**Document Type:** Project State, Continuity & AI Handoff Policy
**Status:** FINAL
**Scope:** All ContextBridge engineering phases, checkpoints, repository activity and AI-assisted sessions

---

# 1. Purpose

The ContextBridge project SHALL maintain a canonical, machine-readable and human-readable project state so that a completely fresh AI session can resume engineering work without relying on hidden conversational memory.

The governing principle is:

> **If a fact is necessary for the next AI agent to safely continue the project, that fact must exist in the project's persistent state.**

Conversation history may provide context.

It SHALL NOT be the authoritative source of project state.

---

# 2. Continuity Objective

A fresh AI agent must be able to determine:

```text
Current phase
Current checkpoint
Completed checkpoints
Active work
Blocked work
Failed attempts
Open decisions
Required approvals
Latest validation
Repository state
Known defects
Next permitted action
```

without asking:

> "What did the previous AI know?"

The repository and its associated engineering records must answer that question.

---

# 3. Canonical State Architecture

ContextBridge uses five formal state records:

```text
PROJECT_STATE
      │
      ├── DECISION_LOG
      │
      ├── CHECKPOINT_LOG
      │
      ├── VALIDATION_RECORD
      │
      └── HANDOFF_RECORD
```

Their responsibilities are different.

```text
PROJECT_STATE
→ Where the project is now

DECISION_LOG
→ Why important decisions were made

CHECKPOINT_LOG
→ What engineering work has been completed/in progress

VALIDATION_RECORD
→ What has actually been proven

HANDOFF_RECORD
→ What the next AI must know and do
```

---

# 4. Source-of-Truth Hierarchy

The following hierarchy SHALL be used:

```text
Approved project documents
        ↓
Repository state + persistent engineering records
        ↓
GitHub Issues / PRs / CI evidence
        ↓
Current working tree
        ↓
Conversation history
```

For implementation state, repository/GitHub records take precedence over conversational claims.

A previous AI saying:

> "This checkpoint is complete"

does not make it complete.

The persistent evidence must support that state.

---

# 5. PROJECT_STATE

`PROJECT_STATE` is the canonical current-state snapshot.

It answers:

> **Where is ContextBridge right now?**

Recommended location:

```text
docs/project-state/PROJECT_STATE.md
```

A machine-readable representation may additionally exist if useful, but the canonical human-readable state must remain understandable without specialized tooling.

---

# 6. PROJECT_STATE Schema

The record SHALL contain:

```text id="h8r5ps"
PROJECT_STATE

Project:
ContextBridge

State Version:
<version>

Last Updated:
<timestamp>

Current Phase:
<Px>

Current Checkpoint:
<CP-ID>

Checkpoint Status:
<status>

Completed Checkpoints:
<list>

Active Work:
<description>

Blocked Work:
<description>

Failed Attempts:
<references>

Open Decisions:
<references>

Required Approvals:
<references>

Latest Validation:
<validation record>

Repository State:
<branch / commit / working-tree status>

Known Defects:
<references>

Known Limitations:
<references>

Next Permitted Action:
<exact action>

Prohibited Next Actions:
<actions>

Last Handoff:
<reference>

State Owner:
<Project owner / agent>

State Integrity:
<verified / needs reconciliation>
```

---

# 7. Current Phase

`Current Phase` identifies the active master phase.

Example:

```text
Current Phase:
P2 — Protocol + Contract Foundation
```

Only one phase may be marked current.

A later phase SHALL NOT become current before its prerequisites are satisfied.

---

# 8. Current Checkpoint

`Current Checkpoint` identifies the active checkpoint.

Example:

```text
Current Checkpoint:
CP-P2-03 — Schema Validation + Structured Results
```

Only one checkpoint should normally be active.

If no checkpoint is active:

```text
Current Checkpoint:
NONE
```

This is valid during decision/blocking periods.

---

# 9. Completed Checkpoints

Completed checkpoints SHALL be listed explicitly.

Example:

```text
Completed Checkpoints:
- CP-P0-01 ✓
- CP-P1-01 ✓
- CP-P1-02 ✓
```

A checkpoint counts as completed only when its official acceptance conditions have been satisfied.

Merged code alone does not qualify.

---

# 10. Active Work

Active work must describe what is actually happening.

Example:

```text
Active Work:
CP-P2-03
Implementing runtime tool-input validation.

Current EWU:
EWU-02 — Reject malformed tool arguments.

Expected next validation:
Schema rejection tests.
```

Avoid vague state such as:

> "Working on backend."

---

# 11. Blocked Work

Blocked work SHALL identify:

* checkpoint
* blocker
* reason
* dependency
* owner action required
* whether work can proceed elsewhere

Example:

```text
Blocked Work:

CP-P1-03
Blocker:
Current MCP client/transport compatibility is unresolved.

Required action:
Owner decision on transport selection.

Status:
BLOCKED
```

---

# 12. Failed Attempts

Failed attempts must not disappear.

The state should reference the relevant record rather than storing huge logs directly.

Example:

```text
Failed Attempts:

FA-002
Checkpoint:
CP-P2-01

Attempt:
Initial transport configuration

Failure:
Client failed during initialization.

Diagnosis:
Configuration mismatch.

Learning:
The selected client expects the approved transport configuration.

Current status:
Resolved.
```

The objective is:

> **Never make the next agent repeat a known failed approach without understanding why it failed.**

---

# 13. Open Decisions

Every unresolved project-level decision must be explicitly recorded.

Example:

```text
Open Decisions:

DR-003
Question:
Should the deployment use option A or B?

Impact:
Production transport.

Status:
Awaiting owner approval.
```

A proposal SHALL NOT appear as an approved decision.

---

# 14. Required Approvals

The state must explicitly identify pending human decisions.

Example:

```text
Required Approvals:

- CP-P3-02 authorization model
- CP-P3-05 audit/security behavior
```

If there are none:

```text
Required Approvals:
NONE
```

---

# 15. Latest Validation

`PROJECT_STATE` SHALL reference the most recent meaningful validation.

Example:

```text
Latest Validation:

VR-014

Result:
PASS

Scope:
CP-P2-03

Validation:
Schema rejection tests

Evidence:
<repository/CI reference>

Date:
<timestamp>
```

A generic "CI green" statement is insufficient.

---

# 16. Repository State

The state must identify the actual repository condition.

Minimum:

```text
Branch:
<current branch>

Commit:
<commit SHA>

Working Tree:
CLEAN / DIRTY

Remote:
<Synchronized / Diverged / Unknown>

Latest PR:
<reference>

Latest Merge:
<commit>

Unexpected Changes:
NONE / description
```

The AI must verify this rather than blindly trusting the previous state record.

---

# 17. Known Defects

Known defects must remain visible.

Example:

```text
Known Defects:

DEF-003
Component:
Tool registry

Problem:
Duplicate tool registration under condition X.

Severity:
Medium

Status:
Open

Affected checkpoint:
CP-P4-02
```

A known defect must not be forgotten simply because another checkpoint is active.

---

# 18. Next Permitted Action

This is one of the most important fields.

It must describe the **specific next action allowed by the project state**.

Example:

```text
Next Permitted Action:

Inspect the current MCP server implementation and
prepare the CP-P2-03 schema-validation work.
```

Not:

> "Continue development."

---

# 19. Prohibited Next Actions

Where necessary, state what the next agent must NOT do.

Example:

```text
Prohibited Next Actions:

- Do not modify MCP transport.
- Do not change database schema.
- Do not introduce a new framework.
- Do not begin CP-P3.
```

This protects the project from accidental scope drift.

---

# 20. DECISION_LOG

The `DECISION_LOG` records decisions that affect the project.

Recommended location:

```text
docs/decisions/DECISION_LOG.md
```

Individual major decisions may also have dedicated ADRs under:

```text
docs/decisions/
```

---

# 21. DECISION_LOG Schema

Each decision SHALL contain:

```text
Decision ID
Date
Question / Problem
Context
Options Considered
Decision
Reason
Trade-offs
Consequences
Requirements Affected
Architecture Affected
Technology Affected
Security Impact
Status
Approver
Evidence / References
```

---

# 22. Decision Status

Only these statuses should be used:

```text
PROPOSED
OPEN
APPROVED
REJECTED
SUPERSEDED
```

An AI SHALL NOT convert:

```text
PROPOSED
```

into:

```text
APPROVED
```

without the required approval.

---

# 23. CHECKPOINT_LOG

The `CHECKPOINT_LOG` is the permanent engineering progression record.

Recommended location:

```text
docs/project-state/CHECKPOINT_LOG.md
```

It records the state of every checkpoint.

---

# 24. CHECKPOINT_LOG Schema

Each checkpoint SHALL contain:

```text
Checkpoint ID
Phase
Name
Status
Prerequisites
Started
Completed
Branch
Issue
PR
Commits
Validation Records
Acceptance Result
Human Approval
Known Failures
Completion Evidence
Next Checkpoint
```

---

# 25. Checkpoint Status Integrity

The following relationship must remain true:

```text
DEFINED
 ↓
READY
 ↓
IN PROGRESS
 ↓
VALIDATION
 ↓
ACCEPTANCE REVIEW
 ↓
APPROVED
 ↓
MERGED
 ↓
CLOSED
```

A checkpoint cannot jump directly from:

```text
IN PROGRESS
```

to:

```text
CLOSED
```

without evidence and required review.

---

# 26. VALIDATION_RECORD

Every meaningful validation event should produce a validation record.

Recommended location:

```text
docs/project-state/validation/
```

Example:

```text
VR-001.md
VR-002.md
VR-003.md
```

---

# 27. VALIDATION_RECORD Schema

Each record SHALL contain:

```text
Validation ID
Date
Checkpoint
Engineering Work Unit
Requirement(s)
Validation Type
Environment
Version/Commit
Procedure
Expected Result
Actual Result
Evidence
Status
Failures
Follow-up
Reviewer
```

---

# 28. Validation Types

Use explicit categories:

```text
UNIT
INTEGRATION
MCP_PROTOCOL
CONTRACT
SECURITY
E2E
REGRESSION
PERFORMANCE
AI_EVALUATION
UX
DEPLOYMENT
OBSERVABILITY
FAILURE
MANUAL
```

Not every checkpoint needs every type.

---

# 29. Validation Status

Only:

```text
PASS
FAIL
BLOCKED
NOT RUN
NOT APPLICABLE
```

Never use:

```text
"mostly passed"
"probably works"
"looks good"
```

for formal validation state.

---

# 30. Evidence Rule

Every `PASS` must have evidence.

Example:

```text
PASS

Evidence:
- Test suite output
- Protocol interaction
- Audit event
- Relevant commit
```

If evidence cannot be produced:

> The validation cannot be marked PASS.

---

# 31. HANDOFF_RECORD

The `HANDOFF_RECORD` is the document specifically designed for a new AI session.

Recommended location:

```text
docs/project-state/HANDOFF.md
```

It should be updated whenever:

* an AI session ends during active work
* a checkpoint changes state
* a blocker appears
* an important decision is made
* a major validation occurs
* repository state changes materially

---

# 32. HANDOFF_RECORD Schema

Every handoff SHALL contain:

```text
Handoff ID

Date / Time

Previous Agent

Current Phase

Current Checkpoint

Checkpoint Status

Completed Work

Active Work

Uncommitted Work

Blocked Work

Failed Attempts

Open Decisions

Required Approvals

Latest Validation

Repository State

Known Defects

Known Limitations

Relevant Documents

Next Permitted Action

Prohibited Actions

Risks

Required Human Action

Resume Instructions
```

---

# 33. Handoff Principle

A handoff must answer:

> **"If I know nothing about this project except the repository, what do I need to know before touching code?"**

If that question cannot be answered from the handoff + persistent project records:

> The handoff is incomplete.

---

# 34. GitHub vs Repository vs Notebook

ContextBridge deliberately separates **persistent engineering truth** from **temporary conversational context**.

---

# 35. Repository — Authoritative Engineering State

The repository SHALL contain information required to reproduce, understand and continue the project.

This includes:

```text
Architecture
Requirements
Decisions
MCP documentation
Checkpoint definitions
Validation records
Project state
Handoff state
Security documentation
Threat model
Tool catalog
Deployment documentation
Relevant test definitions
```

The repository is the primary persistent engineering memory.

---

# 36. GitHub — Engineering Workflow State

GitHub should contain:

```text
Issues
PRs
Reviews
CI results
Branches
Commit history
Release tags
Checkpoint references
Discussion around implementation
```

GitHub answers:

> **What engineering activity occurred and what was reviewed?**

---

# 37. Repository vs GitHub

They serve different purposes.

```text
Repository
    ↓
What the system IS
+
What decisions govern it
+
What has been validated
+
What the current state is

GitHub
    ↓
What changed
+
Why it changed
+
Who reviewed it
+
What CI observed
+
How work progressed
```

Important state should not exist exclusively in a transient GitHub comment.

Critical decisions and state should be persisted in repository documentation.

---

# 38. Notebook / Conversation Reference Material

Notebook/reference material may contain:

* learning notes
* explanations
* brainstorming
* temporary investigation
* exploratory research
* discarded ideas
* educational examples
* detailed reasoning that is not authoritative

It should NOT be the sole source for:

* current checkpoint
* approved architecture
* approved technology
* security policy
* acceptance state
* production state
* final decisions

---

# 39. Authority Rule

If a conversation says:

> "The checkpoint is complete."

but the repository says:

> `VALIDATION: NOT RUN`

the checkpoint is **not complete**.

If a conversation says:

> "We decided to use X."

but the decision record says:

> `OPEN`

the decision is **not approved**.

Persistent records win.

---

# 40. Fresh AI Resume Sequence

A new AI agent MUST NOT touch code immediately.

It must follow this sequence.

```text
STEP 1
Identify repository
        ↓
STEP 2
Inspect Git state
        ↓
STEP 3
Read project state
        ↓
STEP 4
Read current handoff
        ↓
STEP 5
Read checkpoint definition
        ↓
STEP 6
Read relevant requirements
        ↓
STEP 7
Read relevant architecture
        ↓
STEP 8
Read relevant decisions
        ↓
STEP 9
Read latest validation
        ↓
STEP 10
Inspect current implementation
        ↓
STEP 11
Verify prerequisites
        ↓
STEP 12
Determine permitted action
        ↓
STEP 13
Only then modify code
```

---

# 41. Resume Step 1 — Identify Repository

The AI must establish:

* repository path
* repository remote
* current branch
* project identity

Do not assume the working directory is correct.

---

# 42. Resume Step 2 — Inspect Git State

Run the equivalent of:

```text
git status
git branch
git remote -v
git log --oneline --decorate -10
```

The AI must establish whether:

* working tree is clean
* branch is correct
* local and remote are synchronized
* unexpected work exists

---

# 43. Resume Step 3 — Read PROJECT_STATE

Read:

```text
docs/project-state/PROJECT_STATE.md
```

Determine:

* current phase
* current checkpoint
* active work
* blockers
* decisions
* validation
* next permitted action

---

# 44. Resume Step 4 — Read HANDOFF

Read:

```text
docs/project-state/HANDOFF.md
```

Determine:

* what the previous agent was doing
* what was completed
* what failed
* what remains
* what must not be repeated

---

# 45. Resume Step 5 — Read Checkpoint Definition

Locate the current checkpoint specification.

Confirm:

* objective
* prerequisites
* allowed work
* forbidden work
* acceptance criteria
* validation method
* human approval requirement

---

# 46. Resume Step 6 — Read Requirements

Read only the requirements relevant to the current checkpoint first.

Determine:

```text
What requirement am I implementing?
Why?
What behavior must be demonstrated?
```

---

# 47. Resume Step 7 — Read Architecture

Read the relevant architectural components.

The AI must establish:

* component responsibility
* interfaces
* trust boundaries
* data flows
* constraints

Do not redesign architecture during resume.

---

# 48. Resume Step 8 — Read Decisions

Read relevant decision records.

Determine:

* what has already been decided
* what alternatives were rejected
* what remains open
* whether the current work depends on an approval

This prevents rediscovering and reversing old decisions unnecessarily.

---

# 49. Resume Step 9 — Read Latest Validation

Inspect the latest relevant validation record.

Determine:

```text
What already works?
What failed?
What has not been tested?
What evidence exists?
```

This is critical for avoiding repeated failed attempts.

---

# 50. Resume Step 10 — Inspect Implementation

Only after understanding project state should the AI inspect the actual relevant code.

It must determine:

```text
Expected behavior
        vs
Actual behavior
```

Do not start from assumptions.

---

# 51. Resume Step 11 — Verify Prerequisites

Before modifying code:

```text
All prerequisites satisfied?
        │
    ┌───┴───┐
   YES      NO
    │        │
    ▼        ▼
continue   BLOCK
```

If prerequisites are missing:

> Do not start the checkpoint.

---

# 52. Resume Step 12 — Determine Permitted Action

The AI must identify the exact next action allowed by:

```text
PROJECT_STATE
+
CHECKPOINT
+
REQUIREMENTS
+
ARCHITECTURE
+
DECISIONS
+
LATEST VALIDATION
```

If the permitted action is unclear:

> stop and clarify.

---

# 53. Resume Step 13 — Modify Code

Only now may implementation begin.

The AI must:

* create/use the correct branch
* make bounded changes
* preserve working behavior
* validate changes
* record evidence

---

# 54. Resume Safety Gate

Before the first modification, the AI must internally establish:

```text
I know:
✓ Current phase
✓ Current checkpoint
✓ Objective
✓ Requirements
✓ Architecture boundary
✓ Approved technologies
✓ Existing implementation
✓ Known failures
✓ Open decisions
✓ Latest validation
✓ Next permitted action
```

If any critical item is unknown:

> **Do not modify code.**

---

# 55. Handoff After Interrupted Work

If a session ends during active work, the AI should update the handoff state before stopping when possible.

The handoff must identify:

```text
What changed
What remains
What is uncommitted
What passed
What failed
What must happen next
```

---

# 56. Uncommitted Work in Handoff

If work remains uncommitted:

```text
Uncommitted Work:
YES

Files:
...

Reason:
Implementation incomplete.

Validation:
Not final.

Next action:
Continue validation before commit.
```

The next AI must inspect the actual working tree before continuing.

---

# 57. Handoff With Failed Work

If an attempt failed:

```text
Failed Attempt:
FA-XXX

What was attempted:
...

Observed failure:
...

Diagnosis:
...

What was learned:
...

Do not repeat:
...

Recommended next approach:
...
```

This turns failure into persistent engineering knowledge.

---

# 58. Handoff With a Blocker

If blocked:

```text
Status:
BLOCKED

Blocker:
...

Why work cannot continue:
...

Required decision:
...

Owner:
Project owner

Permitted action while blocked:
<if any>
```

A new AI must not bypass the blocker by inventing a workaround unless that workaround is already authorized.

---

# 59. State Reconciliation

The AI must assume state records can become stale.

Therefore:

```text
Persistent State
      ↓
Verify against GitHub
      ↓
Verify against repository
      ↓
Verify against actual working tree
      ↓
Reconcile discrepancies
```

The AI may correct **stale state documentation** when the actual authoritative engineering state is clear.

It must not rewrite history to make state appear consistent.

---

# 60. State Integrity Conflict

If the AI cannot determine which state is correct:

```text
STATE CONFLICT

PROJECT_STATE:
...

GitHub:
...

Repository:
...

Working tree:
...

Cannot safely determine authoritative state.

Status:
BLOCKED

Required action:
Human reconciliation.
```

---

# 61. State Update Rule

State should be updated at meaningful transitions:

```text
Checkpoint started
Checkpoint blocked
Checkpoint failed
Validation completed
Approval obtained
PR merged
Checkpoint closed
Decision approved
Architecture changed
Production deployed
Production rolled back
```

Do not create state churn for trivial activity.

---

# 62. State Versioning

`PROJECT_STATE` should include a state version.

Example:

```text
State Version:
0.8
```

The version identifies the state record revision, not the software release.

Software versions and project-state versions must not be confused.

---

# 63. Single Active Work Rule

Normally:

```text
One phase
   ↓
One checkpoint
   ↓
One active engineering objective
```

Parallel work is allowed only when explicitly authorized and when state tracking can remain unambiguous.

---

# 64. Next Action Rule

The project must always attempt to expose one clear next permitted action.

Example:

```text
NEXT PERMITTED ACTION

CP-P2-03:
Run schema-validation tests against the newly
implemented tool contracts.
```

This is much safer than:

> "Continue development."

---

# 65. Permanent State Layout

The recommended project-state area is:

```text
docs/
└── project-state/
    ├── PROJECT_STATE.md
    ├── HANDOFF.md
    ├── CHECKPOINT_LOG.md
    ├── validation/
    │   ├── VR-001.md
    │   ├── VR-002.md
    │   └── ...
    └── failures/
        ├── FA-001.md
        └── ...
```

And:

```text
docs/
└── decisions/
    ├── DECISION_LOG.md
    ├── ADR-001-*.md
    └── ...
```

These directories should be created when the project begins formally recording these artifacts.

---

# 66. Minimum Fresh-Session Reading Set

A fresh AI does NOT need to read the entire repository before every task.

Minimum initial set:

```text
1. PROJECT_STATE.md
2. HANDOFF.md
3. CHECKPOINT definition
4. Relevant requirements
5. Relevant architecture
6. Relevant decisions
7. Latest relevant validation
8. Git state
```

Then inspect the relevant implementation.

---

# 67. Fresh Session Decision Tree

```text
START
  ↓
Read PROJECT_STATE
  ↓
Read HANDOFF
  ↓
Git state
  ↓
Is repository consistent?
  ├── NO → STOP / RECONCILE
  └── YES
          ↓
Current checkpoint known?
  ├── NO → STOP / RESOLVE
  └── YES
          ↓
Prerequisites satisfied?
  ├── NO → BLOCK
  └── YES
          ↓
Open approval required?
  ├── YES → STOP / REQUEST APPROVAL
  └── NO
          ↓
Read requirements + architecture
          ↓
Inspect implementation
          ↓
Determine permitted action
          ↓
IMPLEMENT
```

---

# 68. What Must Never Depend on Memory

The following SHALL never exist only in conversation:

* current checkpoint
* completed checkpoint list
* approved architecture
* approved technologies
* security decisions
* API contracts
* database schema decisions
* acceptance results
* known defects
* blockers
* failed approaches that affect future work
* required approvals
* next permitted action

If it matters to safe continuation:

> **persist it.**

---

# 69. What May Remain Conversational

The following may remain in conversation/reference material unless they become decisions:

* brainstorming
* explanations
* educational discussions
* temporary hypotheses
* discarded ideas
* exploratory implementation thoughts
* casual discussion

Once an exploratory item becomes a project decision:

> move it into the persistent decision system.

---

# 70. Final Continuity Invariant

ContextBridge must maintain this invariant:

> **A fresh competent engineer or AI agent must be able to reconstruct the project's authoritative current state without access to previous hidden conversation memory.**

---

# 71. Final Resume Invariant

Before touching code, a new AI must be able to answer:

```text
What phase are we in?
What checkpoint are we executing?
Why does it exist?
What must be true when finished?
What has already been done?
What has failed?
What decisions constrain me?
What architecture must I preserve?
What is the repository state?
What was most recently validated?
What exactly am I allowed to do next?
```

If it cannot answer these questions reliably:

> **It must not modify the repository.**

---

# 72. Final State Model

The complete ContextBridge state model is:

```text
                    PROJECT_STATE
                         │
       ┌─────────────────┼─────────────────┐
       ▼                 ▼                 ▼
 DECISION_LOG      CHECKPOINT_LOG    VALIDATION_RECORD
       │                 │                 │
       └─────────────────┼─────────────────┘
                         ▼
                  HANDOFF_RECORD
                         │
                         ▼
                 FRESH AI SESSION
                         │
                         ▼
                  STATE RECONCILIATION
                         │
                         ▼
                  SAFE RESUMPTION
```

---

# 73. Final Engineering Continuity Principle

The project should never reach a state where the answer to:

> "What should I do next?"

is:

> "Ask the previous AI."

The answer must instead be:

```text
Read PROJECT_STATE
       ↓
Read HANDOFF
       ↓
Read current CHECKPOINT
       ↓
Verify repository
       ↓
Follow NEXT PERMITTED ACTION
```

---

# 74. Policy Freeze

This document is the **FINAL PROJECT STATE AND HANDOFF PROTOCOL** for ContextBridge.

It governs:

* canonical project state
* decision persistence
* checkpoint state
* validation records
* AI handoffs
* repository/GitHub state
* Notebook/reference separation
* fresh-session resumption
* state reconciliation
* continuation safety

It SHALL NOT be materially modified without an explicit project-owner-approved change request.

The objective is permanent continuity:

> **No hidden conversational memory should be required to safely continue ContextBridge engineering.**

**END OF FINAL PROJECT STATE AND HANDOFF PROTOCOL**

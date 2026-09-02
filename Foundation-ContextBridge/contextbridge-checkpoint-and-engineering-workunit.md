# FINAL CHECKPOINT AND ENGINEERING-WORK-UNIT SYSTEM

## ContextBridge

**Status:** FINAL
**Applies to:** All ContextBridge engineering work
**Authority:** Final Master Phase Plan and all upstream approved project documents

---

# 1. Purpose

The ContextBridge project SHALL be executed through bounded **Engineering Work Units (EWUs)** grouped into **Checkpoints**.

The system exists to prevent:

* uncontrolled autonomous implementation
* large ambiguous coding tasks
* premature architecture changes
* "code exists = complete" reasoning
* unverified security claims
* accidental scope expansion
* unreviewed AI-generated changes
* giant PRs with unclear responsibility
* merging work without demonstrated acceptance

The fundamental rule is:

> **A checkpoint is an evidence-producing engineering unit, not a coding task.**

---

# 2. Checkpoint Completion Principle

A checkpoint is complete only when:

```text
Work performed
      ↓
Artifacts produced
      ↓
Validation executed
      ↓
Acceptance criteria demonstrated
      ↓
Evidence captured
      ↓
Human approval where required
      ↓
PR merged
      ↓
Checkpoint closed
```

Therefore:

```text
CODE EXISTS          ≠ COMPLETE

COMMIT EXISTS        ≠ COMPLETE

PR EXISTS            ≠ COMPLETE

TESTS WERE ATTEMPTED ≠ COMPLETE

TESTS PASS            ≠ COMPLETE
```

Completion requires **demonstrated acceptance criteria**.

---

# 3. Engineering Work Unit

An **Engineering Work Unit (EWU)** is the smallest bounded piece of engineering work an autonomous AI agent may execute without redefining the project.

An EWU must have:

* one clearly defined objective
* bounded scope
* explicit inputs
* explicit outputs
* validation method
* acceptance criteria
* prohibited work
* dependencies
* completion evidence

Example:

```text
EWU
│
├── Objective
├── Scope
├── Inputs
├── Allowed changes
├── Forbidden changes
├── Artifacts
├── Validation
├── Acceptance criteria
└── Evidence
```

An EWU SHALL NOT silently expand into another checkpoint.

---

# 4. Relationship Between Units

```text
MASTER PHASE
     │
     ├── Checkpoint
     │      ├── EWU
     │      ├── EWU
     │      └── EWU
     │
     ├── Checkpoint
     │      ├── EWU
     │      └── EWU
     │
     └── Checkpoint
            └── EWU
```

The hierarchy is:

> **Phase → Checkpoint → Engineering Work Unit → Evidence**

---

# 5. Checkpoint Lifecycle

Every checkpoint follows:

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

A checkpoint may instead enter:

```text
FAILED
   ↓
REWORK REQUIRED
   ↓
IN PROGRESS
```

---

# 6. Checkpoint Statuses

Only these statuses SHALL be used:

### DEFINED

Checkpoint specification exists but prerequisites are not yet satisfied.

### READY

All prerequisites are satisfied and work may begin.

### IN PROGRESS

Engineering work is actively occurring.

### VALIDATION

Implementation is complete enough to test against acceptance criteria.

### ACCEPTANCE REVIEW

Evidence is being reviewed.

### APPROVED

Acceptance criteria have been demonstrated and required human approval has been obtained.

### MERGED

The approved implementation has been merged into the designated branch.

### CLOSED

The checkpoint has been formally closed with completion evidence.

### BLOCKED

A dependency or decision prevents progress.

### FAILED

Acceptance criteria were not met.

### REWORK REQUIRED

Work exists but requires corrective changes.

---

# 7. Master Phase → Checkpoint Structure

The minimum checkpoint structure is:

```text
P0 ── 1 checkpoint
P1 ── 4 checkpoints
P2 ── 4 checkpoints
P3 ── 5 checkpoints
P4 ── 4 checkpoints
P5 ── 4 checkpoints
P6 ── 4 checkpoints
```

Total:

> **26 bounded checkpoints**

This is the minimum practical decomposition that keeps major decisions, protocol implementation, security implementation, integration, verification and deployment independently verifiable.

---

# PHASE P0 — REPOSITORY BASELINE

## CP-P0-01 — Repository Foundation

### Objective

Verify and preserve the existing ContextBridge repository foundation.

### Prerequisites

None.

### Allowed work

* inspect Git state
* verify remote
* verify branch
* verify baseline files
* verify documentation directories
* make only corrective repository-baseline changes if genuinely necessary

### Expected artifacts

* clean repository
* README
* LICENSE
* `.gitignore`
* documentation directories
* synchronized GitHub repository

### Validation method

Git inspection:

```text
git status
git remote -v
git branch
git log
```

### Acceptance criteria

* repository exists
* `main` exists
* remote is correct
* local and remote history are synchronized
* working tree is clean
* baseline documentation exists

### Definition of Done

All acceptance criteria demonstrated.

### Failure conditions

* repository diverges unexpectedly
* baseline files missing
* uncommitted unexplained changes
* remote mismatch

### Dependencies

None.

### Human approval

Not required if current state already satisfies criteria.

### Expected GitHub activity

No new implementation issue required unless corrective work is necessary.

### Branch/PR behavior

No branch required for inspection-only validation.

### Completion evidence

Repository state + commit history.

### Next checkpoint

**CP-P1-01**

---

# PHASE P1 — TECHNOLOGY + PRODUCT RESOLUTION

# CP-P1-01 — Product Use-Case Resolution

### Objective

Resolve the final real-world domain/use case without changing the approved product vision.

### Prerequisites

P0 complete.

### Allowed work

* evaluate candidate domains
* assess AI usefulness
* assess tool usefulness
* assess permission relevance
* assess structured-data relevance
* assess implementation feasibility
* document decision

### Forbidden

* redesigning ContextBridge
* adding unrelated product capabilities
* introducing technologies merely to support a domain

### Expected artifacts

* finalized use-case definition
* primary user definition
* core demo workflow
* tool-domain boundary

### Validation

Trace selected use case against requirements.

### Acceptance criteria

The selected domain demonstrates:

* genuine AI tool-use value
* meaningful structured data
* meaningful authorization
* controlled data exposure
* realistic implementation scope

### Definition of Done

Owner-approved use case exists.

### Failure conditions

* domain is artificial
* domain requires unsupported architecture
* scope expands beyond approved product

### Dependencies

P0.

### Human approval

**MANDATORY**

### GitHub

Issue + documentation PR.

### Branch

```text
decision/p1-use-case
```

### Completion evidence

Approved decision document.

### Next checkpoint

**CP-P1-02**

---

# CP-P1-02 — Implementation Stack Resolution

### Objective

Select the actual implementation technologies.

### Prerequisites

CP-P1-01.

### Allowed work

Evaluate:

* language
* runtime
* MCP SDK
* validation technology
* database
* database access layer
* testing framework
* deployment approach
* observability implementation

### Acceptance criteria

Every selected technology has:

* requirement justification
* alternative evaluation
* security consideration
* cost consideration
* operational consideration

### Definition of Done

Technology baseline is explicitly approved.

### Human approval

**MANDATORY**

### GitHub

Architecture/decision issue + ADR/documentation PR.

### Next checkpoint

**CP-P1-03**

---

# CP-P1-03 — MCP Client + Transport Resolution

### Objective

Resolve the actual MCP client and transport required for development and public deployment.

### Prerequisites

CP-P1-02.

### Allowed work

* verify current MCP requirements
* evaluate client compatibility
* evaluate local transport
* evaluate remote transport
* document deployment implications

### Acceptance criteria

The selected client can:

* connect
* initialize
* discover tools
* invoke tools

using the selected implementation approach.

### Human approval

**MANDATORY**

### Expected artifacts

* MCP client decision
* transport decision
* compatibility notes

### Next checkpoint

**CP-P1-04**

---

# CP-P1-04 — Security + Deployment Decision Baseline

### Objective

Resolve implementation-level security and deployment decisions.

### Prerequisites

CP-P1-03.

### Allowed work

Finalize:

* authentication mechanism
* authorization implementation
* role/permission model
* storage decision
* hosting model
* secrets configuration approach

### Acceptance criteria

No implementation-blocking security/deployment OPEN QUESTION remains.

### Human approval

**MANDATORY**

### Next checkpoint

**CP-P2-01**

---

# PHASE P2 — PROTOCOL + CONTRACT FOUNDATION

# CP-P2-01 — MCP Server Foundation

### Objective

Establish a genuine working MCP server.

### Prerequisites

All P1 checkpoints approved.

### Allowed work

* initialize project
* configure runtime
* install approved MCP SDK
* implement server startup
* implement protocol initialization
* establish selected transport

### Forbidden

* external production integration
* authorization implementation
* unrelated frontend

### Acceptance criteria

An MCP-compatible client can establish a valid connection.

### Definition of Done

Connection demonstrated with evidence.

### Human approval

Not normally required.

### GitHub

Issue → branch → PR.

### Branch

```text
feat/p2-mcp-foundation
```

### Next checkpoint

CP-P2-02.

---

# CP-P2-02 — Tool Discovery + Registry

### Objective

Implement the controlled tool registry and discovery mechanism.

### Prerequisites

CP-P2-01.

### Allowed work

* define tool names
* descriptions
* input contracts
* output contracts
* discovery behavior

### Acceptance criteria

Client can discover all intended initial tools with correct schemas/descriptions.

### Validation

Tool discovery test + client demonstration.

### Human approval

Not required unless tool scope changes.

### Next checkpoint

CP-P2-03.

---

# CP-P2-03 — Schema Validation + Structured Results

### Objective

Establish safe tool input/output contracts.

### Prerequisites

CP-P2-02.

### Allowed work

* input schemas
* runtime validation
* output contracts
* structured result mapping
* validation failures

### Acceptance criteria

Valid requests proceed.

Invalid requests are rejected before underlying access.

Results conform to defined contracts.

### Failure conditions

* malformed input reaches external systems
* raw internal objects are exposed
* validation is treated as authorization

### Next checkpoint

CP-P2-04.

---

# CP-P2-04 — Protocol + Error Foundation

### Objective

Complete protocol behavior and safe structured error handling.

### Prerequisites

CP-P2-03.

### Allowed work

* MCP protocol tests
* structured errors
* error classification
* safe error mapping

### Acceptance criteria

Required protocol flows work and failures do not expose internal implementation details.

### Human approval

Not required.

### Next checkpoint

CP-P3-01.

---

# PHASE P3 — SECURITY + CONTROL PLANE

# CP-P3-01 — Identity + Authentication

### Objective

Establish verified request identity.

### Prerequisites

P2 complete.

### Allowed work

Implement the approved authentication mechanism.

### Acceptance criteria

* valid identity accepted
* invalid credentials rejected
* identity available to authorization layer
* credentials never exposed in tool results/logs

### Human approval

Required for final authentication behavior.

### Next checkpoint

CP-P3-02.

---

# CP-P3-02 — Authorization + RBAC

### Objective

Implement policy enforcement independently of the model.

### Prerequisites

CP-P3-01.

### Allowed work

* roles
* permissions
* policy evaluation
* tool authorization
* allow/deny decisions

### Acceptance criteria

```text
authorized → executes
unauthorized → denied
```

and:

```text
DENIED
 ↓
NO EXTERNAL EXECUTION
```

### Human approval

**MANDATORY**

Security-sensitive checkpoint.

### Next checkpoint

CP-P3-03.

---

# CP-P3-03 — Least Privilege + Security Enforcement

### Objective

Demonstrate that tools expose only approved capabilities.

### Prerequisites

CP-P3-02.

### Allowed work

* permission refinement
* sensitive operation restrictions
* malicious argument handling
* privilege escalation tests

### Acceptance criteria

No tool provides unnecessary privileges.

Unauthorized escalation attempts fail.

### Human approval

**MANDATORY**

### Next checkpoint

CP-P3-04.

---

# CP-P3-04 — Safe Data Exposure

### Objective

Prevent unnecessary/sensitive underlying data from reaching the AI.

### Prerequisites

CP-P3-03.

### Allowed work

* result filtering
* result mapping
* sensitive-field exclusion
* output contract enforcement

### Acceptance criteria

Only approved data reaches the AI client.

Sensitive fields cannot be exposed through normal tool responses.

### Next checkpoint

CP-P3-05.

---

# CP-P3-05 — Audit + Secret Protection

### Objective

Make security-relevant operations auditable without leaking secrets.

### Prerequisites

CP-P3-04.

### Allowed work

* audit events
* request IDs
* authorization records
* safe parameter recording
* secret exclusion

### Acceptance criteria

A sensitive operation can be reconstructed from the audit trail without requiring secrets.

### Human approval

**MANDATORY**

### Next checkpoint

CP-P4-01.

---

# PHASE P4 — CONTROLLED INTEGRATION + AI WORKFLOW

# CP-P4-01 — External System Adapter

### Objective

Connect ContextBridge to the approved real-world external system.

### Prerequisites

P3 complete.

### Allowed work

* adapter
* credentials/configuration
* controlled external calls
* upstream error handling

### Acceptance criteria

ContextBridge can communicate with the external system through the controlled adapter.

### Forbidden

Direct AI-to-external-system access.

### Next checkpoint

CP-P4-02.

---

# CP-P4-02 — Integrated Tool Execution

### Objective

Connect approved tools to real external operations.

### Prerequisites

CP-P4-01.

### Acceptance criteria

Each tool:

```text
MCP
 ↓
identity
 ↓
authorization
 ↓
validation
 ↓
adapter
 ↓
external system
 ↓
safe result
```

works correctly.

### Next checkpoint

CP-P4-03.

---

# CP-P4-03 — End-to-End AI Workflow

### Objective

Demonstrate the actual product workflow.

### Prerequisites

CP-P4-02.

### Acceptance criteria

Demonstrate:

1. AI client connects
2. tools are discovered
3. user requests task
4. AI selects tool
5. ContextBridge validates
6. authorization occurs
7. tool executes
8. safe structured result returns
9. AI responds

### Human approval

**MANDATORY**

This is a core product checkpoint.

### Next checkpoint

CP-P4-04.

---

# CP-P4-04 — Denied Operation Demonstration

### Objective

Demonstrate the defining security property of ContextBridge.

### Prerequisites

CP-P4-03.

### Acceptance criteria

An AI-requested operation that exceeds permissions is:

```text
requested
 ↓
identified
 ↓
denied
 ↓
not executed
 ↓
audited
```

### Human approval

**MANDATORY**

### Next checkpoint

CP-P5-01.

---

# PHASE P5 — VERIFICATION + OBSERVABILITY

# CP-P5-01 — Functional Verification

### Objective

Verify all required functionality.

### Prerequisites

P4 complete.

### Allowed work

* unit tests
* integration tests
* protocol tests
* tool tests
* error tests

### Acceptance criteria

All mandatory functional requirements have passing evidence.

### Next checkpoint

CP-P5-02.

---

# CP-P5-02 — Security Verification

### Objective

Attack the system against its documented threat model.

### Prerequisites

CP-P5-01.

### Required testing areas

* unauthorized access
* malformed input
* injection
* privilege escalation
* data leakage
* prompt-injection-related scenarios
* secret exposure
* rate abuse where implemented

### Acceptance criteria

No critical security requirement fails.

### Human approval

**MANDATORY**

### Next checkpoint

CP-P5-03.

---

# CP-P5-03 — Observability + Audit Verification

### Objective

Prove that operations can be reconstructed and investigated.

### Prerequisites

CP-P5-02.

### Acceptance criteria

Given a request identifier, the system can establish:

```text
who
 ↓
requested what
 ↓
authorization result
 ↓
tool execution
 ↓
latency/status
 ↓
failure/result
```

without exposing secrets.

### Next checkpoint

CP-P5-04.

---

# CP-P5-04 — Evaluation + Release Readiness

### Objective

Determine whether the system actually satisfies the approved requirements.

### Prerequisites

CP-P5-03.

### Allowed work

* requirements traceability
* performance evaluation
* reliability evaluation
* security evidence review
* maintainability review
* documentation review

### Acceptance criteria

Every mandatory requirement has:

```text
Requirement
 ↓
Implementation
 ↓
Validation
 ↓
Evidence
```

### Human approval

**MANDATORY**

### Next checkpoint

CP-P6-01.

---

# PHASE P6 — PRODUCTION DEPLOYMENT + FINAL VALIDATION

# CP-P6-01 — Production Environment

### Objective

Prepare the actual public deployment environment.

### Prerequisites

P5 complete.

### Allowed work

* hosting configuration
* production database/storage
* secrets
* environment configuration
* deployment configuration

### Acceptance criteria

Production environment is reproducibly configured without source-controlled secrets.

### Next checkpoint

CP-P6-02.

---

# CP-P6-02 — Public MCP Deployment

### Objective

Deploy the real ContextBridge service publicly.

### Prerequisites

CP-P6-01.

### Acceptance criteria

A compatible external MCP client can connect to the public service.

### Validation

Remote connection + discovery + tool invocation.

### Human approval

Required before production exposure.

### Next checkpoint

CP-P6-03.

---

# CP-P6-03 — Production Smoke + Security Validation

### Objective

Prove the deployed system behaves like the validated local system.

### Prerequisites

CP-P6-02.

### Acceptance criteria

Production demonstrates:

* successful workflow
* denied workflow
* authorization
* validation
* safe output
* audit
* observability
* safe failures

### Human approval

**MANDATORY**

### Next checkpoint

CP-P6-04.

---

# CP-P6-04 — Final Product Acceptance

### Objective

Establish final project completion.

### Prerequisites

CP-P6-03.

### Allowed work

* final README
* architecture documentation
* tool catalog
* security model
* threat model
* deployment documentation
* demo instructions
* limitations
* roadmap
* repository cleanup

### Acceptance criteria

The complete Definition of Done is demonstrated.

### Human approval

**MANDATORY — FINAL PROJECT ACCEPTANCE**

### Definition of Done

ContextBridge is:

```text
working
+
tested
+
security-validated
+
observable
+
auditable
+
publicly deployed
+
documented
+
demonstrable
+
interview-defensible
```

### Next checkpoint

None.

Project complete.

---

# 8. Global Checkpoint Rules

Every checkpoint SHALL obey these rules.

## Rule 1 — No scope expansion

An agent may not introduce functionality outside the checkpoint.

---

## Rule 2 — No architecture changes

If implementation reveals an architectural problem:

```text
STOP
 ↓
REPORT
 ↓
RAISE CHANGE REQUEST
 ↓
WAIT FOR APPROVAL
```

The agent must not silently redesign the architecture.

---

## Rule 3 — No technology substitution

If an approved technology becomes problematic:

```text
STOP
 ↓
DOCUMENT PROBLEM
 ↓
PROPOSE ALTERNATIVES
 ↓
OWNER DECISION
```

---

## Rule 4 — Evidence before completion

The agent must provide concrete evidence.

Examples:

* test output
* protocol interaction
* screenshots where useful
* logs
* audit records
* deployment response
* Git diff
* requirement traceability

---

## Rule 5 — Negative testing matters

Security checkpoints require demonstrating rejection, not merely successful execution.

---

## Rule 6 — No "tests attempted" completion

The acceptance criteria determine completion.

---

## Rule 7 — One checkpoint at a time

An autonomous agent SHALL work on one active checkpoint unless explicitly authorized otherwise.

---

# 9. Autonomous Agent Operating Contract

An AI coding agent receives:

```text
CHECKPOINT SPECIFICATION
+
APPROVED ARCHITECTURE
+
APPROVED TECHNOLOGY BASELINE
+
REPOSITORY STATE
```

It SHALL NOT receive authority to:

* redefine requirements
* redefine architecture
* select unapproved infrastructure
* expand scope
* weaken security
* bypass tests
* merge its own security-sensitive work without required review

---

# 10. Standard Agent Workflow

For every checkpoint:

```text
1. READ CHECKPOINT
        ↓
2. VERIFY PREREQUISITES
        ↓
3. INSPECT REPOSITORY
        ↓
4. PLAN WITHIN BOUNDARY
        ↓
5. IMPLEMENT ALLOWED WORK
        ↓
6. RUN VALIDATION
        ↓
7. COMPARE AGAINST ACCEPTANCE CRITERIA
        ↓
8. COLLECT EVIDENCE
        ↓
9. REPORT RESULT
        ↓
10. OPEN/UPDATE PR
        ↓
11. HUMAN REVIEW IF REQUIRED
        ↓
12. MERGE
        ↓
13. CLOSE CHECKPOINT
```

---

# 11. GitHub Representation

Every implementation checkpoint SHOULD have one GitHub Issue.

Example:

```text
CP-P2-03 — Schema Validation + Structured Results
```

The issue SHALL contain:

```text
Checkpoint ID
Phase
Objective
Prerequisites
Allowed work
Forbidden work
Acceptance criteria
Validation method
Definition of Done
Dependencies
Human approval requirement
```

---

# 12. Issue Lifecycle

```text
GitHub Issue
    ↓
READY
    ↓
IN PROGRESS
    ↓
PR OPEN
    ↓
VALIDATION
    ↓
REVIEW
    ↓
MERGED
    ↓
CLOSED
```

A GitHub Issue SHALL NOT be closed merely because its PR was merged.

The acceptance evidence must exist.

---

# 13. Branch Convention

Implementation branches SHALL follow:

```text
<type>/<checkpoint-id>-<short-name>
```

Examples:

```text
feat/cp-p2-01-mcp-foundation
feat/cp-p3-02-authorization
test/cp-p5-02-security-verification
chore/cp-p6-04-final-documentation
```

The branch must correspond to one bounded checkpoint.

---

# 14. Commit Convention

Commits SHOULD describe the actual engineering change.

Examples:

```text
feat(mcp): establish server foundation
feat(auth): enforce tool permissions
test(security): add unauthorized tool cases
docs(checkpoint): record validation evidence
```

Avoid meaningless commits such as:

```text
update
changes
final
fix stuff
AI generated
```

---

# 15. Pull Request Rules

A PR SHALL identify:

```text
Checkpoint:
CP-Px-xx

Objective:
...

Changes:
...

Acceptance criteria:
...

Validation:
...

Evidence:
...

Architecture impact:
None / Change Request required

Security impact:
...

Known limitations:
...
```

---

# 16. PR Size Rule

A PR should normally represent one checkpoint.

If a checkpoint is too large to review safely, it may contain multiple EWU commits, but the PR still corresponds to the checkpoint.

If the work cannot reasonably fit into one checkpoint:

> Stop and redesign the **work decomposition**, not the architecture.

---

# 17. Review Rules

### Ordinary checkpoints

One meaningful review is sufficient unless risk dictates otherwise.

### Security-sensitive checkpoints

Mandatory human review applies to:

* authentication
* authorization
* RBAC
* least privilege
* safe output
* audit
* security testing
* production exposure

### Final release

Human approval is mandatory.

---

# 18. Merge Rules

A checkpoint PR may be merged only when:

```text
Acceptance criteria demonstrated
        +
Required tests pass
        +
Required review completed
        +
No unresolved blocking issue
        +
No unauthorized architecture change
```

Then:

```text
MERGE
 ↓
CHECKPOINT EVIDENCE
 ↓
ISSUE CLOSE
```

---

# 19. Evidence Standard

Each checkpoint SHALL produce a completion evidence record.

Minimum format:

```text
Checkpoint:
CP-Px-xx

Implementation:
<what changed>

Validation performed:
<commands/tests/demo>

Acceptance criteria:
AC-01 — PASS
AC-02 — PASS
AC-03 — PASS

Evidence:
<test output / logs / screenshots / links>

Security impact:
<none / description>

Known limitations:
<description>

Human approval:
<required/not required>
<approved/pending>

Final status:
COMPLETE
```

---

# 20. Failure Handling

If acceptance fails:

```text
Checkpoint
   ↓
FAILED
   ↓
Identify failure
   ↓
Determine whether within checkpoint scope
   │
   ├── YES → REWORK
   │
   └── NO → BLOCK + CHANGE REQUEST
```

An agent SHALL NOT:

* weaken acceptance criteria
* delete tests
* redefine the requirement
* bypass authorization
* mark partial work complete

---

# 21. Architecture Change Detection

During any checkpoint, if an agent encounters:

```text
"Current architecture cannot satisfy this requirement."
```

it SHALL stop.

Required response:

```text
ARCHITECTURAL CONFLICT

Checkpoint:
...

Observed problem:
...

Affected architectural component:
...

Requirement:
...

Why current architecture appears insufficient:
...

Proposed alternatives:
...

No implementation change made.
Awaiting owner decision.
```

This prevents autonomous architecture drift.

---

# 22. Technology Change Detection

Similarly:

```text
APPROVED TECHNOLOGY
        ↓
Implementation problem
        ↓
STOP
        ↓
Document alternatives
        ↓
Owner approval
```

An agent cannot silently replace an approved database, framework, authentication mechanism or deployment technology.

---

# 23. Standard Checkpoint Template

The following template SHALL be reused for every future checkpoint.

```text
# CHECKPOINT <ID> — <NAME>

## Phase
<Px>

## Status
DEFINED / READY / IN PROGRESS / VALIDATION /
ACCEPTANCE REVIEW / APPROVED / MERGED / CLOSED /
BLOCKED / FAILED / REWORK REQUIRED

## Objective
<single clear objective>

## Why This Checkpoint Exists
<dependency/reason>

## Prerequisites
- <prerequisite>
- <prerequisite>

## Inputs
- <approved document>
- <existing component>
- <decision>

## Allowed Work
- <allowed work>
- <allowed work>

## Forbidden Work
- <out-of-scope work>
- <architecture changes>
- <technology substitutions>

## Engineering Work Units

### EWU-01
<bounded task>

### EWU-02
<bounded task>

## Expected Artifacts
- <artifact>
- <artifact>

## Validation Method
<exact validation approach>

## Acceptance Criteria

### AC-01
<criterion>

### AC-02
<criterion>

### AC-03
<criterion>

## Definition of Done

The checkpoint is complete only when:
- <condition>
- <condition>
- <condition>

## Failure Conditions
- <failure>
- <failure>

## Dependencies
- <dependency>

## Human Approval Requirement
REQUIRED / NOT REQUIRED

If required:
<what must be approved>

## GitHub Issue
<issue reference>

## Branch
<branch name>

## Expected Commit(s)
<commit convention>

## Pull Request
<PR reference>

## Review Requirement
<review requirement>

## Completion Evidence
- <test output>
- <demo>
- <log>
- <documentation>
- <other evidence>

## Architecture Impact
NONE / CHANGE REQUEST REQUIRED

## Security Impact
<description>

## Known Limitations
<description>

## Final Status
COMPLETE / FAILED / BLOCKED

## Next Checkpoint
<checkpoint ID>
```

---

# 24. Standard Engineering Work Unit Template

Each checkpoint's internal work units SHALL use:

```text
## EWU-<number> — <name>

### Objective
<one bounded objective>

### Inputs
<what the agent may rely upon>

### Allowed Changes
<files/components/behavior>

### Forbidden Changes
<explicit boundary>

### Expected Output
<artifact>

### Validation
<how correctness is demonstrated>

### Acceptance
<what must be true>

### Failure Condition
<what stops the unit>

### Evidence
<what must be reported>
```

---

# 25. Checkpoint Completion Equation

For ContextBridge:

```text
                 CHECKPOINT COMPLETE
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
       Artifacts      Validation     Acceptance
          │              │              │
          └──────────────┼──────────────┘
                         ▼
                    Evidence
                         │
                         ▼
                 Required Review
                         │
                         ▼
                       Merge
                         │
                         ▼
                      Closure
```

If any required element is missing:

> **Checkpoint = NOT COMPLETE**

---

# 26. Phase Completion Rule

A phase is complete only when all of its checkpoints are closed.

```text
P3
 │
 ├── CP-P3-01 ✓
 ├── CP-P3-02 ✓
 ├── CP-P3-03 ✓
 ├── CP-P3-04 ✓
 └── CP-P3-05 ✓
          │
          ▼
       P3 COMPLETE
```

A later phase SHALL NOT begin while a mandatory predecessor checkpoint remains incomplete.

---

# 27. Phase Gate Rule

The phase gate is:

```text
All checkpoint acceptance criteria
             +
All required reviews
             +
All completion evidence
             +
No blocking failures
             ↓
       PHASE COMPLETE
             ↓
       NEXT PHASE READY
```

---

# 28. Autonomous Agent Safety Boundary

The AI agent is authorized to:

```text
READ
 ↓
ANALYZE
 ↓
IMPLEMENT WITHIN CHECKPOINT
 ↓
TEST
 ↓
REPORT
```

It is NOT authorized to:

```text
CHANGE REQUIREMENTS
CHANGE ARCHITECTURE
CHANGE SCOPE
CHANGE SECURITY MODEL
PROMOTE PROPOSALS TO APPROVED TECHNOLOGIES
BYPASS ACCEPTANCE
DECLARE SECURITY SUCCESS WITHOUT EVIDENCE
MERGE REQUIRED HUMAN-REVIEW WORK WITHOUT REVIEW
```

---

# 29. Final Checkpoint Map

```text
P0
└── CP-P0-01 Repository Foundation

P1
├── CP-P1-01 Product Use-Case Resolution
├── CP-P1-02 Implementation Stack Resolution
├── CP-P1-03 MCP Client + Transport Resolution
└── CP-P1-04 Security + Deployment Decision Baseline

P2
├── CP-P2-01 MCP Server Foundation
├── CP-P2-02 Tool Discovery + Registry
├── CP-P2-03 Schema Validation + Structured Results
└── CP-P2-04 Protocol + Error Foundation

P3
├── CP-P3-01 Identity + Authentication
├── CP-P3-02 Authorization + RBAC
├── CP-P3-03 Least Privilege + Security Enforcement
├── CP-P3-04 Safe Data Exposure
└── CP-P3-05 Audit + Secret Protection

P4
├── CP-P4-01 External System Adapter
├── CP-P4-02 Integrated Tool Execution
├── CP-P4-03 End-to-End AI Workflow
└── CP-P4-04 Denied Operation Demonstration

P5
├── CP-P5-01 Functional Verification
├── CP-P5-02 Security Verification
├── CP-P5-03 Observability + Audit Verification
└── CP-P5-04 Evaluation + Release Readiness

P6
├── CP-P6-01 Production Environment
├── CP-P6-02 Public MCP Deployment
├── CP-P6-03 Production Smoke + Security Validation
└── CP-P6-04 Final Product Acceptance
```

---

# 30. Final Dependency Graph

```text
                         P0
                         │
                         ▼
                ┌─────────────────┐
                │ CP-P0-01        │
                └────────┬────────┘
                         │
                         ▼
                         P1
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     CP-P1-01         CP-P1-02         CP-P1-03
                                           │
                                           ▼
                                      CP-P1-04
                                           │
                                           ▼
                         P2
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     CP-P2-01         CP-P2-02         CP-P2-03
                                           │
                                           ▼
                                      CP-P2-04
                                           │
                                           ▼
                         P3
                         │
        ┌────────────────┼─────────────────────┐
        ▼                ▼                     ▼
     CP-P3-01         CP-P3-02              CP-P3-03
                                                 │
                                                 ▼
                                           CP-P3-04
                                                 │
                                                 ▼
                                           CP-P3-05
                                                 │
                                                 ▼
                         P4
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     CP-P4-01         CP-P4-02         CP-P4-03
                                           │
                                           ▼
                                      CP-P4-04
                                           │
                                           ▼
                         P5
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     CP-P5-01         CP-P5-02         CP-P5-03
                                           │
                                           ▼
                                      CP-P5-04
                                           │
                                           ▼
                         P6
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
     CP-P6-01         CP-P6-02         CP-P6-03
                                           │
                                           ▼
                                      CP-P6-04
                                           │
                                           ▼
                                        DONE
```

The visual grouping represents phase boundaries; the mandatory dependency remains sequential.

---

# 31. Final Engineering Rule

The most important rule of this entire system is:

> **ContextBridge engineering progresses through evidence, not activity.**

An agent cannot say:

> "I implemented it."

and therefore conclude:

> "The checkpoint is complete."

The correct sequence is:

```text
I implemented it.
        ↓
I tested it.
        ↓
The defined acceptance criteria passed.
        ↓
I produced evidence.
        ↓
The required reviewer approved it.
        ↓
The PR was merged.
        ↓
The checkpoint was closed.
```

Only then does the project advance.

---

# 32. System Freeze

This document is now the **FINAL CHECKPOINT AND ENGINEERING-WORK-UNIT SYSTEM** for ContextBridge.

The following are frozen:

* checkpoint hierarchy
* checkpoint dependencies
* acceptance principle
* autonomous-agent boundaries
* GitHub representation
* branch behavior
* PR behavior
* review requirements
* merge requirements
* evidence requirements
* failure handling
* architecture-change handling
* standard checkpoint template
* standard EWU template

The checkpoint system SHALL NOT be materially modified without an explicit **Checkpoint System Change Request** approved by the project owner.

Individual checkpoint execution may discover defects or implementation issues. Such discoveries do not permit silent modification of the checkpoint system.

**END OF FINAL CHECKPOINT AND ENGINEERING-WORK-UNIT SYSTEM**

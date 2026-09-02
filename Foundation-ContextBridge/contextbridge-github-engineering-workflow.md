# FINAL GITHUB ENGINEERING WORKFLOW

## ContextBridge

**Document Type:** Repository & Engineering Workflow Policy
**Status:** FINAL
**Scope:** All ContextBridge development, AI-assisted or human-assisted

---

# 1. Purpose

This document defines how engineering work is represented, reviewed, validated, merged and recorded in GitHub.

The purpose is to ensure that GitHub reflects **real engineering progress**, rather than activity.

The governing principle is:

> **GitHub is the project's engineering record, not its activity scoreboard.**

Therefore:

```text
Commit ≠ progress
PR ≠ progress
Green CI ≠ correctness
Merged code ≠ accepted checkpoint
```

A change becomes meaningful project progress only when its associated requirements and acceptance criteria have been demonstrated.

---

# 2. Repository

The canonical repository is:

[ContextBridge GitHub repository](https://github.com/ORAM15/ContextBridge?utm_source=chatgpt.com)

The repository's primary branch is:

```text
main
```

The repository already contains the initial project baseline.

---

# 3. Repository Structure

The repository SHALL evolve around the following logical structure:

```text
ContextBridge/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── architecture/
│   ├── decisions/
│   ├── mcp/
│   └── validation/
│
├── src/
│   └── <implementation>
│
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   └── security/
│
├── <approved configuration files>
│
└── <approved deployment/configuration artifacts>
```

This is a **logical structure**, not authorization to create every directory immediately.

Directories SHALL be introduced when their corresponding project work requires them.

The repository SHALL NOT be populated with speculative empty structure merely for appearance.

---

# 4. Main Branch

`main` represents the integrated project state.

Rules:

* `main` must remain buildable.
* Broken experimental work must not be merged into `main`.
* Security-sensitive changes require the defined review.
* Checkpoint completion does not occur merely because a PR is merged.

Where practical, direct commits to `main` should be avoided for implementation work.

---

# 5. Branch Strategy

ContextBridge uses a **checkpoint-oriented branch model**.

```text
main
 │
 ├── checkpoint branch
 │       ↓
 │      PR
 │       ↓
 │     review
 │       ↓
 │     merge
 │       ↓
 └──── main
```

One implementation branch SHOULD correspond to one checkpoint.

This prevents unrelated work from accumulating in a single branch.

---

# 6. Branch Naming

Branches SHALL follow:

```text
<type>/cp-<phase>-<number>-<short-name>
```

Examples:

```text
feat/cp-p2-01-mcp-foundation
feat/cp-p2-03-schema-validation
feat/cp-p3-02-authorization
test/cp-p5-02-security-verification
docs/cp-p6-04-final-documentation
```

The checkpoint identifier is mandatory for implementation branches.

---

# 7. Branch Types

Use only meaningful branch types.

### `feat/`

New functionality.

### `fix/`

Correction of an identified defect.

### `test/`

Testing-focused changes.

### `docs/`

Documentation-only work.

### `chore/`

Repository/tooling/maintenance work.

### `refactor/`

Behavior-preserving restructuring.

A branch type must describe the actual work.

---

# 8. Branch Scope

A branch SHALL remain bounded to its checkpoint.

Do not combine:

```text
authorization
+
database redesign
+
frontend
+
deployment
```

into one branch unless the approved checkpoint explicitly requires all of them.

If the work expands beyond the checkpoint:

> stop and report the scope expansion.

---

# 9. Existing Working Tree

Before creating or modifying a branch, the AI agent must inspect:

```text
git status
git branch
git log
git remote -v
```

The agent must understand whether unrelated user changes exist.

It must never overwrite unrelated work.

---

# 10. Commit Philosophy

Commits represent meaningful engineering changes.

A commit should answer:

> **What coherent change does this commit represent?**

Good:

```text
feat(mcp): add tool discovery
feat(auth): enforce role permissions
test(security): reject unauthorized tool calls
docs(mcp): document initialization flow
```

Bad:

```text
update
changes
final
fix
work
try again
test
```

---

# 11. Commit Expectations

A commit SHOULD be:

* logically coherent
* reviewable
* attributable to the active checkpoint
* free of unrelated modifications
* validated where practical

The AI SHALL NOT create commits merely to demonstrate activity.

---

# 12. Empty and Meaningless Commits

The following are prohibited:

* empty commits
* repeated "progress" commits with no meaningful change
* artificial commits to trigger CI
* commits solely to increase contribution activity
* commits whose content does not correspond to the message

---

# 13. Repeated Failed Attempts

Repeated failed attempts without learning are prohibited.

If an implementation attempt fails:

```text
Attempt 1
   ↓
Failure
   ↓
Diagnosis
   ↓
New information
   ↓
Attempt 2
```

The second attempt must be informed by the first failure.

The AI must record:

* what failed
* why it failed
* what changed in its approach
* whether the failure indicates a deeper problem

Blindly repeating the same operation is not engineering progress.

---

# 14. GitHub Issues

Issues represent bounded engineering objectives, decisions, defects or blockers.

They SHALL NOT be created simply to increase repository activity.

---

# 15. Checkpoint-to-Issue Mapping

Each meaningful implementation checkpoint SHOULD have exactly one primary GitHub Issue.

Example:

```text
CP-P3-02
Authorization + RBAC

        ↓

GitHub Issue
#<number>
CP-P3-02 — Authorization + RBAC

        ↓

Implementation branch
feat/cp-p3-02-authorization

        ↓

Pull Request

        ↓

Merge

        ↓

Checkpoint evidence

        ↓

Issue closure
```

---

# 16. Issue Types

Issues may represent:

### Checkpoint

Bounded engineering work.

### Change Request

Proposed modification to requirements, architecture, technology or scope.

### Defect

Observed incorrect behavior.

### Blocker

External or internal condition preventing progress.

### Decision

A decision requiring explicit owner input.

---

# 17. Checkpoint Issue Structure

Every checkpoint Issue SHOULD contain:

```text
# Checkpoint: CP-Px-xx

## Objective

...

## Prerequisites

...

## Allowed Work

...

## Forbidden Work

...

## Acceptance Criteria

- AC-01
- AC-02
- AC-03

## Validation Method

...

## Definition of Done

...

## Dependencies

...

## Human Approval

Required / Not Required

## Completion Evidence

...
```

---

# 18. Issue Closure

An Issue SHALL NOT be closed merely because:

* code exists
* a branch exists
* a PR was opened
* a PR was merged
* CI is green

The issue closes only when the checkpoint acceptance criteria are demonstrated.

---

# 19. Pull Requests

A PR represents a reviewable engineering change.

A PR should normally map to one checkpoint.

The PR is not the checkpoint itself.

```text
Checkpoint
    ↓
Implementation
    ↓
PR
    ↓
Validation
    ↓
Acceptance
    ↓
Merge
    ↓
Checkpoint closure
```

---

# 20. PR Title

PR titles SHOULD identify the checkpoint.

Example:

```text
feat: CP-P3-02 — implement authorization and RBAC
```

or:

```text
test: CP-P5-02 — security verification
```

---

# 21. PR Description

Every meaningful PR SHALL contain:

```text
## Checkpoint

CP-Px-xx

## Objective

...

## What Changed

...

## Why

...

## Acceptance Criteria

- [ ] AC-01
- [ ] AC-02
- [ ] AC-03

## Validation

...

## Evidence

...

## Security Impact

...

## Architecture Impact

None / Change Request required

## API Contract Impact

None / Changed — approved by ...

## Database Schema Impact

None / Changed — approved by ...

## Known Limitations

...

## Related Issue

Closes #<issue>
```

---

# 22. PR Evidence Requirement

A PR is **not mergeable** until it contains sufficient evidence that the acceptance criteria have been satisfied.

Minimum evidence must include, as applicable:

* tests executed
* test results
* build/type-check results
* protocol validation
* integration validation
* security validation
* representative manual demonstration
* deployment evidence
* screenshots/logs where genuinely useful

The evidence must correspond to the actual acceptance criteria.

---

# 23. Validation Before PR

Before requesting review, the AI SHALL run the relevant validation available for the checkpoint.

Depending on the change:

```text
Unit tests
Integration tests
Protocol tests
Security tests
Build
Type checking
Linting
E2E
Manual demonstration
```

Not every checkpoint requires every category.

The checkpoint specification determines what is required.

---

# 24. Test Reporting

The PR must distinguish:

```text
PASS
FAIL
NOT RUN
NOT APPLICABLE
BLOCKED
```

Never convert:

```text
NOT RUN
```

into:

```text
PASS
```

---

# 25. Failed Validation

If validation fails, the PR must not pretend to be ready.

The AI must document:

```text
Failure
Cause
Impact
Attempted fix
Current status
```

If the failure is unresolved:

> PR remains non-mergeable.

---

# 26. Build Requirements

A successful build is necessary where applicable.

But:

> Build success alone is never sufficient evidence of acceptance.

The PR must additionally demonstrate the behavior required by the checkpoint.

---

# 27. Security PR Requirements

Security-sensitive PRs require stronger evidence.

For changes involving:

* authentication
* authorization
* RBAC
* least privilege
* safe output
* audit logging
* secret handling
* security boundaries

the PR SHALL contain:

```text
Security behavior changed
Threat addressed
Positive tests
Negative tests
Authorization evidence
Data exposure considerations
Secret-handling considerations
```

Human review is mandatory.

---

# 28. API Contract Changes

If a PR changes an API or MCP tool contract, the PR must explicitly state:

```text
Old contract
New contract
Reason
Requirement
Approval
Compatibility impact
Tests updated
Documentation updated
```

Without authorization:

> the change is not mergeable.

---

# 29. Database Changes

Any schema modification requires explicit approval.

A database-impacting PR must include:

* schema change
* reason
* migration approach
* affected components
* data compatibility considerations
* validation
* rollback considerations

No silent schema changes.

---

# 30. Architecture Changes

A PR SHALL NOT contain an unapproved architecture change.

If implementation requires architecture modification:

```text
PR
 ↓
STOP
 ↓
Architecture Change Request
 ↓
Owner decision
 ↓
Updated approved architecture
 ↓
Implementation
```

The AI must not smuggle architecture changes into an implementation PR.

---

# 31. Review Requirements

Review depth corresponds to risk.

### Normal changes

Engineering review.

### Security changes

Human security review.

### Architecture changes

Explicit owner approval before implementation.

### Production deployment

Human review.

### Final project acceptance

Human approval.

---

# 32. Reviewer Responsibilities

Reviewers should ask:

### Scope

Is the PR limited to the checkpoint?

### Correctness

Does the implementation satisfy the requirement?

### Security

Can permissions or data protections be bypassed?

### Contracts

Were interfaces changed?

### Regression

Could existing functionality break?

### Evidence

Does the evidence actually prove the claims?

### Maintainability

Can another engineer understand the change?

---

# 33. Merge Requirements

A PR may be merged only when:

```text
Required implementation complete
        +
Required tests pass
        +
Acceptance criteria demonstrated
        +
Evidence recorded
        +
Required review complete
        +
No blocking failure
        +
No unauthorized architectural/contract/schema change
```

Then:

```text
MERGE
```

---

# 34. Merge Does Not Equal Checkpoint Completion

This distinction is mandatory.

```text
PR MERGED
    ↓
Review repository state
    ↓
Verify acceptance evidence
    ↓
Close checkpoint
```

A merged PR may still leave the checkpoint incomplete if acceptance evidence is insufficient.

---

# 35. Main Branch Protection

Where GitHub capabilities permit, `main` should be protected against:

* direct unreviewed implementation changes
* failed required checks
* accidental force-push
* unauthorized history rewriting

The exact GitHub settings are operational configuration and must reflect the repository's actual available features.

---

# 36. Rollback Strategy

If a merged change causes a regression:

```text
Detect
 ↓
Assess severity
 ↓
Protect production if necessary
 ↓
Identify introducing change
 ↓
Revert or corrective fix
 ↓
Validate
 ↓
Document incident
```

For a clearly isolated bad commit, a normal Git revert is preferred over destructive history rewriting.

Example:

```text
git revert <commit>
```

Do not rewrite shared history merely to make the repository look clean.

---

# 37. Security Rollback

For a security regression:

```text
Security regression detected
        ↓
STOP affected release
        ↓
Assess exposure
        ↓
Revert/contain
        ↓
Patch
        ↓
Security validation
        ↓
Human review
        ↓
Redeploy
```

Security takes precedence over development velocity.

---

# 38. Release Strategy

The project does not need frequent formal releases merely for appearance.

A release is appropriate when a meaningful validated product state exists.

Potential structure:

```text
v0.x
```

for milestone/pre-release states and:

```text
v1.0.0
```

for the first formally accepted finished product, if the project owner chooses to use semantic versioning.

Release tagging is therefore **applicable but not required for every checkpoint**.

---

# 39. Release Requirements

A formal release should correspond to:

* required checkpoints complete
* final quality gate passed
* security review complete
* deployment validated
* documentation complete
* known limitations recorded

The tag must identify an actual validated state.

---

# 40. Documentation Updates

Documentation must evolve with implementation.

Documentation changes are required when a change affects:

* architecture
* MCP behavior
* tools
* API contracts
* security behavior
* deployment
* configuration
* user workflow

Do not allow documentation to describe a system that no longer exists.

---

# 41. Decision Records

Important decisions SHALL be captured under:

```text
docs/decisions/
```

Decision records should include:

```text
Decision
Context
Problem
Options considered
Decision made
Reason
Trade-offs
Consequences
Requirements affected
Approval
Date
```

An AI agent may draft a decision record.

The AI SHALL NOT represent a proposal as an approved decision.

---

# 42. MCP Documentation

MCP-specific learning and implementation documentation belongs under:

```text
docs/mcp/
```

It may document:

* protocol concepts
* transport
* initialization
* tool discovery
* invocation
* structured results
* protocol-specific implementation notes

Documentation must distinguish:

```text
Protocol fact
Implementation decision
Project-specific behavior
Open question
```

---

# 43. Architecture Documentation

Architecture artifacts belong under:

```text
docs/architecture/
```

Implementation must remain consistent with the approved architecture.

If implementation diverges:

> Change Request required.

---

# 44. Validation Documentation

Validation evidence belongs under:

```text
docs/validation/
```

where persistent documentation is useful.

Not every raw test log should be committed.

Prefer reproducible validation instructions and concise evidence summaries.

Sensitive operational data must never be committed.

---

# 45. State Tracking

Project state should be represented through:

```text
Checkpoint status
GitHub Issue
Branch
PR
Validation evidence
Review
Merge
Closure
```

The authoritative progression is:

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

---

# 46. State Must Not Be Faked

An AI SHALL NOT mark:

```text
READY
```

if prerequisites are missing.

It SHALL NOT mark:

```text
VALIDATION
```

if implementation is incomplete.

It SHALL NOT mark:

```text
APPROVED
```

without required approval.

It SHALL NOT mark:

```text
CLOSED
```

without evidence.

---

# 47. Green-Dot Farming Prevention

The project explicitly rejects contribution/activity optimization.

The following are prohibited:

* meaningless commits
* artificial documentation changes
* repeated no-op PRs
* empty commits
* unnecessary formatting changes
* speculative files
* duplicate tests with no value
* tiny artificial commits designed solely to increase activity

The project would rather have:

```text
1 meaningful commit
```

than:

```text
20 meaningless commits
```

---

# 48. Huge Change Prevention

Large uncontrolled changes must be decomposed.

If a PR contains changes across unrelated concerns:

```text
STOP
 ↓
Identify logical boundaries
 ↓
Split into checkpoints/EWUs
```

The objective is reviewability and traceability, not an arbitrary line-count limit.

---

# 49. Unrelated Work Prevention

A checkpoint branch must not include unrelated:

* features
* refactors
* dependency upgrades
* documentation campaigns
* formatting sweeps
* architecture experiments

unless explicitly required.

This keeps cause and effect visible.

---

# 50. Failed Attempt Learning Requirement

When a meaningful implementation attempt fails, the AI should leave a useful engineering record.

Example:

```text
Attempt:
Remote MCP connection

Failure:
Transport negotiation failed

Diagnosis:
Server/client transport mismatch

Learning:
Current implementation expects X

Next action:
Use approved transport configuration
```

The next attempt must incorporate the diagnosis.

---

# 51. AI GitHub Operating Rules

An AI agent interacting with GitHub SHALL:

1. inspect repository state first
2. identify the active checkpoint
3. inspect the relevant Issue
4. understand the current implementation
5. create a bounded branch
6. make bounded changes
7. validate
8. report failures
9. prepare evidence
10. create/update the PR
11. request required review
12. merge only when authorized
13. close the checkpoint only after acceptance

---

# 52. AI GitHub Prohibitions

The AI SHALL NOT:

* force-push shared history without explicit authorization
* delete branches containing potentially important work
* overwrite user changes
* close issues falsely
* fabricate test evidence
* fabricate review approval
* merge blocked work
* bypass branch protection
* alter acceptance criteria
* hide failed CI
* create meaningless commits
* create meaningless PRs
* change architecture silently
* change contracts silently
* change schema silently

---

# 53. Evidence Required for Mergeability

A PR is considered **mergeable** only if the following applicable evidence exists:

```text
┌──────────────────────────────────────┐
│ MERGEABILITY EVIDENCE                │
├──────────────────────────────────────┤
│ Checkpoint identified                │
│ Scope matches checkpoint             │
│ Implementation explained             │
│ Acceptance criteria addressed        │
│ Relevant tests executed              │
│ Results recorded                     │
│ Build/type validation where needed   │
│ Security validation where needed     │
│ Integration/E2E where needed         │
│ Documentation updated where needed   │
│ Architecture impact declared         │
│ API impact declared                  │
│ DB impact declared                   │
│ Known limitations documented         │
│ Required human review completed      │
└──────────────────────────────────────┘
```

---

# 54. Final Merge Decision

The reviewer should be able to answer:

> **"What evidence proves this PR satisfies the checkpoint?"**

If the answer is unclear:

> **Do not merge.**

---

# 55. GitHub Engineering State Machine

```text
             ┌─────────────┐
             │    ISSUE    │
             └──────┬──────┘
                    ↓
                 READY
                    ↓
                BRANCH
                    ↓
               IN PROGRESS
                    ↓
                  COMMIT
                    ↓
                   PR
                    ↓
                VALIDATE
              ┌─────┴─────┐
              │           │
            PASS         FAIL
              │           │
              ↓           ↓
            REVIEW      DEBUG
              │           │
              ↓           └──→ VALIDATE
          APPROVED
              ↓
            MERGE
              ↓
      ACCEPTANCE EVIDENCE
              ↓
           CLOSE ISSUE
              ↓
       CHECKPOINT COMPLETE
```

---

# 56. GitHub as Evidence System

The final repository history should allow an engineer to answer:

```text
What changed?
      ↓
Why?
      ↓
Which requirement required it?
      ↓
Which checkpoint authorized it?
      ↓
How was it tested?
      ↓
What evidence exists?
      ↓
Who reviewed it?
      ↓
When was it merged?
```

That traceability is more valuable than contribution volume.

---

# 57. Final Operating Principle

ContextBridge GitHub activity must follow:

> **Issue → bounded work → evidence → review → merge → acceptance → closure.**

Not:

> **code → commit → green CI → "done".**

---

# 58. Policy Freeze

This document is the **FINAL GITHUB ENGINEERING WORKFLOW** for ContextBridge.

It governs:

* repository organization
* branching
* commits
* issues
* checkpoints
* pull requests
* validation
* reviews
* merges
* rollback
* releases
* documentation
* decisions
* state tracking
* AI GitHub behavior

It SHALL NOT be materially modified without an explicit project-owner-approved change request.

**END OF FINAL GITHUB ENGINEERING WORKFLOW**

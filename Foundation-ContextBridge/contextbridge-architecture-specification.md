# FINAL ARCHITECTURE SPECIFICATION

## ContextBridge

**Status:** FINAL
**Architecture baseline:** v1.0
**Authority:** Approved Project Constitution + Requirements & Constraints Specification
**Scope:** Architecture only; no product-scope redesign
**Change policy:** This architecture SHALL NOT be modified without an explicit architectural change request approved by the project owner.

---

# 1. Architectural Intent

ContextBridge is a controlled integration layer between AI clients and external tools/data.

Its fundamental architectural responsibility is:

> **Allow an AI client to request useful operations while ensuring that identity, validation, authorization, data exposure and execution are controlled independently of the model.**

The resulting system is therefore not designed as:

```text
AI
 │
 ▼
Database/API
```

It is designed as:

```text
AI Client
    │
    │ MCP
    ▼
┌────────────────────────────────────────────┐
│                ContextBridge               │
│                                            │
│  Identity → Policy → Validation → Tool     │
│                         │                  │
│                         ▼                  │
│                  Data Filtering             │
│                         │                  │
│                         ▼                  │
│                External Systems             │
│                                            │
│  Audit + Observability operate across      │
│  security-critical execution paths         │
└────────────────────────────────────────────┘
                         │
                         ▼
                 Structured Result
                         │
                         ▼
                    AI Client
```

---

# 2. Architecture Classification Model

Every technology appearing in this specification is classified as one of:

* **APPROVED DECISION** — explicitly established by the project requirements/constitution.
* **PROPOSAL** — previously suggested but not approved; therefore not selected by this architecture.
* **OPEN QUESTION** — required decision remains unresolved.
* **NOT REQUIRED** — architecture does not require it.

This distinction is intentional.

---

# 3. System Context

## 3.1 Context Diagram

```text
                         ┌──────────────────────┐
                         │       End User       │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      AI Client       │
                         │                      │
                         │ LLM + MCP Client     │
                         └──────────┬───────────┘
                                    │
                              MCP interaction
                                    │
                                    ▼
                  ╔══════════════════════════════════╗
                  ║          CONTEXTBRIDGE            ║
                  ║                                  ║
                  ║ Identity / Authentication        ║
                  ║ Authorization / Policy           ║
                  ║ Schema Validation                ║
                  ║ Tool Execution                   ║
                  ║ Safe Data Exposure               ║
                  ║ Error Handling                   ║
                  ║ Audit / Observability            ║
                  ╚══════════════╤═══════════════════╝
                                 │
                    controlled interfaces
                                 │
                  ┌──────────────┴──────────────┐
                  ▼                             ▼
        ┌──────────────────┐          ┌──────────────────┐
        │ External Data    │          │ External Tools   │
        │ / Systems        │          │ / APIs           │
        └──────────────────┘          └──────────────────┘
```

---

# 4. High-Level Architecture

ContextBridge SHALL use a **single controlled application boundary** containing logically separated modules.

The architecture is:

```text
                    ┌───────────────────┐
                    │     AI Client     │
                    │  + MCP Client     │
                    └─────────┬─────────┘
                              │
                         MCP Protocol
                              │
                    ┌─────────▼─────────┐
                    │   MCP Interface   │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │ Request Context   │
                    │ / Identity        │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │ Authorization /   │
                    │ Policy Enforcement│
                    └─────────┬─────────┘
                              │
                       authorized only
                              │
                    ┌─────────▼─────────┐
                    │ Schema Validation │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │   Tool Registry   │
                    └─────────┬─────────┘
                              │
                    ┌─────────▼─────────┐
                    │  Tool Executor    │
                    └─────┬───────┬─────┘
                          │       │
             ┌────────────┘       └─────────────┐
             ▼                                  ▼
      ┌─────────────┐                    ┌─────────────┐
      │ Data Access │                    │ API / Tool  │
      │ Adapter     │                    │ Adapter     │
      └──────┬──────┘                    └──────┬──────┘
             │                                  │
             ▼                                  ▼
      External Data                       External System
             │                                  │
             └────────────┬─────────────────────┘
                          ▼
                   Safe Result Mapping
                          │
                          ▼
                    Structured Result
                          │
                          ▼
                       MCP Client
                          │
                          ▼
                           AI

       ┌─────────────────────────────────────┐
       │ Cross-cutting controls              │
       │                                     │
       │ Audit │ Logging │ Metrics │ Errors  │
       └─────────────────────────────────────┘
```

---

# 5. Architectural Style

## Decision

**Decision:** ContextBridge SHALL be architected as a **single deployable application with internally separated modules/components**, rather than as a collection of independently deployed microservices.

### Why required

The requirements demand:

* controlled tool execution
* authorization
* validation
* auditability
* observability
* deployment
* maintainability
* avoidance of unnecessary complexity.

Nothing in the requirements requires independent service deployment.

### Alternatives considered

**Microservices**

Rejected because no requirement requires independent scaling or independent deployment of components.

**Serverless functions**

Not required by the requirements.

**Fully monolithic/unstructured application**

Rejected because security boundaries and responsibilities must remain explicit.

### Trade-offs

A modular single application is simpler to deploy and understand but provides less independent scaling than microservices.

That trade-off is acceptable because no current requirement establishes a need for independently scaling components.

### Consequences

* Clear internal boundaries are mandatory.
* Independent deployment of each module is not required.
* Additional infrastructure requires a future architectural change if it materially changes this model.

### Requirements satisfied

NFR-002, SCALE-001, MAIN-003, MAIN-006, RES-001–RES-005.

---

# 6. Component Architecture

ContextBridge SHALL contain the following logical components.

```text
┌──────────────────────────────────────────────┐
│                ContextBridge                 │
│                                              │
│ ┌──────────────────────────────────────────┐ │
│ │ MCP Interface                           │ │
│ └────────────────┬─────────────────────────┘ │
│                  ▼                           │
│ ┌──────────────────────────────────────────┐ │
│ │ Request / Identity Context               │ │
│ └────────────────┬─────────────────────────┘ │
│                  ▼                           │
│ ┌──────────────────────────────────────────┐ │
│ │ Authorization / Policy                   │ │
│ └────────────────┬─────────────────────────┘ │
│                  ▼                           │
│ ┌──────────────────────────────────────────┐ │
│ │ Schema Validation                        │ │
│ └────────────────┬─────────────────────────┘ │
│                  ▼                           │
│ ┌──────────────────────────────────────────┐ │
│ │ Tool Registry                            │ │
│ └────────────────┬─────────────────────────┘ │
│                  ▼                           │
│ ┌──────────────────────────────────────────┐ │
│ │ Tool Execution                           │ │
│ └───────────┬─────────────────┬────────────┘ │
│             ▼                 ▼              │
│      Data Adapters       External API        │
│             │             Adapters           │
│             ▼                 ▼              │
│        Data Systems      External Systems     │
│                                              │
│ ┌──────────────────────────────────────────┐ │
│ │ Result Filtering / Mapping               │ │
│ └──────────────────────────────────────────┘ │
│                                              │
│ Cross-cutting: Audit / Errors / Observability│
└──────────────────────────────────────────────┘
```

---

# 7. Component Responsibilities

## 7.1 MCP Interface

Responsible for:

* accepting MCP communication
* exposing available tools
* receiving tool requests
* returning MCP-compatible results/errors
* maintaining the protocol boundary

It SHALL NOT make authorization decisions by itself.

---

## 7.2 Request / Identity Context

Responsible for establishing the security context associated with a request.

It SHALL provide downstream components with the identity information required for authorization.

It SHALL NOT grant permissions.

---

## 7.3 Authorization / Policy Component

Responsible for answering:

> Is this actor allowed to perform this operation?

It SHALL evaluate explicit policy/permissions.

It SHALL execute before protected tool operations.

The model SHALL have no authority to override it.

---

## 7.4 Schema Validation

Responsible for validating tool arguments against the declared tool contract.

Validation SHALL happen before arguments reach the underlying system.

---

## 7.5 Tool Registry

Responsible for defining the controlled capabilities exposed through MCP.

Each tool SHALL have:

* a clear name
* a purpose
* defined parameters
* defined output behavior
* authorization requirements
* error behavior

---

## 7.6 Tool Execution

Responsible for executing an already-authorized, validated operation.

It SHALL NOT independently reinterpret model intent as authorization.

---

## 7.7 Data/API Adapters

Responsible for communicating with underlying systems.

Adapters SHALL prevent underlying system-specific details from becoming the AI's direct interface.

---

## 7.8 Result Filtering / Mapping

Responsible for converting underlying results into safe structured results.

It SHALL prevent unnecessary or sensitive data from being returned.

---

## 7.9 Audit Component

Responsible for recording meaningful sensitive operations.

At minimum, relevant records SHALL support:

* actor
* timestamp
* tool
* request identifier
* authorization decision
* operation/result status
* relevant safe parameters
* error information where applicable

Secrets SHALL NOT be logged.

---

## 7.10 Observability Component

Responsible for exposing:

* request activity
* tool activity
* latency
* failures
* authorization decisions
* request identifiers
* usage information where relevant

---

# 8. Data Architecture

The architecture deliberately does **not** freeze a domain-specific schema.

The requirements do not approve a final domain.

Therefore the logical data model is:

```text
Identity
   │
   ▼
Authorization Context
   │
   ├───────────────┐
   ▼               ▼
Permissions      Tool
   │               │
   └───────┬───────┘
           ▼
       Audit Event
```

Underlying business data remains external to the security/control logic unless the final use case requires ContextBridge-managed storage.

### Architectural rule

ContextBridge SHALL distinguish:

```text
Control-plane data
        │
        ├── identities
        ├── permissions
        ├── policies
        └── audit records

from

Data-plane data
        │
        └── external business/system data
```

This prevents business data from being confused with authorization/audit state.

### Final database technology

**OPEN QUESTION**

The requirements do not mandate a particular database.

PostgreSQL is **PROPOSAL**, not an approved architectural decision.

---

# 9. Application Architecture

The request lifecycle SHALL follow:

```text
Receive request
      │
      ▼
Establish identity/security context
      │
      ▼
Determine requested tool
      │
      ▼
Authorization
      │
      ├──── DENIED ────► structured error + audit
      │
      ▼
Schema validation
      │
      ├──── INVALID ──► structured error + audit
      │
      ▼
Execute controlled tool
      │
      ▼
Retrieve/perform operation
      │
      ▼
Filter/map result
      │
      ▼
Audit operation
      │
      ▼
Return structured result
```

The ordering of security controls is an architectural invariant.

---

# 10. Integration Architecture

ContextBridge SHALL act as an intermediary rather than allowing the AI client to communicate directly with protected external systems.

```text
AI Client
   │
   │ MCP
   ▼
ContextBridge
   │
   │ controlled adapter
   ▼
External System
```

External integrations SHALL be encapsulated behind internal adapters.

This prevents an external API's native interface from becoming an uncontrolled AI interface.

### External integration technology

**OPEN QUESTION**

No specific external system is approved by the current requirements.

GitHub is therefore:

**PROPOSAL**

and not an architectural dependency.

---

# 11. Security Architecture

Security is organized as defense in depth:

```text
                    UNTRUSTED / EXTERNAL
                           │
                           ▼
                 ┌─────────────────────┐
                 │ MCP Interface       │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Identity            │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Authorization       │
                 │ / Policy            │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Input Validation    │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ Controlled Tool     │
                 └──────────┬──────────┘
                            ▼
                 ┌─────────────────────┐
                 │ External System     │
                 └─────────────────────┘
```

The security boundary is **ContextBridge**, not the LLM.

---

# 12. Authentication Architecture

Authentication SHALL exist where required by the deployment/client context.

The architecture requires an abstract identity boundary:

```text
Credential / Identity Evidence
             │
             ▼
      Authentication
             │
             ▼
     Verified Identity
             │
             ▼
    Request Security Context
```

The authentication mechanism itself is not frozen.

### Technology classification

| Technology                     | Classification |
| ------------------------------ | -------------- |
| API keys                       | PROPOSAL       |
| OAuth/OIDC                     | PROPOSAL       |
| Signed tokens                  | PROPOSAL       |
| Sessions                       | PROPOSAL       |
| Final authentication mechanism | OPEN QUESTION  |

No authentication technology is selected merely because it is popular.

The final mechanism SHALL be selected according to the actual MCP client and deployment requirements.

---

# 13. Authorization Architecture

Authorization is mandatory.

The logical model is:

```text
Authenticated Identity
        │
        ▼
Requested Tool
        │
        ▼
Requested Operation
        │
        ▼
Policy Evaluation
        │
   ┌────┴────┐
   ▼         ▼
 ALLOW      DENY
   │         │
   ▼         ▼
Execute    Reject
```

Authorization SHALL occur independently of the model.

### RBAC

Role-based access control is supported by the requirements where appropriate.

However, exact roles and permissions remain unresolved.

Therefore:

**RBAC:** APPROVED ARCHITECTURAL CAPABILITY

**Specific roles:** OPEN QUESTION

**Specific permission names:** OPEN QUESTION

---

# 14. AI / LLM Architecture

ContextBridge SHALL **not** be the LLM itself.

The LLM remains part of the external AI client environment.

```text
┌────────────────────────────────┐
│           AI Client            │
│                                │
│ ┌──────────────┐ ┌───────────┐ │
│ │     User     │ │    LLM    │ │
│ └──────────────┘ └─────┬─────┘ │
│                        │       │
│                    MCP Client  │
└────────────────────────┬───────┘
                         │
                         ▼
                  ContextBridge
```

The LLM may:

* interpret user intent
* decide that a tool may be useful
* generate tool arguments
* consume structured results
* formulate the final response

The LLM SHALL NOT:

* determine its own permissions
* bypass policy
* receive unrestricted database access
* become the authorization authority

### LLM provider

**OPEN QUESTION**

No LLM provider is required by the current architecture.

### AI framework

**NOT REQUIRED**

No AI-agent framework is required by the architectural requirements.

MCP itself is the required AI integration protocol.

---

# 15. Storage Architecture

Storage is divided conceptually into:

```text
                 Storage
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
   Control-plane         External-system
      storage                storage
          │                   │
          ├── identity        ├── business data
          ├── permissions     └── external records
          ├── policies
          └── audit
```

ContextBridge SHALL not duplicate external business data unnecessarily.

### Database technology

**OPEN QUESTION**

The architecture requires persistent storage only where the finalized requirements/use case require it.

PostgreSQL:

**PROPOSAL**

Relational database:

**ARCHITECTURAL OPTION, not technology approval**

---

# 16. API Architecture

There are two distinct interfaces.

## 16.1 MCP interface

This is the AI-facing integration interface.

```text
AI Client
    │
    ▼
MCP
    │
    ▼
ContextBridge
```

It is responsible for:

* tool discovery
* tool invocation
* structured results
* structured errors

## 16.2 Internal/external system interfaces

These are ContextBridge-to-system integrations:

```text
ContextBridge
      │
      ├── data adapter
      ├── API adapter
      └── other controlled adapter
```

These SHALL remain internal implementation interfaces rather than becoming automatically exposed to the AI.

### REST API

**NOT REQUIRED as the product interface.**

An HTTP API may exist if genuinely required by deployment or administration, but the requirements do not establish a general REST API as part of the product.

---

# 17. MCP Protocol Architecture

The MCP boundary SHALL conceptually provide:

```text
Client
   │
   ├── initialize / protocol negotiation
   │
   ├── capability discovery
   │
   ├── tools/list
   │
   └── tools/call
   │
   ▼
Server
```

The architecture recognizes MCP as a protocol layer rather than as a replacement for underlying databases/APIs.

### JSON-RPC

The MCP communication layer SHALL use the protocol semantics required by the selected MCP specification.

JSON-RPC concepts include:

* request
* response
* method
* parameters
* identifier
* error

### JSON-RPC technology classification

**APPROVED DECISION**

Because the project explicitly requires understanding and implementing MCP protocol behavior, including JSON-RPC interaction.

---

# 18. Transport Architecture

The architecture requires an MCP transport appropriate to the actual deployment mode.

The final implementation SHALL distinguish:

```text
Local development
       │
       ▼
appropriate local MCP transport

Remote deployment
       │
       ▼
appropriate remote MCP transport
```

The exact transport SHALL be selected according to the MCP specification applicable when implementation begins and the actual deployment/client requirements.

### Transport technology

**OPEN QUESTION**

No specific transport is frozen in this architecture.

---

# 19. Data Flow Architecture

## Successful operation

```text
User
 │
 ▼
AI Client
 │
 │ tool request
 ▼
MCP Interface
 │
 ▼
Identity Context
 │
 ▼
Authorization
 │
 │ ALLOW
 ▼
Validation
 │
 │ VALID
 ▼
Tool Executor
 │
 ▼
External System
 │
 ▼
Raw Result
 │
 ▼
Safe Result Mapping
 │
 ▼
Structured Result
 │
 ▼
MCP Client
 │
 ▼
AI
 │
 ▼
User
```

## Denied operation

```text
AI Client
    │
    ▼
MCP Interface
    │
    ▼
Identity
    │
    ▼
Authorization
    │
    │ DENY
    ▼
Structured Error
    │
    ├────► Audit Event
    │
    ▼
AI Client
```

Critically:

```text
DENY
 ↓
NO TOOL EXECUTION
 ↓
NO EXTERNAL SYSTEM ACCESS
```

---

# 20. Control Flow Architecture

The control flow SHALL be:

```text
Request
  ↓
Identify
  ↓
Authorize
  ↓
Validate
  ↓
Execute
  ↓
Filter
  ↓
Audit
  ↓
Respond
```

The following ordering is mandatory:

> **Authorization SHALL occur before protected execution.**

Validation SHALL occur before underlying-system invocation.

Safe result transformation SHALL occur before model exposure.

---

# 21. Trust Boundaries

The architecture contains multiple trust boundaries.

## Boundary 1 — User / AI Client

```text
User
 │
 ▼
AI Client
```

The AI client cannot automatically be treated as trusted.

---

## Boundary 2 — AI Client / ContextBridge

```text
AI Client
    │
    ║ TRUST BOUNDARY
    ▼
ContextBridge
```

Requests crossing this boundary SHALL be treated as externally supplied input.

---

## Boundary 3 — ContextBridge / External Systems

```text
ContextBridge
    │
    ║ TRUST BOUNDARY
    ▼
External System
```

External responses SHALL not automatically be treated as safe model context.

---

## Boundary 4 — Retrieved Content / LLM

Retrieved content may contain malicious instructions.

Therefore:

```text
External Data
      │
      ▼
ContextBridge
      │
  controlled result
      │
      ▼
     LLM
```

The data itself does not gain authority over ContextBridge security decisions.

---

# 22. Prompt Injection Architecture

Prompt injection is treated as a **data/content trust problem**, not solely an LLM problem.

The architecture therefore requires:

```text
Untrusted external content
            │
            ▼
       Tool layer
            │
            ▼
      Safe filtering
            │
            ▼
     Structured result
            │
            ▼
           LLM
```

The LLM SHALL NOT be given authority to turn retrieved instructions into privileged operations.

A subsequent tool call must still pass through:

```text
identity
   ↓
authorization
   ↓
validation
   ↓
execution
```

---

# 23. Audit Architecture

Every meaningful sensitive operation SHALL create an audit event.

```text
Request
  │
  ├───────────────► Audit
  │
  ▼
Authorization
  │
  ├───────────────► Audit
  │
  ▼
Execution
  │
  └───────────────► Audit
```

The audit trail SHALL support investigation of:

> "What happened when the AI called this tool?"

Audit records SHALL avoid storing secrets.

---

# 24. Observability Architecture

Observability SHALL cover the complete request lifecycle.

```text
                 Request ID
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
     Request       Tool Call   Authorization
        │            │            │
        └────────────┼────────────┘
                     ▼
                  Execution
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
       Latency                Result
          │                     │
          └──────────┬──────────┘
                     ▼
                   Logs
                   Metrics
                 Audit events
```

The exact observability technology is:

**OPEN QUESTION**

OpenTelemetry:

**PROPOSAL**

No specific observability framework is required by the architecture.

---

# 25. Failure-Handling Architecture

Failures SHALL be contained within the appropriate boundary.

```text
                    Operation
                       │
             ┌─────────┼─────────┐
             ▼         ▼         ▼
          Invalid   Forbidden  External
           Input     Access     Failure
             │         │         │
             └─────────┼─────────┘
                       ▼
                Structured Error
                       │
                       ├──► Audit
                       │
                       └──► Observability
                       │
                       ▼
                   AI Client
```

The system SHALL NOT expose:

* internal stack traces
* credentials
* secrets
* unnecessary infrastructure details

Failure categories SHALL be distinguishable where applicable:

* invalid input
* unauthorized
* forbidden
* not found
* rate limited
* database failure
* upstream failure

---

# 26. Rate Limiting

Rate limiting is architecturally recognized as a control against resource abuse.

However, the exact mechanism is not required by the current architecture.

### Classification

**IMPORTANT architectural capability**

Specific rate-limiting technology:

**OPEN QUESTION**

Redis:

**PROPOSAL**

Redis is therefore **not an approved dependency**.

---

# 27. Internal Interfaces

The following logical interfaces SHALL exist.

## MCP → Request Context

```text
MCP Request
→ Request Context
```

Purpose: establish the security context.

---

## Request Context → Authorization

```text
Identity + Requested Operation
→ Authorization Decision
```

Result:

```text
ALLOW | DENY
```

---

## Authorization → Validation

```text
Authorized Operation
→ Tool Schema Validation
```

---

## Validation → Tool Executor

```text
Validated Arguments
→ Tool Executor
```

---

## Tool Executor → Adapter

```text
Tool Operation
→ External-System Adapter
```

---

## Adapter → Result Mapper

```text
External Result
→ Safe Structured Result
```

---

## All major components → Audit/Observability

```text
Execution event
→ Audit / Observability
```

These interfaces are conceptual contracts; implementation details remain technology-independent.

---

# 28. External Dependencies

The final architecture requires categories of external dependencies rather than particular vendors.

| Dependency                             | Status            | Architectural necessity                                                 |
| -------------------------------------- | ----------------- | ----------------------------------------------------------------------- |
| Compatible MCP AI client               | APPROVED DECISION | Required for intended AI interaction                                    |
| AI/LLM capability                      | APPROVED CONCEPT  | Required for AI-driven workflow                                         |
| Underlying external system/data source | APPROVED CONCEPT  | Required for meaningful integration                                     |
| Authentication authority               | OPEN QUESTION     | Required where authentication architecture needs external identity      |
| Database/storage service               | OPEN QUESTION     | Required only where finalized data requirements need persistent storage |
| Hosting environment                    | OPEN QUESTION     | Required for public deployment                                          |
| External business/API integration      | OPEN QUESTION     | Depends on finalized domain/use case                                    |
| GitHub                                 | PROPOSAL          | Not approved                                                            |
| PostgreSQL                             | PROPOSAL          | Not approved                                                            |
| Redis                                  | PROPOSAL          | Not approved                                                            |
| OpenTelemetry                          | PROPOSAL          | Not approved                                                            |

---

# 29. Technology Classification

## APPROVED DECISION

| Technology / standard         | Reason                              |
| ----------------------------- | ----------------------------------- |
| MCP                           | Core product protocol               |
| JSON-RPC semantics within MCP | Required by MCP interaction         |
| Git                           | Existing repository infrastructure  |
| GitHub                        | Existing project repository hosting |

Note: GitHub as the **source-code repository** is an existing project fact. That does **not** mean GitHub's API is an approved ContextBridge integration.

---

## PROPOSAL

These technologies were previously suggested but are **not selected by this architecture**:

| Technology         | Status   |
| ------------------ | -------- |
| TypeScript         | PROPOSAL |
| Node.js            | PROPOSAL |
| MCP TypeScript SDK | PROPOSAL |
| Zod                | PROPOSAL |
| Fastify            | PROPOSAL |
| PostgreSQL         | PROPOSAL |
| Drizzle            | PROPOSAL |
| GitHub API         | PROPOSAL |
| Vitest             | PROPOSAL |
| Pino               | PROPOSAL |
| OpenTelemetry      | PROPOSAL |
| Redis              | PROPOSAL |
| OAuth/OIDC         | PROPOSAL |

---

## OPEN QUESTION

The following must be resolved before implementation where applicable:

* final programming language/runtime
* exact MCP SDK
* exact transport
* final authentication mechanism
* identity provider
* authorization implementation
* role/permission model
* database technology
* storage provider
* external integration(s)
* deployment provider
* observability implementation
* AI client used for the demonstration
* exact final domain

---

## NOT REQUIRED

The following are not required by the current architecture:

* microservices
* Kubernetes
* Kafka
* unrestricted SQL execution
* a generic REST API as the primary product interface
* an LLM hosted inside ContextBridge
* an AI-agent framework
* a decorative enterprise dashboard
* multiple databases without a requirement
* a separate policy microservice
* a separate authentication microservice
* a separate audit microservice

---

# 30. Deployment Architecture

The final system SHALL be publicly deployable.

Logical deployment:

```text
                 Internet
                    │
                    ▼
          ┌──────────────────┐
          │  Public Endpoint │
          └────────┬─────────┘
                   │
                   ▼
          ┌──────────────────┐
          │   ContextBridge  │
          │     Service      │
          └───────┬──────────┘
                  │
        ┌─────────┼──────────┐
        ▼         ▼          ▼
     Storage   External    Observability
                Systems
```

The exact hosting platform remains an:

**OPEN QUESTION**

The architecture does not require Kubernetes, service meshes, container orchestration or multi-region infrastructure.

---

# 31. Development / Production Separation

The system SHALL distinguish:

```text
Development
    │
    ├── local implementation
    ├── protocol testing
    └── security testing
           
Production
    │
    ├── public deployment
    ├── secure configuration
    ├── protected credentials
    └── real integration
```

Local development SHALL NOT be considered sufficient for final completion.

---

# 32. Testing Architecture

Testing SHALL exist at multiple levels.

```text
                    Testing
                       │
       ┌───────────────┼────────────────┐
       ▼               ▼                ▼
    Unit tests     Integration       Security
                       tests           tests
       │               │                │
       ▼               ▼                ▼
   Schemas          Tools/MCP       Authorization
   Policies         Database        Malicious input
   Error paths      External APIs   Data exposure
                                     Injection
```

## Required test areas

* tool schemas
* authorization
* permissions
* malicious input
* database access where implemented
* tool execution
* error handling
* audit logging
* MCP protocol behavior

Security testing SHALL receive particular emphasis.

---

# 33. Test Architecture Invariant

A successful functional test is insufficient to establish security.

The test suite SHALL demonstrate both:

```text
AUTHORIZED REQUEST
        ↓
     EXECUTES
```

and:

```text
UNAUTHORIZED REQUEST
        ↓
      DENIED
        ↓
  DOES NOT EXECUTE
```

This is a central architectural validation of ContextBridge's purpose.

---

# 34. Major Architectural Invariants

The following SHALL remain true unless an approved architectural change explicitly changes them.

### INV-001 — No unrestricted AI access

AI clients SHALL never receive unrestricted access to protected underlying systems.

### INV-002 — Model is not security authority

The LLM SHALL never be the final authority for authorization.

### INV-003 — Authorization precedes execution

A protected operation cannot execute before authorization succeeds.

### INV-004 — Validation precedes underlying access

Invalid tool arguments SHALL not reach underlying systems.

### INV-005 — Tool boundaries are explicit

Every exposed capability SHALL correspond to a defined tool contract.

### INV-006 — Least privilege

Tools and permissions SHALL expose no more access than required.

### INV-007 — Safe output

Underlying data SHALL be filtered/mapped before exposure to the AI where necessary.

### INV-008 — Sensitive operations are auditable

Meaningful sensitive operations SHALL produce audit records.

### INV-009 — Secrets are never model data

Secrets and credentials SHALL not be returned through AI-facing tool results.

### INV-010 — Failure is explicit

Failed operations SHALL not appear successful.

### INV-011 — MCP remains the AI-facing protocol

The AI-facing integration SHALL be MCP rather than an unrelated custom protocol.

### INV-012 — No unnecessary architecture

Components SHALL exist because requirements justify them.

### INV-013 — No unsupported scale claims

The architecture SHALL not imply enterprise-scale capacity that has not been demonstrated.

---

# 35. Architectural Constraints

The architecture is constrained by the following:

```text
1. Least privilege
2. Independent authorization
3. Model is not a security boundary
4. Validated tool input
5. Safe output
6. Auditability
7. Observability
8. Structured errors
9. Public deployment
10. Professional documentation
11. Security testing
12. Incremental implementation
13. No unnecessary infrastructure
14. No unrestricted SQL
15. No unsupported enterprise claims
```

---

# 36. Major Architectural Decisions Register

## ADR-A01 — ContextBridge is the security/control boundary

**Decision:** All AI-originated protected operations pass through ContextBridge controls.

**Why required:** Core product problem and SEC-004, SEC-013, AI-004.

**Alternatives considered:** Direct AI-to-database/API access.

**Trade-offs:** Adds an intermediary layer and therefore implementation complexity.

**Consequences:** ContextBridge must own validation, authorization and safe execution.

**Requirements satisfied:** FR-009, SEC-004, SEC-013, DATA-001, AI-004.

---

## ADR-A02 — Authorization is external to the model

**Decision:** Authorization is enforced by ContextBridge.

**Why required:** The model cannot be trusted to enforce its own permissions.

**Alternatives considered:** Prompt instructions telling the model which operations are permitted.

**Trade-offs:** Requires explicit policy logic.

**Consequences:** Every protected operation must carry sufficient identity/security context.

**Requirements satisfied:** SEC-004–SEC-006, AI-004, AI-007, NFR-001.

---

## ADR-A03 — Validate tool arguments before execution

**Decision:** Tool arguments are validated before reaching underlying systems.

**Why required:** Prevent malformed and malicious inputs.

**Alternatives considered:** Validation only in downstream systems.

**Trade-offs:** Adds an additional validation stage.

**Consequences:** Tool schemas become security-relevant contracts.

**Requirements satisfied:** FR-005, SYS-003, SEC-007, TEST-001.

---

## ADR-A04 — Use controlled tool interfaces

**Decision:** AI receives narrow tools rather than unrestricted system interfaces.

**Why required:** Least privilege and controlled integration are fundamental requirements.

**Alternatives considered:** Generic query/command interfaces.

**Trade-offs:** More tool definitions may be required for different operations.

**Consequences:** Tool design becomes an important part of the architecture.

**Requirements satisfied:** FR-004, NFR-001, SEC-001, SEC-009, MAIN-001.

---

## ADR-A05 — Filter results before model exposure

**Decision:** External results pass through safe result mapping/filtering before returning to AI.

**Why required:** Underlying systems may contain information the AI should not receive.

**Alternatives considered:** Returning raw database/API responses.

**Trade-offs:** Requires explicit output contracts.

**Consequences:** Each tool must define safe output behavior.

**Requirements satisfied:** SEC-010, SEC-011, DATA-002, DATA-003, AI-006.

---

## ADR-A06 — Audit security-relevant operations

**Decision:** Meaningful sensitive operations are auditable.

**Why required:** The project requires traceability of AI actions.

**Alternatives considered:** Application logs alone.

**Trade-offs:** Requires persistent audit handling and careful data minimization.

**Consequences:** Audit events become part of the system's control plane.

**Requirements satisfied:** REL-004, OBS-005, TEST-008, EVAL-009.

---

## ADR-A07 — Structured failure handling

**Decision:** Errors are categorized and returned without internal implementation leakage.

**Why required:** The AI client needs meaningful failures while secrets/internal details remain protected.

**Alternatives considered:** Raw exceptions/stack traces.

**Trade-offs:** Requires explicit error mapping.

**Consequences:** Tool/API failures must be translated into safe error contracts.

**Requirements satisfied:** FR-008, REL-001–REL-003, EVAL-010.

---

## ADR-A08 — Single deployable application with logical modules

**Decision:** Use one deployment boundary with strong internal separation.

**Why required:** Requirements demand modularity and security but do not justify microservices.

**Alternatives considered:** Microservices.

**Trade-offs:** Less independent scaling, simpler operation.

**Consequences:** Internal interfaces must remain clean even though components share a deployment boundary.

**Requirements satisfied:** NFR-002, SCALE-001, MAIN-006, RES-001–RES-005.

---

## ADR-A09 — AI/LLM remains outside ContextBridge

**Decision:** ContextBridge does not host the model as part of its core architecture.

**Why required:** ContextBridge's purpose is controlled integration, not model serving.

**Alternatives considered:** Embedding an LLM into the platform.

**Trade-offs:** The demo depends on an external AI client.

**Consequences:** AI-client compatibility becomes a deployment/client requirement.

**Requirements satisfied:** AI-001–AI-003, UX-001, FR-001.

---

## ADR-A10 — Technology choices remain requirement-driven

**Decision:** No programming language, database, authentication provider, hosting provider or framework is frozen unless required and explicitly selected.

**Why required:** The Constitution explicitly prohibits technology theater and unapproved decisions.

**Alternatives considered:** Selecting the previously proposed stack immediately.

**Trade-offs:** Implementation cannot begin with every technology already predetermined.

**Consequences:** A technology-selection step is required before implementation.

**Requirements satisfied:** CON-013, CON-014, TECH-003–TECH-007.

---

# 37. Architecture Consistency Audit

The architecture was checked against the requirements specification.

| Requirement area                 | Architecture coverage                       | Status |
| -------------------------------- | ------------------------------------------- | ------ |
| MCP server                       | MCP Interface                               | PASS   |
| Tool discovery                   | MCP Interface / Tool Registry               | PASS   |
| Tool execution                   | Tool Executor                               | PASS   |
| Input validation                 | Schema Validation                           | PASS   |
| Authentication                   | Identity boundary                           | PASS   |
| Authorization                    | Policy component                            | PASS   |
| Least privilege                  | Tool + policy model                         | PASS   |
| Safe output                      | Result Mapping                              | PASS   |
| Structured results               | MCP Result contract                         | PASS   |
| Structured errors                | Error architecture                          | PASS   |
| Audit logging                    | Audit component                             | PASS   |
| Observability                    | Observability component                     | PASS   |
| Prompt injection                 | Trust/data boundary                         | PASS   |
| SQL injection                    | Validation + controlled adapters            | PASS   |
| Secret protection                | Output/audit constraints                    | PASS   |
| Privilege escalation             | Independent policy enforcement              | PASS   |
| Malicious clients                | MCP boundary + authentication/authorization | PASS   |
| Rate abuse                       | Architectural consideration                 | PASS   |
| Testing                          | Layered testing architecture                | PASS   |
| Public deployment                | Deployment architecture                     | PASS   |
| Professional documentation       | Repository requirement                      | PASS   |
| AI-client workflow               | MCP client boundary                         | PASS   |
| Denied operation                 | Authorization flow                          | PASS   |
| No unrestricted SQL              | Tool architecture invariant                 | PASS   |
| No unnecessary microservices     | Single deployable architecture              | PASS   |
| No unsupported enterprise claims | Architectural constraint                    | PASS   |

---

# 38. Consistency Audit Findings

The architecture satisfies the requirements without requiring the previously proposed technologies.

However, the audit identifies the following **deliberately unresolved implementation decisions**:

1. Final product domain.
2. Exact primary user persona.
3. Exact MCP client.
4. Exact MCP specification version to implement against.
5. Exact transport.
6. Authentication mechanism.
7. Identity provider, if required.
8. Exact authorization/RBAC structure.
9. Database technology.
10. External integration(s).
11. Hosting provider.
12. Observability implementation.
13. Numerical performance targets.

These are **not architectural defects**.

They are unresolved inputs that must be selected before their corresponding implementation areas begin.

---

# 39. Architecture Decision Boundary

The following are now architecturally fixed:

```text
AI Client
    ↓
MCP
    ↓
ContextBridge
    ↓
Identity
    ↓
Authorization
    ↓
Validation
    ↓
Controlled Tool
    ↓
External System
    ↓
Safe Result
    ↓
AI
```

Together with:

```text
             ┌─────────────┐
             │     AI      │
             └──────┬──────┘
                    │
                  MCP
                    │
             ┌──────▼──────┐
             │ ContextBridge│
             └──────┬──────┘
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       Policy    Tooling    Audit
          │         │         │
          ▼         ▼         ▼
       Allow?    Adapter   Observability
                    │
                    ▼
              External System
```

These are the core architectural commitments.

---

# 40. Implementation Consequence

Implementation SHALL proceed in dependency order:

```text
Architecture
    ↓
Resolve implementation technologies
    ↓
Resolve final domain/use case
    ↓
Resolve exact MCP/client/transport requirements
    ↓
Define tool contracts
    ↓
Define data contracts
    ↓
Define authentication contract
    ↓
Define authorization model
    ↓
Implement MCP foundation
    ↓
Implement security controls
    ↓
Implement integrations
    ↓
Implement audit/observability
    ↓
Testing
    ↓
Deployment
    ↓
Security validation
```

No substantial implementation should bypass the architectural control sequence.

---

# 41. Final Technology Status

For absolute clarity:

```text
                    TECHNOLOGY STATUS

MCP                         APPROVED DECISION
JSON-RPC                    APPROVED DECISION
Git                         APPROVED DECISION
GitHub repository           APPROVED DECISION

TypeScript                  PROPOSAL
Node.js                     PROPOSAL
MCP TypeScript SDK          PROPOSAL
Zod                         PROPOSAL
Fastify                     PROPOSAL
PostgreSQL                  PROPOSAL
Drizzle                     PROPOSAL
GitHub API                  PROPOSAL
Vitest                      PROPOSAL
Pino                        PROPOSAL
OpenTelemetry               PROPOSAL
Redis                       PROPOSAL
OAuth/OIDC                  PROPOSAL

Final MCP transport         OPEN QUESTION
Final authentication       OPEN QUESTION
Final database              OPEN QUESTION
Final hosting provider      OPEN QUESTION
Final external systems      OPEN QUESTION
Final AI client             OPEN QUESTION
Final domain                OPEN QUESTION
```

This is intentional.

The architecture is **final without pretending unresolved implementation choices are already approved**.

---

# 42. Final Architectural Statement

ContextBridge SHALL be implemented as a controlled MCP integration platform in which an AI client interacts with narrowly defined tools through a ContextBridge security boundary.

Every protected operation SHALL pass through:

```text
Identity
   ↓
Authorization
   ↓
Validation
   ↓
Controlled execution
   ↓
Safe result transformation
```

Security-relevant activity SHALL be auditable and observable.

The model SHALL never be treated as the authorization boundary.

The system SHALL not provide unrestricted database/system access to AI clients.

The architecture SHALL remain deliberately simpler than a distributed enterprise platform unless future requirements explicitly justify additional complexity.

The final implementation SHALL be deployed publicly and demonstrated through a working AI-client workflow including both successful and denied operations.

---

# 43. Architecture Freeze

**This document constitutes the FINAL ARCHITECTURE SPECIFICATION for ContextBridge.**

From this point forward:

> **No architectural component, boundary, invariant, responsibility, or architectural decision defined here SHALL be changed without an explicit Architectural Change Request approved by the project owner.**

Technology-selection decisions that this document explicitly labels **OPEN QUESTION** must be resolved before implementation of the corresponding area, but resolving an open question does not permit changing the architectural principles defined here.

Any future request to alter:

* system boundaries
* security boundaries
* trust boundaries
* request/control flow
* authorization placement
* tool architecture
* data exposure model
* deployment topology
* architectural style
* major component responsibilities
* architectural invariants

must be treated as an **Architectural Change Request**, not an ordinary implementation adjustment.

**END OF FINAL ARCHITECTURE SPECIFICATION**

# TECHNOLOGY AND TOOLING SPECIFICATION

## ContextBridge

**Status:** FINAL
**Purpose:** Define the technology/tooling baseline for implementing the frozen ContextBridge architecture.

**Authoritative inputs:**

1. FINAL PROJECT ARCHITECTURE
2. REQUIREMENTS AND CONSTRAINTS SPECIFICATION
3. Existing repository state

**Technology-selection principle:**

> Requirements determine technologies. Technologies do not determine requirements.

This specification therefore evaluates technologies against the already-frozen architecture rather than redesigning the architecture around available tools.

---

# 1. Technology Status Vocabulary

Every technology/tool is classified as:

### ALREADY CONFIGURED

The tool is already configured in the development environment/account based on the established project context.

This does **not** mean it is automatically approved for ContextBridge implementation.

### AVAILABLE

The tool is accessible/available to the project owner but is not necessarily configured specifically for this repository.

### REQUIRES SETUP

The tool is relevant to the project but requires project-specific configuration before use.

### APPROVED DECISION

The technology is selected as part of the implementation baseline.

### PROPOSAL

The technology is technically suitable but has not been selected as an implementation decision.

### OPEN QUESTION

The architecture requires a decision in this category, but the exact technology cannot yet be selected without resolving an outstanding requirement.

### NOT REQUIRED

The requirements do not justify introducing this technology.

---

# 2. Selection Principles

Technology selection SHALL obey these rules:

1. The frozen architecture cannot be changed merely to accommodate a technology.
2. Every production dependency must have a demonstrated requirement.
3. A tool being installed does not make it an approved project dependency.
4. AI coding tools are development aids, not runtime dependencies.
5. The LLM is external to ContextBridge.
6. Vector search is not introduced unless the final use case requires semantic retrieval.
7. Embeddings are not introduced unless vector/semantic retrieval is actually required.
8. Microservices are not introduced.
9. Unrestricted SQL is not introduced.
10. OAuth is not introduced merely because it is considered advanced.
11. Docker is not automatically required.
12. Redis is not automatically required.
13. The final stack must remain understandable to the developer.
14. Security controls cannot be delegated to AI coding tools or the LLM.
15. The repository remains the technical source of truth.

---

# 3. Programming Language

## Candidates

* TypeScript
* Python
* JavaScript

## Requirement analysis

ContextBridge requires:

* MCP implementation
* typed tool contracts
* schema validation
* maintainable security logic
* structured interfaces
* a production server
* testability
* understandable architecture

A strongly typed language is advantageous because tool schemas, authorization inputs and internal interfaces are security-sensitive.

## TypeScript

**Status:** PROPOSAL

### Purpose

Potential primary implementation language.

### Why needed

Type safety is particularly useful for:

* MCP tool contracts
* request contexts
* authorization structures
* external-system adapters
* structured errors
* audit events

### Alternatives

**Python**

Strong AI ecosystem and excellent backend capabilities, but does not provide the same compile-time type discipline by default.

**JavaScript**

Suitable for Node-based applications but weaker type guarantees.

### Reason for selection

**Not yet selected.**

TypeScript remains the strongest candidate from the evaluated options, but this specification does not convert the previous proposal into an approved decision without owner approval.

### Constraints

The language must support:

* MCP
* structured validation
* asynchronous external-system interaction
* testing
* production deployment

---

# 4. Runtime

## Node.js

**Status:** PROPOSAL

### Purpose

Potential runtime for a TypeScript/JavaScript implementation.

### Why needed

Only required if TypeScript/JavaScript is selected.

### Alternatives

* Python runtime
* Other MCP-compatible runtime environments

### Reason for selection

The previously proposed MCP ecosystem makes Node.js a strong candidate, but no requirement independently mandates it.

### Configuration

Would require:

* supported Node.js version
* package manager
* environment configuration

### Security

Production dependencies must be pinned/managed appropriately and secrets must remain outside source control.

### Cost

No intrinsic licensing cost.

### Operational considerations

Simple deployment footprint relative to a distributed architecture.

**Final status:** PROPOSAL.

---

# 5. MCP Implementation

## MCP SDK

**Status:** PROPOSAL**

### Purpose

Implement the MCP protocol without manually recreating the complete protocol implementation.

### Why needed

MCP is the project's core AI-facing protocol.

The requirements explicitly require:

* MCP server
* tool discovery
* tool invocation
* protocol behavior
* structured results
* structured errors

### Alternatives

* Official SDK for the selected language
* Manual protocol implementation

### Reason for selection

An official SDK is preferable to manually implementing protocol mechanics because it reduces protocol implementation risk.

However, the exact SDK cannot be approved until the programming language is approved.

### Constraints

The SDK version SHALL correspond to the MCP specification applicable when implementation begins.

Outdated tutorials/examples SHALL NOT be treated as authoritative.

### Security

The SDK must not be considered a security boundary. Application-level authorization, validation and safe-output controls remain ContextBridge responsibilities.

**Final status:** PROPOSAL pending language selection.

---

# 6. Schema Validation

## Zod

**Status:** PROPOSAL

### Purpose

Validate tool arguments and potentially other structured application inputs.

### Why needed

Input validation is mandatory.

The architecture requires:

```text
MCP request
     ↓
Authorization
     ↓
Schema validation
     ↓
Tool execution
```

### Alternatives

* Native language validation
* JSON Schema-based validation
* Other runtime schema libraries

### Reason for selection

Zod was previously proposed because it combines runtime validation with strong TypeScript integration.

It is **not approved yet** because TypeScript itself remains unapproved.

### Security considerations

Validation must happen before arguments reach underlying systems.

Validation alone must never be treated as authorization.

### Cost

No mandatory commercial cost.

**Final status:** PROPOSAL.

---

# 7. Backend Framework

## Fastify

**Status:** PROPOSAL

### Purpose

Potential HTTP/application framework if required by the selected MCP transport or deployment architecture.

### Why needed

The architecture requires a remotely deployable MCP service, but does not independently require a specific HTTP framework.

### Alternatives

* Express
* native runtime HTTP facilities
* framework facilities provided by the MCP implementation

### Reason for selection

Fastify was previously proposed for a lightweight production backend.

### Important constraint

A framework SHALL NOT be introduced merely because a conventional REST backend normally uses one.

If the selected MCP implementation provides everything required without a separate framework, another framework may be unnecessary.

**Final status:** PROPOSAL.

---

# 8. Frontend

**Status:** NOT REQUIRED

### Reason

The frozen architecture defines the AI client as the primary user interaction surface.

A decorative enterprise dashboard is explicitly outside scope.

The project does not require a frontend application to satisfy:

* MCP communication
* tool discovery
* authorization
* validation
* auditability
* observability
* deployment

### Consequence

No React/Next.js/Vue frontend is part of the baseline technology stack.

A small demonstration interface may be considered later only if a genuine requirement emerges.

---

# 9. Database

A persistent data store may be required for:

* identities
* permissions
* policies
* audit events
* configuration

However, the requirements do not mandate a particular database technology.

**Status:** OPEN QUESTION

### PostgreSQL

**Status:** PROPOSAL

### Why it is a strong candidate

A relational model naturally represents:

```text
identity
    ↓
roles
    ↓
permissions
    ↓
policies
    ↓
audit events
```

It is also appropriate for structured relational data.

### Alternatives

* SQLite
* another relational database
* managed database service
* document database

### Reason for selection

No final selection has been made.

### Constraints

The selected database must support:

* persistent control-plane data where required
* reliable transactions
* appropriate access controls
* safe query execution
* audit data
* deployment

### Security

The application SHALL never expose unrestricted database access to the AI.

### Final status

**PostgreSQL — PROPOSAL**

**Database technology overall — OPEN QUESTION**

---

# 10. ORM / Database Access Layer

## Drizzle

**Status:** PROPOSAL

### Purpose

Potential typed database access layer.

### Why needed

Only if a database requiring an application data-access abstraction is selected.

### Alternatives

* direct SQL with safe parameterization
* another ORM/query builder
* database-specific client

### Reason for selection

Drizzle was previously proposed because of strong TypeScript integration and relatively lightweight abstraction.

### Constraint

An ORM SHALL NOT be introduced merely to appear more production-grade.

**Final status:** PROPOSAL.

---

# 11. Storage

The architecture distinguishes:

```text
Control-plane storage
        +
External-system data
```

ContextBridge should not unnecessarily duplicate external business data.

## Object/file storage

**Status:** NOT REQUIRED

No current requirement establishes:

* file uploads
* image storage
* large object storage
* document blobs

Therefore S3-style object storage is not part of the baseline.

## Vector storage

**Status:** NOT REQUIRED**

See the dedicated vector section below.

---

# 12. AI Models

The architecture does not require ContextBridge itself to host an LLM.

The model belongs to the external AI-client environment.

Therefore:

**ContextBridge runtime LLM — NOT REQUIRED**

### External LLM

**Status:** OPEN QUESTION**

A compatible AI client and model will eventually be needed for the demonstration, but the architecture does not mandate a particular provider.

Possible providers may include:

* Gemini
* OpenAI
* Anthropic
* another MCP-compatible environment

The provider must not become a ContextBridge runtime dependency unless the finalized implementation actually requires it.

---

# 13. Embedding Models

**Status:** NOT REQUIRED**

### Reason

The frozen requirements do not require:

* semantic document retrieval
* similarity search
* embedding generation
* vector search

The project is fundamentally about **controlled tool/data access**, not a RAG system.

Introducing embeddings would add:

* another model dependency
* additional storage
* additional operational complexity
* additional security/data-flow concerns

without a current requirement.

---

# 14. Vector Database

**Status:** NOT REQUIRED**

Examples such as:

* Pinecone
* Weaviate
* Qdrant
* Milvus
* pgvector

are not required by the current architecture.

A vector database may only become relevant if the final approved use case introduces a genuine semantic retrieval requirement.

That would require a scope/architecture change.

---

# 15. APIs

## MCP

**Status:** APPROVED DECISION**

This is the primary AI-facing protocol.

Required capabilities include:

* connection/protocol initialization
* capability discovery
* tool discovery
* tool invocation
* structured results
* structured errors

## External APIs

**Status:** OPEN QUESTION**

The architecture requires controlled adapters to external systems where the final use case needs them.

No specific external API is approved.

## GitHub API

**Status:** PROPOSAL**

GitHub is an approved source-code repository but GitHub API access by ContextBridge is **not** an approved product integration.

It must not be silently treated as one.

---

# 16. Authentication

Authentication is an architectural requirement where the deployment/client context requires it.

The exact mechanism remains:

**OPEN QUESTION**

Candidates:

| Mechanism     | Status   |
| ------------- | -------- |
| API keys      | PROPOSAL |
| OAuth/OIDC    | PROPOSAL |
| Signed tokens | PROPOSAL |
| Sessions      | PROPOSAL |

### Selection rule

The authentication mechanism SHALL be selected based on:

* actual MCP client requirements
* remote deployment model
* identity requirements
* security needs
* implementation complexity

OAuth SHALL NOT be selected merely because it is considered more advanced.

---

# 17. Authorization

**Status:** APPROVED ARCHITECTURAL CAPABILITY**

The authorization layer is mandatory.

The technology implementing it is:

**OPEN QUESTION**

The architecture requires:

```text
Identity
   ↓
Requested tool/operation
   ↓
Policy evaluation
   ↓
ALLOW / DENY
```

RBAC is an approved architectural capability where appropriate.

### Exact roles

**OPEN QUESTION**

### Exact permissions

**OPEN QUESTION**

### Authorization library

**OPEN QUESTION**

No particular authorization package is currently required.

---

# 18. Rate Limiting

**Status:** IMPORTANT REQUIREMENT / TECHNOLOGY OPEN QUESTION**

Rate limiting is relevant because rate abuse and denial-of-service are explicit threats.

However, no specific implementation has been selected.

## Redis

**Status:** PROPOSAL**

Redis SHALL NOT be introduced merely to implement a conventional rate limiter.

If the selected deployment requires persistent/distributed rate limiting that cannot reasonably be implemented otherwise, its inclusion can be evaluated.

---

# 19. Testing

Testing is mandatory.

The architecture requires testing of:

* schemas
* authorization
* permissions
* malicious input
* database access
* tool execution
* errors
* audit logging
* MCP behavior

## Testing framework

**Status:** OPEN QUESTION**

### Vitest

**Status:** PROPOSAL**

### Alternatives

* Jest
* language-native testing tools
* another appropriate test framework

### Selection criterion

The framework must support the selected language/runtime and allow unit, integration and security-focused testing.

The framework itself is not a security control.

---

# 20. CI/CD

## Continuous Integration

**Status:** REQUIRES SETUP**

The repository should automatically verify changes before merging.

The minimum conceptual pipeline is:

```text
Pull Request
     ↓
Install dependencies
     ↓
Static/type checks
     ↓
Unit tests
     ↓
Integration tests
     ↓
Security tests
     ↓
Build
     ↓
PASS / FAIL
```

### GitHub Actions

**Status:** AVAILABLE / REQUIRES PROJECT SETUP**

GitHub is already the project's source-code repository.

GitHub Actions is therefore a natural candidate for CI, but it has not yet been approved as the project's CI implementation.

**Classification:** PROPOSAL

### Alternatives

* GitHub Actions
* another CI provider
* deployment platform's built-in CI

### Selection criterion

Use the simplest CI mechanism that satisfies testing and deployment requirements.

---

# 21. Deployment

Public deployment is:

**APPROVED DECISION**

Specific provider:

**OPEN QUESTION**

The final deployment must provide:

```text
Public MCP endpoint
       ↓
ContextBridge service
       ↓
Persistent storage where required
       ↓
External integrations
```

### Containerization

**Status:** OPEN QUESTION**

Docker is not intrinsically required.

A deployment platform that can run the selected application directly may eliminate the need for Docker.

### Docker

**Status:** PROPOSAL**

Docker may be selected if it materially simplifies reproducible deployment or local environment consistency.

It SHALL NOT be introduced solely because production projects commonly use containers.

---

# 22. Monitoring

Monitoring is part of the observability objective.

Required information includes:

* requests
* tool calls
* latency
* errors
* authorization decisions
* request identifiers
* usage where relevant

Specific monitoring provider:

**OPEN QUESTION**

Third-party monitoring is not mandatory if the selected deployment environment provides sufficient capabilities.

---

# 23. Observability

## Logging

**Status:** APPROVED CAPABILITY**

Structured application logging is required.

### Pino

**Status:** PROPOSAL**

Pino was previously proposed for structured logging in a Node.js implementation.

It is not currently approved because Node.js itself remains a proposal.

### Alternatives

* runtime-native logging
* another structured logging library
* platform-provided structured logging

---

## Metrics

**Status:** APPROVED CAPABILITY**

Metrics relevant to:

* request count
* tool calls
* errors
* latency
* authorization outcomes

must be available.

Specific metrics technology:

**OPEN QUESTION**

---

## Distributed tracing

**Status:** OPTIONAL**

### OpenTelemetry

**Status:** PROPOSAL**

Tracing can be valuable but is not independently required to satisfy the core product.

It should be introduced only if it materially improves observability of the final deployment.

---

# 24. Security Tooling

Security testing is mandatory.

The exact security tooling remains dependent on the selected implementation language/runtime.

Required categories include:

```text
Dependency scanning
        +
Input/security tests
        +
Authorization tests
        +
Injection tests
        +
Secret protection
        +
Configuration checks
```

No specific commercial security product is currently required.

---

# 25. Secret Management

**Status:** APPROVED CAPABILITY**

Secrets SHALL:

* remain outside source control
* not appear in AI-facing responses
* not appear in audit logs
* not be hardcoded
* be supplied through appropriate environment/configuration mechanisms

The exact production secret-management service:

**OPEN QUESTION**

The repository already excludes `.env` files from version control.

---

# 26. Documentation

Documentation is mandatory.

The repository must eventually contain:

```text
README
Architecture
MCP explanation
Protocol flow
Tool catalog
Security model
Permission model
Setup
Environment configuration
Testing
Deployment
Threat model
Limitations
Roadmap
```

## Markdown

**Status:** ALREADY CONFIGURED / APPROVED**

Markdown is already used by the repository and is appropriate for project documentation.

## Documentation generation tools

**Status:** NOT REQUIRED**

No special documentation platform is required.

---

# 27. Design Tools

Design tooling is not part of the runtime architecture.

## Canva

**Status:** AVAILABLE**

Can be used for:

* presentation material
* architecture visuals
* portfolio material
* training/demo communication

### Why needed

Useful for communicating the project, but not required for the software itself.

**Project runtime status:** NOT REQUIRED

**Development/communication status:** AVAILABLE

---

# 28. AI Development Tools

AI coding tools are development accelerators, not ContextBridge runtime dependencies.

The project may use them to:

* research
* generate scaffolding
* inspect code
* refactor
* test
* document
* review

But generated code remains subject to developer understanding and repository review.

---

# 29. Jules

**Status:** AVAILABLE**

### Purpose

AI-assisted software-engineering workflow.

Potential uses:

* repository-level coding tasks
* refactoring
* issue implementation
* code changes
* review assistance

### Why needed

Potentially useful for accelerating development.

### Architecture status

**NOT REQUIRED**

Jules is not part of the ContextBridge runtime or production architecture.

### Security

Never give an AI coding agent unnecessary production secrets.

Repository permissions should follow least privilege.

---

# 30. Gemini

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Potential:

* technical research
* reasoning
* code assistance
* architecture review
* MCP experimentation
* documentation
* demo/client experimentation

### Architecture status

**NOT REQUIRED as a ContextBridge runtime dependency**

Gemini may be the model behind a demonstration client, but that does not make Gemini part of ContextBridge itself.

### Security

Project secrets and production credentials must not be exposed unnecessarily to external AI tools.

---

# 31. Gemini CLI

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Terminal-based AI-assisted development.

Potential uses:

* repository exploration
* coding
* testing
* debugging
* documentation
* Git operations with explicit review

### Architecture status

**NOT REQUIRED**

It is a development tool, not a runtime dependency.

### Operational constraint

AI-generated changes must be reviewed before being considered authoritative.

---

# 32. Antigravity

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

AI-assisted development workflow.

Potential uses:

* implementation assistance
* project exploration
* coding
* debugging
* refactoring

### Architecture status

**NOT REQUIRED**

It is not part of the deployed ContextBridge system.

---

# 33. Google AI Studio

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Potential:

* model experimentation
* prompt testing
* AI behavior exploration
* prototype/demo-client experimentation

### Why useful

It can help test the AI side of the system before integrating a particular client workflow.

### Architecture status

**NOT REQUIRED**

Google AI Studio is not a ContextBridge runtime dependency.

### Security

API keys must not be committed to GitHub or exposed through tool results.

---

# 34. Stitch

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Potential UI/design exploration.

### Why needed

Not required by the frozen product because ContextBridge does not require a frontend.

### Status

**NOT REQUIRED**

It may be used for supporting visual communication but does not belong in the software architecture.

---

# 35. Opal

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Potential rapid AI application/prototype experimentation.

### ContextBridge relevance

No requirement currently requires Opal.

### Status

**NOT REQUIRED**

---

# 36. NotebookLM

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

Potential:

* studying MCP documentation
* synthesizing technical references
* learning protocol concepts
* organizing research material

### Architecture status

**NOT REQUIRED**

It is a learning/research tool, not a project runtime dependency.

---

# 37. Google Flow

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

### Purpose

General visual/media creation.

### ContextBridge relevance

No software requirement requires it.

### Status

**NOT REQUIRED**

---

# 38. Google Workspace

**Status:** AVAILABLE / ALREADY CONFIGURED IN DEVELOPMENT ECOSYSTEM**

Potential uses:

* documentation
* project planning
* presentation preparation

### Architecture status

**NOT REQUIRED**

The GitHub repository remains the project's technical source of truth.

---

# 39. GitHub

## Repository

**Status: ALREADY CONFIGURED / APPROVED DECISION**

Repository:

```text
ORAM15/ContextBridge
```

Local:

```text
D:\BRDR\Development\Active Projects\ContextBridge
```

Branch:

```text
main
```

### Purpose

* source control
* project history
* documentation
* collaboration
* release/deployment integration

### Security

Repository access must follow least privilege.

Secrets SHALL NOT be committed.

---

# 40. Git

**Status: ALREADY CONFIGURED / APPROVED DECISION**

### Purpose

Version control.

### Current state

The baseline has already been:

* initialized
* committed
* reconciled with remote history
* pushed
* synchronized

### Current baseline

```text
4801420 chore: reconcile remote repository history
```

### Constraint

Implementation changes SHALL be committed incrementally and meaningfully.

---

# 41. GitHub API

**Status: PROPOSAL**

GitHub itself is the repository platform.

GitHub's API is a separate matter.

It SHALL only become a ContextBridge integration if the finalized product use case requires GitHub operations.

It is therefore not currently an approved runtime dependency.

---

# 42. GitHub Actions

**Status: PROPOSAL / REQUIRES SETUP**

### Purpose

Potential CI/CD mechanism.

### Why needed

Automated verification supports:

* testing
* build validation
* security checks
* deployment

### Alternatives

* deployment-platform CI
* another CI service

### Reason for selection

GitHub is already the repository platform, making GitHub Actions operationally attractive.

### Final status

**PROPOSAL**

---

# 43. Package Management

The package manager depends on the final programming language/runtime.

### npm

**Status:** PROPOSAL

### pnpm

**Status:** PROPOSAL

### yarn

**Status:** PROPOSAL

No package manager is currently approved.

The final choice should prioritize reproducibility and simplicity.

---

# 44. Environment Management

The project SHALL separate:

```text
Source code
     ≠
Secrets
     ≠
Environment configuration
```

The existing `.gitignore` already excludes:

```text
.env
.env.*
```

while permitting:

```text
.env.example
```

### `.env.example`

**Status:** APPROVED PRACTICE**

It should document required configuration keys without containing real secrets.

---

# 45. API Documentation

The primary AI-facing interface is MCP.

A conventional REST API documentation system is therefore:

**NOT REQUIRED**

unless a separate administrative/API requirement emerges.

Tool schemas and MCP behavior must nevertheless be documented.

---

# 46. Security Scanning

Security scanning is required conceptually.

Potential categories:

```text
Dependency vulnerabilities
Secret detection
Static analysis
Container scanning, if containers are used
Authorization tests
Injection tests
```

Specific scanners:

**OPEN QUESTION**

No particular security vendor/tool is approved.

---

# 47. Local Development Environment

The local environment must eventually support:

```text
Source repository
       ↓
Selected runtime
       ↓
MCP implementation
       ↓
ContextBridge
       ↓
Local test data/services
       ↓
MCP-compatible client
```

Exact installation requirements cannot yet be finalized because the programming language/runtime remain open.

---

# 48. Production Environment

The production environment must eventually support:

```text
Internet
   ↓
Public MCP endpoint
   ↓
ContextBridge
   ├── Authentication
   ├── Authorization
   ├── Validation
   ├── Tool execution
   ├── Safe output
   ├── Audit
   └── Observability
        │
        ├── Storage
        └── External systems
```

Provider-specific configuration remains an OPEN QUESTION.

---

# 49. Technology Dependency Rules

The following dependency rules are mandatory.

### Rule 1

An AI development tool SHALL NOT become a runtime dependency merely because it was used to write the code.

### Rule 2

An AI model SHALL NOT be trusted as an authorization mechanism.

### Rule 3

A database SHALL NOT be exposed directly to the AI.

### Rule 4

An external API SHALL be exposed through controlled tools/adapters.

### Rule 5

A technology SHALL be removed if the final requirements do not justify it.

### Rule 6

A new production dependency requires architectural justification.

### Rule 7

Security controls SHALL remain enforceable independently of AI coding tools.

---

# 50. Cost Model

The project should prioritize low-cost development and deployment.

## No mandatory paid tooling is currently established.

Potential future costs may include:

* AI model/API usage
* hosting
* managed database
* monitoring
* authentication provider

But no specific commercial provider is approved.

### Cost principle

> Do not pay for infrastructure merely to make the project look more production-grade.

---

# 51. Operational Model

The final operational system must allow an operator/developer to understand:

```text
Who made the request?
        ↓
What tool was requested?
        ↓
Was authorization granted?
        ↓
Was validation successful?
        ↓
What system was contacted?
        ↓
What happened?
        ↓
What result/error returned?
```

This is achieved through the combined:

* authentication
* authorization
* validation
* audit
* logging
* observability

architecture.

---

# 52. Tooling Separation

A strict distinction SHALL be maintained:

```text
                  CONTEXTBRIDGE

┌─────────────────────────────────────────┐
│             Runtime Stack               │
│                                         │
│ MCP + application + security + storage  │
│ + external integrations                 │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│          Development Toolchain          │
│                                         │
│ Git + GitHub + AI coding tools + CI     │
└─────────────────────────────────────────┘

┌─────────────────────────────────────────┐
│         Research / Communication        │
│                                         │
│ Gemini + AI Studio + NotebookLM +       │
│ Canva + Stitch + Flow + Workspace       │
└─────────────────────────────────────────┘
```

Development and research tools SHALL NOT be confused with production dependencies.

---

# 53. Complete Tool Status Register

| Category                | Technology/tool    | Status                        |
| ----------------------- | ------------------ | ----------------------------- |
| Protocol                | MCP                | APPROVED DECISION             |
| Protocol                | JSON-RPC           | APPROVED DECISION             |
| Source control          | Git                | ALREADY CONFIGURED / APPROVED |
| Repository              | GitHub             | ALREADY CONFIGURED / APPROVED |
| Language                | TypeScript         | PROPOSAL                      |
| Runtime                 | Node.js            | PROPOSAL                      |
| MCP SDK                 | Official SDK       | PROPOSAL                      |
| Validation              | Zod                | PROPOSAL                      |
| Backend framework       | Fastify            | PROPOSAL                      |
| Frontend                | React/Next/etc.    | NOT REQUIRED                  |
| Database                | PostgreSQL         | PROPOSAL                      |
| ORM                     | Drizzle            | PROPOSAL                      |
| Vector DB               | Any                | NOT REQUIRED                  |
| Embeddings              | Any                | NOT REQUIRED                  |
| LLM inside server       | Any                | NOT REQUIRED                  |
| External LLM            | Gemini/OpenAI/etc. | OPEN QUESTION                 |
| Authentication          | Exact mechanism    | OPEN QUESTION                 |
| OAuth/OIDC              | OAuth/OIDC         | PROPOSAL                      |
| Authorization           | Policy layer       | APPROVED CAPABILITY           |
| RBAC                    | RBAC               | APPROVED CAPABILITY           |
| Testing                 | Testing framework  | OPEN QUESTION                 |
| Vitest                  | Vitest             | PROPOSAL                      |
| CI                      | GitHub Actions     | PROPOSAL / REQUIRES SETUP     |
| Deployment              | Hosting provider   | OPEN QUESTION                 |
| Docker                  | Docker             | PROPOSAL                      |
| Redis                   | Redis              | PROPOSAL                      |
| Logging                 | Structured logging | APPROVED CAPABILITY           |
| Pino                    | Pino               | PROPOSAL                      |
| Metrics                 | Metrics system     | OPEN QUESTION                 |
| Tracing                 | OpenTelemetry      | PROPOSAL                      |
| Security scanning       | Specific tool      | OPEN QUESTION                 |
| Documentation           | Markdown           | ALREADY CONFIGURED / APPROVED |
| Design                  | Canva              | AVAILABLE                     |
| AI development          | Jules              | AVAILABLE                     |
| AI development          | Gemini CLI         | AVAILABLE                     |
| AI development          | Antigravity        | AVAILABLE                     |
| AI research/development | Gemini             | AVAILABLE                     |
| AI experimentation      | Google AI Studio   | AVAILABLE                     |
| UI design               | Stitch             | AVAILABLE                     |
| AI prototyping          | Opal               | AVAILABLE                     |
| Research                | NotebookLM         | AVAILABLE                     |
| Visual/media            | Google Flow        | AVAILABLE                     |
| Productivity            | Google Workspace   | AVAILABLE                     |

---

# 54. Required vs Optional Technology Map

## Required by architecture

```text
MCP
JSON-RPC
Git
GitHub
Structured validation capability
Authentication capability where required
Authorization/policy capability
Controlled tool execution
Persistent audit capability
Observability capability
Testing capability
Public deployment capability
```

Notice that several of these are **capabilities**, not specific technologies.

---

## Strong candidates but not yet approved

```text
TypeScript
Node.js
Official MCP TypeScript SDK
Zod
PostgreSQL
Drizzle
Fastify
Vitest
GitHub Actions
Pino
OpenTelemetry
Docker
```

These remain candidates until the technology-selection decision is explicitly made.

---

## Explicitly unnecessary at the current scope

```text
Frontend application
Vector database
Embedding model
RAG pipeline
LLM hosted inside ContextBridge
Microservices
Kubernetes
Kafka
Unrestricted SQL interface
AI-agent framework
Decorative enterprise dashboard
```

---

# 55. Tool Selection Matrix

The implementation technology decision should eventually be made by evaluating the strongest candidates against:

| Criterion               | Importance |
| ----------------------- | ---------: |
| MCP support             |   Critical |
| Type/schema safety      |       High |
| Security                |   Critical |
| Maintainability         |   Critical |
| Testing support         |       High |
| Deployment simplicity   |       High |
| Developer understanding |   Critical |
| Operational complexity  |       High |
| Cost                    |     Medium |
| Ecosystem maturity      |     Medium |
| Unnecessary abstraction |   Negative |

No technology should win merely because it has the largest ecosystem.

---

# 56. Installation / Configuration Boundary

Because the exact runtime has not yet been approved, the following are deliberately **not yet installation instructions**:

```text
Node.js version
Python version
package manager
MCP SDK package
database CLI
ORM
test framework
deployment CLI
container runtime
```

These become concrete setup requirements only after the corresponding technology decisions are approved.

The current repository itself requires only the already-established Git/GitHub workflow.

---

# 57. Security Rules for AI-Assisted Development

AI development tools including Jules, Gemini CLI, Antigravity, Gemini and Google AI Studio SHALL be treated as development assistants rather than trusted project operators.

The following rules apply:

```text
AI tool
   ↓
generates/recommends change
   ↓
developer reviews change
   ↓
tests execute
   ↓
Git diff reviewed
   ↓
commit
```

AI-generated code SHALL NOT automatically become authoritative.

Production secrets SHALL NOT be unnecessarily exposed to AI development environments.

---

# 58. Final Technology Decision Boundary

The architecture is frozen, but the following implementation selections still require explicit resolution:

```text
1. Programming language
2. Runtime
3. MCP SDK
4. Validation technology
5. Database technology
6. Database access layer
7. Authentication mechanism
8. Exact RBAC implementation
9. MCP transport
10. AI demonstration client
11. External integration
12. Hosting provider
13. Testing framework
14. CI/CD implementation
15. Observability implementation
```

These are implementation selections, not permission to redesign the architecture.

---

# 59. TOOLCHAIN MAP

The final toolchain is represented at four layers.

```text
                         CONTEXTBRIDGE
                              │
             ┌────────────────┴────────────────┐
             │                                 │
       PRODUCTION                        DEVELOPMENT
             │                                 │
             ▼                                 ▼
      ┌──────────────┐                  ┌──────────────┐
      │ MCP /        │                  │ Git          │
      │ JSON-RPC     │                  │ GitHub       │
      └──────┬───────┘                  └──────┬───────┘
             │                                 │
             ▼                                 ▼
      ┌──────────────┐                  ┌──────────────┐
      │ ContextBridge│                  │ CI/CD        │
      │ Service      │                  │ tooling      │
      └──────┬───────┘                  └──────┬───────┘
             │                                 │
     ┌───────┼────────┐                 ┌──────┼─────────┐
     ▼       ▼        ▼                 ▼      ▼         ▼
 Identity  Policy  Validation         Jules  Gemini   Antigravity
     │       │        │
     └───────┼────────┘
             ▼
       Controlled Tools
             │
      ┌──────┴───────┐
      ▼              ▼
   Storage       External APIs
      │              │
      └──────┬───────┘
             ▼
       Safe Results
             │
             ▼
          AI Client
             │
             ▼
            LLM


       RESEARCH / COMMUNICATION
       ────────────────────────

       Gemini
       Gemini CLI
       Google AI Studio
       NotebookLM
       Canva
       Stitch
       Opal
       Google Flow
       Google Workspace
```

---

# 60. Definitive Status

## Already configured

* Git
* GitHub repository
* Markdown documentation baseline
* Existing local ContextBridge repository
* Existing development ecosystem containing Gemini, Gemini CLI, Antigravity, Jules, Google AI Studio, NotebookLM, Stitch, Flow and Workspace

## Available

* Jules
* Gemini
* Gemini CLI
* Antigravity
* Google AI Studio
* Stitch
* Opal
* NotebookLM
* Google Flow
* Google Workspace
* Canva

Availability here refers to the established development ecosystem, **not project-specific production configuration**.

## Requires setup

* Project CI/CD
* production deployment
* production database if required
* production authentication
* production observability
* project-specific secrets/configuration
* selected runtime/toolchain

## Proposed

* TypeScript
* Node.js
* official MCP TypeScript SDK
* Zod
* Fastify
* PostgreSQL
* Drizzle
* Vitest
* GitHub Actions
* Pino
* OpenTelemetry
* Redis
* Docker
* OAuth/OIDC
* GitHub API

## Open questions

* final programming language/runtime
* final MCP SDK
* final MCP transport
* final database
* final authentication mechanism
* final authorization implementation
* final AI client
* final external integration
* final hosting provider
* final testing framework
* final observability implementation

## Not required

* frontend application
* embedding model
* vector database
* RAG infrastructure
* LLM hosted inside ContextBridge
* unrestricted SQL
* microservices
* Kubernetes
* Kafka
* AI-agent framework
* fake enterprise dashboard

---

# 61. Final Toolchain Principle

The project does **not** have the following logic:

```text
"We have TypeScript + PostgreSQL + Fastify,
therefore let's find something to build with them."
```

It has the opposite logic:

```text
Requirements
     ↓
Frozen architecture
     ↓
Required capabilities
     ↓
Technology evaluation
     ↓
Minimal suitable toolchain
     ↓
Implementation
```

This distinction is mandatory for ContextBridge.

---

# 62. Technology Specification Status

This document establishes the **technology-selection framework and current status**, while preserving the open implementation decisions that the approved architecture intentionally left unresolved.

The following are already fixed:

> **MCP + JSON-RPC + Git + GitHub + controlled tool architecture + independent authorization + validation + safe output + auditability + observability + testing + public deployment.**

The following are **not yet fixed**:

> **language, runtime, SDK, database, authentication mechanism, transport, hosting provider, testing framework, external integration and concrete observability stack.**

No later implementation activity may silently promote a PROPOSAL or OPEN QUESTION into an approved production dependency.

A technology becomes an **APPROVED DECISION** only when it is explicitly selected through the project's technology-decision process.

**END OF TECHNOLOGY AND TOOLING SPECIFICATION**

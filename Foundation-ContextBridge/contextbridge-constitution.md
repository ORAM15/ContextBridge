PROJECT CONSTITUTION — DRAFT

Forensic extraction of the ContextBridge project state established in this conversation

Status: Draft / pre-architecture
Purpose: Preserve what has actually been established without silently turning proposals into approved decisions.

Method: This document distinguishes what was explicitly stated or subsequently confirmed from recommendations that were merely proposed. Where something was recommended but the user never explicitly approved it, it remains PROPOSAL. Where the conversation contains tension or evolution, it is recorded rather than silently resolved.

1. Project identity

Classification: FACT

The project is named ContextBridge.

Its intended identity is a secure enterprise MCP server/platform that allows AI clients to interact with controlled external data and tools.

The central concept established in the master instruction is:

AI systems should be able to use real tools and data, but they must not receive unrestricted access to the underlying systems.

The project is intended to become more than an MCP tutorial: a credible, deployed, security-conscious AI integration platform.

2. Problem being solved

Classification: FACT

The fundamental problem explicitly defined is:

How can organizations allow AI systems to interact with internal systems without simply handing the AI unrestricted database/API access?

The project therefore focuses on:

standardized tool interfaces
authentication
authorization
validation
least privilege
auditability
safe data exposure
structured responses
observability
3. Target users
Primary user

Classification: FACT

The master instruction identifies the intended user as someone using a compatible AI client to interact with controlled organizational data/tools.

The later architecture discussion proposed a more specific primary user:

An engineering/support team member using an AI assistant to investigate and act on operational information.

However, that narrower engineering/support persona was not explicitly approved by the user.

Classification of narrower persona: PROPOSAL

4. Intended user experience

Classification: FACT

The intended experience is:

User
  ↓
AI client
  ↓
ContextBridge
  ↓
Authentication / Authorization / Policy
  ↓
Validated tool
  ↓
Database / API / external system
  ↓
Structured result
  ↓
AI
  ↓
User

The user should be able to connect a compatible AI client to ContextBridge.

The AI client should discover available tools.

The AI should determine when to call them.

ContextBridge should control the resulting operation.

The master instruction gives an example involving:

search_customer
get_customer
search_orders
get_order

The later proposal replaced this illustrative customer/order example with engineering operations.

That replacement remains PROPOSAL, not an approved product requirement.

5. Core product vision

Classification: FACT

The core vision is to transform ContextBridge from:

"a student MCP project"

into:

a credible, deployed, security-conscious AI integration platform demonstrating protocol understanding, typed tool design, authorization, safe data access, observability and production engineering.

The portfolio message should emphasize the integration/security problem rather than merely claiming:

"I built an MCP server."

The master instruction proposes positioning it more like:

"I built a secure protocol layer that lets AI agents interact with enterprise tools and data using typed, permission-aware interfaces."

6. Primary objectives

Classification: FACT

The explicitly established primary objectives are to build a system that:

Implements MCP correctly.
Exposes useful, controlled tools.
Allows AI clients to discover and invoke those tools.
Validates tool inputs.
Controls access through authentication and authorization.
Applies least privilege.
Prevents unrestricted database/system access.
Filters data before exposing it to AI.
Records meaningful sensitive operations.
Handles errors safely.
Provides observability.
Is publicly deployed.
Includes a working demonstration.
Has professional documentation.
Can be defended technically in an interview.
7. Secondary objectives

Classification: FACT

The project is also intended to teach and demonstrate understanding of:

MCP
JSON-RPC
MCP clients
MCP servers
tools
resources
prompts where relevant
discovery
schemas
transport
authentication
authorization
RBAC
least privilege
prompt injection
API security
database security
audit logging
tracing
rate limiting
deployment
production AI systems

A further objective is career/portfolio value in:

AI engineering
AI product engineering
AI solutions engineering
AI product building
AI product management

These career directions originate from the project's master instruction.

8. Explicit non-objectives

Classification: FACT

The project explicitly does not aim to become:

a simple chatbot
a generic REST API
an MCP hello-world demo
a fake enterprise dashboard
a static local server
a collection of meaningless tools
an unrestricted SQL interface
a system where the AI has administrator privileges
a project that assumes the model itself is the security boundary
a project with OAuth merely because OAuth sounds advanced
an unnecessarily complex microservice architecture
an unnecessarily huge database
a system making fabricated enterprise-scale security/usage claims
a copied protocol example that the developer does not understand
9. Existing implementation

Classification: FACT

There is currently no substantive ContextBridge implementation established in this conversation.

The project was deliberately kept in a pre-implementation state.

The repository baseline was created, but no MCP server, MCP client, database, authentication system, authorization engine, production tool implementation, or deployed service has been established in this conversation.

10. Existing repository state

Classification: FACT

The GitHub repository exists:

ORAM15/ContextBridge

The local project directory is:

D:\BRDR\Development\Active Projects\ContextBridge

The local repository was initialized and connected to:

https://github.com/ORAM15/ContextBridge.git

The default branch is:

main

The repository was initially created with a GitHub-generated commit:

8917c65 Initial commit

A local project baseline was then created:

456a31d chore: establish project baseline

The two histories were intentionally reconciled, resulting in:

4801420 chore: reconcile remote repository history

The resulting history was:

*   4801420 chore: reconcile remote repository history
|\
| * 8917c65 Initial commit
* | 456a31d chore: establish project baseline
|/

The final verified state was:

On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean

The repository was confirmed to be private at the time it was inspected.

Classification: FACT

The baseline contains:

README.md
.gitignore
LICENSE

and the intended documentation directories:

docs/
├── architecture/
├── decisions/
└── mcp/

The directories were initially empty and therefore were not tracked by Git.

11. Existing functionality

Classification: FACT

No functional ContextBridge capability currently exists based on the established conversation.

The repository baseline README describes the intended direction but does not constitute implemented functionality.

12. Existing technologies

Classification: FACT

At the repository level, the only technologies actually established are:

Git
GitHub
Markdown
GitHub's repository infrastructure

No application runtime or framework has actually been implemented.

The following were recommended but not implemented:

TypeScript
Node.js
MCP TypeScript SDK
Zod
Fastify
PostgreSQL
Drizzle
GitHub API
Vitest
Pino
OpenTelemetry

Those remain PROPOSALS.

13. Existing integrations

Classification: FACT

The only currently established integration is:

Local Git repository
        ↕
GitHub repository

No MCP integration has been implemented.

No PostgreSQL integration has been implemented.

No GitHub API integration has been implemented.

No AI client integration has been implemented.

14. Existing limitations

Classification: FACT

At the current state:

ContextBridge is not an operational MCP server.
There is no MCP client.
There are no implemented tools.
There is no database.
There is no authentication.
There is no authorization system.
There is no RBAC implementation.
There is no policy engine.
There is no audit logging implementation.
There is no observability implementation.
There is no production deployment.
There is no demonstrated AI-client workflow.
There are no security tests.
There is no demonstrated denied operation.

The repository is therefore currently a project baseline, not a functioning product.

15. Future ideas

The following ideas were discussed as future implementation directions.

Classification: PROPOSAL

Potential tools:

search_incidents
get_incident
search_github_issues
create_github_issue

Potential engineering-operations data:

users
organizations
customers
orders
documents
permissions
audit_events
tool_configurations

The later proposal instead suggested a narrower engineering-operations database:

users
roles
permissions
incidents
services
audit_events

The following were also discussed as potential future capabilities:

remote MCP deployment
OAuth/OIDC-based authentication where actually required
RBAC
rate limiting
OpenTelemetry tracing
GitHub integration
safe-output filtering
security testing
public deployment
AI demonstration client
production hardening

These are future directions, not current implementation facts.

16. Requirements
Product requirements

Classification: FACT

The final system is required to:

expose useful MCP capabilities
allow tool discovery
execute tools correctly
validate schemas
authenticate where required
authorize requests
enforce permissions
deny unsafe/unauthorized operations
maintain meaningful audit logs
handle errors
provide observability
provide a deployed database
provide a deployed server
provide a working demo client
provide professional documentation
include a threat model
document limitations
have a polished GitHub repository
Security requirements

Classification: FACT

The system must:

follow least privilege
avoid unrestricted database access
avoid exposing raw SQL execution to the AI
validate tool arguments
independently authorize operations
avoid treating the model as a security boundary
prevent secret exposure
limit sensitive data returned to AI
provide auditability
consider prompt injection
consider privilege escalation
consider SQL injection
consider malicious clients
consider denial-of-service/rate abuse
consider replay where relevant
Educational requirements

Classification: FACT

The developer must understand rather than blindly generate:

MCP
JSON-RPC
tools
schemas
authorization
security
transport
authentication
17. Constraints

Classification: FACT

The project has explicit constraints:

Do not overengineer.

The master instruction repeatedly emphasizes realistic implementation and avoiding unnecessary complexity.

Do not build substantial code before understanding the system.

The established process is:

requirements
→ use case
→ threat model
→ architecture
→ tool definitions
→ data model
→ authentication
→ authorization
→ deployment
→ testing
→ implementation
Avoid unnecessary infrastructure.

Specifically:

no unnecessary microservices
no unnecessary Redis
no unnecessary Kubernetes
no unnecessary Kafka
no unnecessary OAuth implementation
no unrestricted SQL
no unnecessary database complexity
Public deployment is required eventually.

The final system must not remain localhost-only.

Do not make unsupported security claims.

The project must explicitly document its threat model and limitations.

18. Assumptions

Classification: FACT where explicitly stated; otherwise UNKNOWN

Explicitly established assumptions include:

AI clients will interact with ContextBridge through MCP.
The underlying enterprise systems should not be directly exposed to the AI.
Security decisions should happen outside the model.
Tool capabilities should be narrow and purpose-specific.
Structured results are preferable to unrestricted raw data.
A realistic demonstration should include both allowed and denied operations.
MCP is the protocol layer, while underlying systems may continue using ordinary APIs/database mechanisms.
Still assumptions rather than established facts

Classification: OPEN QUESTION / UNKNOWN

The exact AI client has not been finalized.
The exact authentication mechanism has not been finalized.
The exact production hosting provider has not been finalized.
The exact PostgreSQL provider has not been finalized.
The exact production MCP transport implementation has not been implemented.
The exact domain has not been formally approved.
19. Decisions already explicitly approved

This category must be kept extremely strict.

Project identity

DECISION

The project is ContextBridge.

Repository

DECISION

The project repository is:

ORAM15/ContextBridge

Local location

DECISION

The working repository is:

D:\BRDR\Development\Active Projects\ContextBridge
Git branch

DECISION

Use:

main
Repository baseline

DECISION

The repository baseline was created and committed.

Product principle

DECISION

ContextBridge must not give AI unrestricted access to underlying systems.

Least privilege

DECISION

Least privilege is a core architectural/security principle.

Model is not security boundary

DECISION

Security decisions must be made outside the model where possible.

No unrestricted SQL

DECISION

ContextBridge will not expose unrestricted SQL execution to the AI.

No superficial MCP project

DECISION

A hello-world MCP server cannot constitute the finished project.

Public deployment

DECISION

The final project must be deployed publicly.

Documentation/interview defensibility

DECISION

The finished system must be documented sufficiently for the developer to explain and defend its architecture and security model.

Incremental development

DECISION

Substantial implementation should happen incrementally after foundational understanding and requirements work.

20. Proposals that were never approved

This is particularly important.

The following were recommended by the assistant but were not explicitly approved by the user in this conversation.

Engineering Operations as the final domain

PROPOSAL

The assistant recommended replacing the illustrative customer/order domain with an engineering-operations domain involving incidents, services and GitHub.

The user did not explicitly approve this proposal.

Therefore it must not be treated as final.

Four-tool catalog

PROPOSAL

search_incidents
get_incident
search_github_issues
create_github_issue

Not approved.

TypeScript

PROPOSAL

Recommended as the core language.

Not yet approved through an explicit user decision.

Node.js

PROPOSAL

Recommended runtime.

Not yet approved.

Official MCP TypeScript SDK v2

PROPOSAL

Recommended MCP implementation library.

Not yet approved.

Zod 4

PROPOSAL

Recommended validation library.

Not yet approved.

Fastify

PROPOSAL

Recommended HTTP framework.

Not yet approved.

PostgreSQL

PROPOSAL

Recommended database.

The master instruction says PostgreSQL was a strong backend research proposal, but the later exact-stack recommendation was still a proposal rather than a confirmed user decision.

Drizzle ORM

PROPOSAL

Recommended database access layer.

Not approved.

GitHub API

PROPOSAL

Recommended external integration.

Not approved as a final integration.

Vitest

PROPOSAL

Recommended testing framework.

Not approved.

Pino

PROPOSAL

Recommended logging library.

Not approved.

OpenTelemetry

PROPOSAL

Recommended later for tracing.

Not approved.

Fastify + Streamable HTTP production architecture

PROPOSAL

Recommended, but not yet implemented or explicitly approved.

OAuth 2.1 / OIDC

PROPOSAL

Recommended conditionally for production depending on actual client/deployment requirements.

Not approved as a final authentication implementation.

Modular monolith

PROPOSAL

Recommended architecture.

Not explicitly approved.

Specific RBAC roles

PROPOSAL

Viewer
Analyst
Engineer
Admin

Not approved.

Specific permission names

PROPOSAL

incident.read
github.issue.read
github.issue.create

Not approved.

21. Unknowns

The following information has not yet been established.

Product/domain
What exact real-world domain will ContextBridge serve?
Is Engineering Operations actually the final domain?
Who is the exact primary customer/user?
What organization/system is being modeled?
What specific pain point will the final demo represent?
MCP
Which exact current MCP specification revision will be frozen for implementation?
Which MCP capabilities beyond tools are actually required?
Are resources required?
Are prompts required?
Is task functionality required?
Which client(s) must be supported?
What exact remote transport configuration will be used?
Authentication
Who authenticates?
Is authentication between AI client and ContextBridge, user and ContextBridge, or both?
Which identity provider will be used?
Is OAuth actually necessary for the chosen demo client?
What credential lifecycle will be used?
Authorization
What exact roles exist?
What exact permissions exist?
Are permissions global or organization-scoped?
Will permissions apply at tool level, resource level, record level, or multiple levels?
Data
Exact schema
Exact entities
Exact relationships
Data volume
Seed data
Sensitive fields
Data-retention policy
External systems
Which systems besides PostgreSQL and GitHub, if any?
Is GitHub definitely required?
Which GitHub operations are permitted?
Which repositories may ContextBridge access?
Deployment
Hosting provider
PostgreSQL provider
Domain
TLS configuration
Secret-management mechanism
Production authentication provider
CI/CD strategy
Evaluation
Exact success metrics
Security test coverage target
Performance expectations
Tool latency expectations
Reliability expectations
Definition of "production target"
22. Risks

Classification: FACT

The project explicitly recognizes the following risks.

MCP protocol evolution

MCP is evolving, meaning old tutorials and examples may no longer accurately represent current protocol behavior.

Authentication overengineering

OAuth could consume disproportionate development effort without contributing meaningful value to the actual demonstration.

Fake security

Having authentication/RBAC tables does not itself establish security.

Security controls must be demonstrated through enforcement and tests.

Excessive tool surface

Too many tools increase complexity and attack surface.

Prompt injection

Retrieved content can attempt to manipulate the model.

ContextBridge must not rely on model obedience for security.

Data leakage

Raw database records could expose information that the AI should never receive.

SQL injection

AI-controlled arguments must never become unrestricted SQL.

Excessive privileges

A compromised/manipulated AI should not automatically receive administrator-level capabilities.

Secret exposure

Credentials must never be returned to model context or committed to Git.

Malicious clients

A publicly deployed MCP server must not assume every client is trustworthy.

Rate abuse / denial of service

Remote tool invocation creates potential abuse and resource-consumption risks.

"Enterprise theater"

The project risks looking impressive while containing little actual engineering substance.

This is explicitly something the project is intended to avoid.

23. Dependencies
Confirmed dependencies

FACT

The project depends on:

Git
GitHub
a development machine
an AI client eventually capable of MCP interaction
underlying systems/data eventually used by the implementation
Proposed dependencies

PROPOSAL

Potential future dependencies:

Node.js
TypeScript
MCP SDK
Zod
PostgreSQL
GitHub API
hosting provider
AI provider
authentication provider

None of the external production services have yet been definitively selected.

24. Success criteria

Classification: FACT

The master definition of done requires all of the following:

MCP server works
        ↓
tools are discoverable
        ↓
tools execute correctly
        ↓
schema validation works
        ↓
authentication works where required
        ↓
authorization works
        ↓
permissions are enforced
        ↓
unsafe operations are denied
        ↓
audit logs work
        ↓
errors are handled
        ↓
observability exists
        ↓
database is deployed
        ↓
server is deployed
        ↓
demo client works
        ↓
documentation is professional
        ↓
GitHub repository is polished
        ↓
architecture can be defended in an interview

A further implicit success criterion is that the project must demonstrate an actual integration/security problem rather than merely demonstrate that an MCP SDK can create a tool.

Extracted state summary
Facts extracted
The project is ContextBridge.
Its purpose is controlled AI access to external tools/data.
MCP is central to the project.
Security is a core concern.
Least privilege is a core principle.
The model is not the security boundary.
Unrestricted SQL is explicitly rejected.
Authentication, authorization, validation, safe output, auditability and observability are required.
Public deployment is required for the finished system.
The project must have a working AI-client demonstration.
A denied-operation demonstration is explicitly desirable/required as part of the security story.
The project has not yet had substantive implementation.
ORAM15/ContextBridge exists.
The local repository is at D:\BRDR\Development\Active Projects\ContextBridge.
main is the branch.
The repository baseline has been committed and pushed.
The current repository state was verified clean and synchronized with GitHub.
The project explicitly rejects superficial MCP demos and unnecessary infrastructure.
Decisions extracted

Only the following should currently be treated as established decisions:

Project name: ContextBridge.
GitHub repository: ORAM15/ContextBridge.
Local repository location.
main as the branch.
Repository baseline established.
ContextBridge must control AI access rather than expose unrestricted underlying systems.
Least privilege is fundamental.
Authorization must not depend on model compliance.
Unrestricted SQL is not acceptable.
The finished product must be deployed.
The finished product must be technically defensible and professionally documented.
Development must be incremental and understanding-driven.
Unresolved questions

The most important unresolved items are:

What is the final domain/use case?
Who exactly is the primary user?
What exact problem scenario will the demo solve?
Which MCP client(s) must ContextBridge support?
Which exact MCP specification revision will implementation target?
What capabilities beyond tools are actually needed?
What authentication mechanism is required?
What exact authorization model/roles/permissions are required?
Is PostgreSQL definitely the final database?
Is GitHub definitely the final external integration?
What production hosting/services will be used?
What exact security/evaluation criteria determine success?
Contradictions / evolution in the record

There are no direct contradictions about the project's fundamental mission, but there are several places where the project evolved from broad requirements into assistant recommendations.

Domain

The master instruction initially used:

customers
orders

as an example.

Later, the assistant proposed:

Engineering Operations
incidents
services
GitHub

These are not necessarily contradictory because the original customer/order example was explicitly presented as an example. However, the later Engineering Operations domain was never explicitly approved.

Therefore the final domain remains unresolved.

Authentication

The master instruction says possible mechanisms include:

API keys
OAuth
signed tokens
session authentication

and explicitly says not to implement OAuth merely because it sounds advanced.

The later response recommended OAuth 2.1/OIDC conditionally for remote deployment.

These are compatible, but the final mechanism remains unresolved.

Transport

The earlier architecture recommendation was:

local → stdio
production → Streamable HTTP

This was presented as a recommendation based on the contemporary MCP specification.

It is not an approved implementation decision.

Technology stack

The later response presented an "exact technology stack," but the user asked for recommendations at that stage and did not explicitly approve each technology afterward.

Therefore:

The stack should not currently be treated as approved architecture.

Missing information requiring owner input

Before architecture is frozen, the project owner needs to decide:

┌─────────────────────────────────────────┐
│       CONTEXTBRIDGE OWNER INPUT         │
├─────────────────────────────────────────┤
│ 1. Final domain                         │
│ 2. Primary user                         │
│ 3. Core demo scenario                   │
│ 4. Required MCP client(s)               │
│ 5. Required MCP capabilities            │
│ 6. Authentication requirement           │
│ 7. Authorization/role model             │
│ 8. Required external integrations       │
│ 9. Production deployment expectations   │
│10. Evaluation / success thresholds      │
└─────────────────────────────────────────┘

Until these are settled, the final architecture should not be considered frozen.

Constitution status

Current status: DRAFT

The repository exists and has a clean baseline, but ContextBridge itself has not yet entered substantive implementation.

Most importantly, this forensic extraction reveals that we should not accidentally promote the previously recommended Engineering Operations + PostgreSQL + GitHub + TypeScript/Fastify/Drizzle/Zod stack into "the architecture." Those were recommendations, not owner-approved decisions.

That distinction should remain preserved in the permanent project record.
# CP-P1-01 — Product Use-Case Resolution

## Objective
Resolve the final real-world domain/use case without changing the approved product vision.

## Candidate Domains

### Domain 1: Engineering Operations (Incidents, Services, Issues)
- **AI Usefulness**: High. AI agents are heavily utilized to assist with debugging, reading logs, and correlating incidents across systems.
- **Tool Usefulness**: High. Tools for fetching logs, looking up service owners, retrieving incidents, and querying code repositories map well to typical SRE/Developer workflows.
- **Permission Relevance**: High. Strict least privilege is necessary. An AI agent might be allowed to read incident data but not close them, or query code but not commit to main.
- **Structured-Data Relevance**: High. Engineering systems (GitHub, PagerDuty, Jira) return highly structured data which is essential for accurate contextual grounding.
- **Implementation Feasibility**: High. Known integration points (e.g., GitHub API) exist and are well-documented.

### Domain 2: E-Commerce (Customers, Orders, Inventory)
- **AI Usefulness**: Medium-High. AI can assist customer service representatives.
- **Tool Usefulness**: High. Looking up orders, canceling items, applying refunds.
- **Permission Relevance**: High. PII considerations require strict RBAC.
- **Structured-Data Relevance**: High. Orders and inventory are structured.
- **Implementation Feasibility**: Medium. Might require mocking a large database of fake customer and order data, which is less organic than querying an actual project management tool or code repository.

## Assessment

The "Engineering Operations" domain provides a stronger native fit for a developer-focused MCP (Model Context Protocol) context integration. It allows the system to interact with live developer APIs (like GitHub) rather than relying exclusively on a mocked database of fake customer orders, providing a more realistic and compelling demonstration of ContextBridge's core value: securing autonomous AI agent interactions with enterprise tools.

## Proposed Use Case

**Domain**: Engineering Operations

**Primary User**: Platform Engineer / Site Reliability Engineer (SRE)

**Core Demo Workflow**:
1. AI Agent asks ContextBridge to search for recent open incidents.
2. ContextBridge validates the AI Agent's read-only permissions for incidents and returns a list.
3. AI Agent queries the details of a specific incident.
4. AI Agent attempts to automatically close the incident. ContextBridge explicitly DENIES this operation due to lack of write permissions (demonstrating the security thesis).
5. AI Agent instead looks up the service owner in GitHub and requests ContextBridge to create an issue in the respective repository, which is authorized.

**Tool-Domain Boundary**:
- `search_incidents` (Read)
- `get_incident` (Read)
- `search_github_issues` (Read)
- `create_github_issue` (Write)

## Traceability against Requirements

- **AI-001 / AI-002 / AI-003**: AI interacts with tools to find and create issues.
- **AI-004 / AI-008**: The system will not trust the model to close incidents.
- **AI-006**: Outputs from GitHub/Incident tools are mapped to structured interfaces.
- **AI-007**: The denied operation (closing the incident) directly fulfills this requirement.

## Decision Request

We seek Owner Approval to proceed with **Engineering Operations** as the finalized use case.
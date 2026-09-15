# ContextBridge Product Use-Case Definition

**Status:** PENDING APPROVAL
**Checkpoint:** CP-P1-01

## 1. Selected Domain: Corporate IT Support Helpdesk

The selected real-world domain for ContextBridge is a **Corporate IT Support Helpdesk System**.

## 2. Why This Domain Was Selected

This domain satisfies all criteria from CP-P1-01:

- **Genuine AI Tool-Use Value:** An AI assistant can significantly accelerate support workflows by querying ticket status, summarizing past interactions, looking up asset assignments, and drafting responses.
- **Meaningful Structured Data:** Tickets, users, and assets have strict schemas (e.g., ticket ID, severity, status, assignee, timestamps) that require structured retrieval.
- **Meaningful Authorization:** A critical requirement for ContextBridge is demonstrating authorization. In a helpdesk, a standard employee must only be able to view and update their own tickets, whereas an IT administrator can view all tickets and reassign them.
- **Controlled Data Exposure:** Tools must be carefully designed to prevent the AI from querying sensitive HR data (like salary or termination plans) while resolving IT tickets.
- **Realistic Implementation Scope:** A helpdesk schema can be implemented with a simple set of tables/entities (Users, Tickets, Assets, Comments) without requiring massive infrastructure, fitting within the project's constraints.

## 3. Primary User Definitions

- **Standard Employee:** Authenticates to the system to check the status of their IT requests, add comments, or request new software/hardware.
- **IT Support Agent (Admin):** Authenticates to manage queues, update ticket statuses, and view all hardware assignments.
- **AI Assistant:** Operates on behalf of the authenticated user via MCP tools, strictly constrained by the user's permissions.

## 4. Core Demo Workflow

The final P4/P6 demonstration will execute the following workflow:

1. **Authentication:** The user logs in as a "Standard Employee".
2. **Context Gathering:** The user asks the AI: *"What is the status of my laptop replacement request?"*
3. **Authorized Tool Use:** The AI invokes the `get_tickets` tool. ContextBridge validates the identity and automatically scopes the query to only tickets owned by the employee.
4. **Structured Data:** The tool returns a structured JSON payload representing the ticket.
5. **AI Response:** The AI reads the JSON and responds conversationally.
6. **Denied Operation Demonstration:** The user instructs the AI: *"Delete all tickets."* The AI attempts to invoke a `delete_ticket` tool, which is intercepted and DENIED by the ContextBridge authorization layer because the user lacks the `admin` role. The AI reports the permission failure.

## 5. Tool-Domain Boundary

The system will expose the following tools to the AI via MCP:

- `get_tickets(status?, assignee?)` -> Returns ticket metadata (enforces identity filtering).
- `get_ticket_comments(ticket_id)` -> Returns discussion history (enforces read access).
- `add_ticket_comment(ticket_id, comment)` -> Appends a note (enforces write access).
- `update_ticket_status(ticket_id, status)` -> Updates status (requires elevated role).

ContextBridge will act as the intermediary, ensuring that the AI can discover these tools, understand their schemas, and invoke them, while ContextBridge securely intercepts all invocations to enforce identity, RBAC, and data-exposure limits before executing the underlying business logic.

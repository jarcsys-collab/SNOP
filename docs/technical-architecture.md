# SNOP Technical Architecture

**Status:** Initial baseline approved; detailed design contains open decisions.  
**Technology baseline approval:** 2026-11-12

## 1. Approved technology baseline

| Concern | Approved technology |
|---|---|
| Frontend | React + TypeScript |
| Backend | Node.js + NestJS |
| Architecture | Modular Monolith |
| Operational database | MongoDB Atlas |
| Analytical data source | Microsoft Fabric SQL Analytics Endpoint |

This approval establishes the primary technologies. It does not automatically approve every library, hosting service, identity configuration, or deployment choice.

## 2. Architecture overview

~~~text
Users (SCD / SMD / Administrators)
              |
              v
     React + TypeScript UI
              |
         HTTPS / REST
              |
              v
     NestJS Modular Monolith
       |                |
       v                v
MongoDB Atlas     Fabric SQL Analytics Endpoint
SNOP operational  Gold analytical/reference data
data               (read-only from SNOP)
~~~

### Responsibility boundaries

**Frontend**
- Renders SCD and SMD experiences.
- Collects user input and displays backend results.
- Calls the REST API.
- Does not connect directly to MongoDB or Fabric.
- Does not own authoritative business calculations or authorization.

**NestJS backend**
- Owns API contracts, validation, business rules, calculations, authorization, workflow transitions, and audit events.
- Coordinates reads from Fabric and reads/writes to MongoDB.
- Exposes explicit module interfaces and keeps dependencies controlled.

**MongoDB Atlas**
- Stores SNOP operational and planning data.
- Does not replace Fabric as the source for Gold analytical/reference data.
- Uses a dedicated SNOP database and dedicated application credentials, subject to infrastructure confirmation.

**Microsoft Fabric**
- Supplies approved Gold tables through the SQL Analytics Endpoint.
- Is accessed read-only by SNOP.
- Is not automatically updated with planning or approval results.

## 3. Backend modular structure

The following are logical NestJS modules within one deployable backend. They are not separate microservices.

| Module | Main responsibility |
|---|---|
| Identity & Access | Identity integration, application users, roles, permissions |
| SNOP Cycles | Planning period, supplier, division, assigned SMD approver |
| SNOP Workspace | Shared workspace lifecycle and collaborative planning |
| Inventory | Inventory and stock-position queries |
| Calculations | Run rate, inventory months, safety stock, reorder point, SOQ and related measures |
| Lead Time | Supplier, transport mode, origin point, lead-time configuration |
| Planning & Decisions | SMD order quantities, notes, tags and decisions |
| Approval Workflow | SCD confirmation, SMD confirmation, locking, approval and rejection |
| Audit & History | Traceable user actions, timestamps, versions and change history |
| Fabric Integration | Read-only SQL connectivity and query/data-access adapters |

Each module should own its domain services and persistence/query interfaces. Other modules should use exported application services or interfaces rather than reaching into another module's internal repositories.

## 4. Data ownership

### Fabric Gold — analytical and reference data

Expected source responsibilities include:
- Product and product-division mapping from Dim_productDB.
- Supplier/product reference attributes.
- Inventory and sales facts used by SNOP measures.
- Other approved Gold datasets required by the manual SNOP report.

Exact table names, columns, refresh characteristics, and data types must be verified against the actual Fabric endpoint before implementing queries.

### MongoDB Atlas — SNOP operational data

Proposed logical collections:
- snop_cycles
- snop_workspaces
- snop_workspace_items
- supplier_lead_times
- snop_decisions (if decisions are not embedded in workspace items)
- snop_approvals
- snop_audit_events
- application_users / application_roles (only if needed by the selected identity design)

These are logical candidates, not a final schema. Choose embedding versus references based on aggregate boundaries, access patterns, versioning, and expected data volume. Avoid unbounded arrays and oversized documents.

## 5. Fabric integration

- Backend-only access through a dedicated Fabric data-access adapter.
- Use the SQL Analytics Endpoint TDS/SQL interface.
- Use parameterized SQL for all user-supplied values.
- Use a bounded connection pool, query timeouts, and explicit error handling.
- Grant the application identity only the permissions required to read approved Gold objects.
- Do not expose Fabric credentials or connection details to the browser.
- Verify network access, endpoint connection string, identity permissions, driver compatibility, and query performance in a proof of concept.

Candidate Node.js client: mssql with the Tedious TDS driver. The exact driver/authentication configuration is not final until tested against JARC's Fabric endpoint.

## 6. Authentication and authorization

Microsoft Entra ID is the recommended identity direction because the organization uses Microsoft Fabric, but the identity provider and tenant configuration remain to be confirmed.

Regardless of identity provider:
- Authentication establishes who the user is.
- Application RBAC determines what the user can do.
- Backend guards/policies enforce permissions on every protected API operation.
- The UI may hide unavailable actions for usability, but this is not an authorization boundary.
- Record actor identity and timestamps for sensitive workflow and configuration changes.

Expected application roles include System Administrator, SCD Admin, SCD User, SMD User, SMD Approver, and Management Viewer. Final role definitions and permission matrix remain to be approved.

## 7. Business calculation design

- Translate the existing DAX measures into backend calculation services.
- Keep calculations deterministic and independently testable.
- Do not expose DAX expressions to end users; users see calculated output values.
- Maintain reference test cases using known input data and Power BI outputs.
- Explicitly document differences between DAX date/filter semantics and the new implementation.
- Do not invent missing measures. Track missing or ambiguous formulas as open requirements.

## 8. Workspace and approval integrity

The shared workspace is created by SCD and accessed by SCD and the assigned SMD approver for collaborative planning.

The agreed workflow includes:
1. SCD creates the workspace and assigns an SMD approver.
2. SCD and SMD collaborate on the same workspace.
3. SCD confirms the final data.
4. The assigned SMD approver separately confirms the final data.
5. A planning-data change before both confirmations invalidates previous confirmations.
6. Once both confirmations are valid, the workspace is locked.
7. The assigned SMD approver formally approves or rejects.
8. Approved workspaces are read-only. Rejected workspaces return for correction with comments and audit history.

The backend must enforce state transitions and locking. The UI alone must never control approval state.

## 9. Initial security and operational requirements

- Keep secrets in environment-specific secret storage; commit only example variable names in .env.example.
- Use separate credentials for development, test, and production.
- Apply least-privilege access to MongoDB and Fabric.
- Validate and normalize all API input.
- Use structured application logs and correlation/request IDs.
- Audit business-critical changes and workflow transitions.
- Apply pagination and bounded filters to potentially large inventory results.
- Export Excel from the same backend query/data service used by the displayed inventory view.
- Define retention, backup, restore, and deployment procedures before production release.

## 10. Decisions still required

See [Open Decisions](open-decisions.md). Do not treat unconfirmed recommendations as approved implementation requirements.

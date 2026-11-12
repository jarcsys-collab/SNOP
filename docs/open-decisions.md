# SNOP Open Decisions

This document separates approved baseline choices from recommendations that still require confirmation.

| ID | Decision | Current position | Status |
|---|---|---|---|
| OD-001 | Authentication provider | Microsoft Entra ID recommended | Pending confirmation |
| OD-002 | Hosting/deployment target | Azure hosting is a candidate; no target selected | Pending |
| OD-003 | Frontend build tool | Vite recommended for React SPA | Pending |
| OD-004 | Package manager | pnpm workspaces recommended | Pending |
| OD-005 | MongoDB data access | Select native driver or Mongoose after schema/access-pattern review | Pending |
| OD-006 | Fabric Node.js driver | mssql + Tedious candidate; must pass connection PoC | Pending verification |
| OD-007 | UI component system | Select after reviewing table editing, accessibility, Excel export and theming needs | Pending |
| OD-008 | API data-fetching strategy | TanStack Query is a candidate | Pending |
| OD-009 | Workspace snapshot policy | Controlled data snapshot/freeze is recommended; exact refresh and freeze rules are not approved | Pending |
| OD-010 | SMD approver permissions | Whether the approver may also enter order quantities is unresolved | Pending |
| OD-011 | Supplier/division selection | Single vs multiple selection per workspace is unresolved | Pending |
| OD-012 | Origin and transport assignment | Confirm whether selection is per product row, supplier, or another level | Pending |

## Required proof of concept

Before finalizing Fabric integration:
1. Obtain the exact Fabric SQL Analytics Endpoint connection string and target Gold object names.
2. Confirm Microsoft Entra identity and required permissions.
3. Test a Node.js connection using the proposed TDS driver.
4. Run a parameterized read query against a permitted Gold table.
5. Verify result types, date handling, query latency, timeout behavior, and connection-pool reuse.
6. Record the test environment and evidence without committing secrets.

## Change control

When an item is decided:
- Record the approved decision and date.
- Update the architecture or relevant requirements document.
- Remove or update the corresponding pending entry.
- Do not infer approval from implementation activity.

# SNOP Delivery Roadmap

**Status:** Initial staging; dates and estimates not yet committed.

| Phase | Scope | Exit criteria |
|---|---|---|
| 1. Requirements baseline | Manual report, DAX mapping, SCD/SMD workflow, roles, field definitions | Business requirements and calculation gaps reviewed and approved |
| 2. Technical foundation | Resolve open decisions, Fabric connection PoC, detailed architecture | Architecture and integration approach approved |
| 3. Repository foundation | Monorepo, React app, NestJS app, linting, tests, CI checks | Clean install, build, type-check and baseline tests pass |
| 4. Security and data access | Authentication, RBAC, MongoDB access, Fabric read adapter, audit foundation | Access rules and data connections validated |
| 5. SCD capabilities | SNOP settings, workspace, calculations, lead-time maintenance | SCD workflow and calculation outputs validated |
| 6. SMD capabilities | Inventory View, filters, Excel export, planning/decision inputs | Display and export use the same backend dataset; permissions validated |
| 7. Approval workflow | Dual confirmation, invalidation, locking, approve/reject, history | Workflow transition and concurrency tests pass |
| 8. Validation and UAT | Power BI comparison, role tests, workflow tests, user acceptance | Business and technical acceptance recorded |
| 9. Deployment and handover | Production deployment, monitoring, backup/restore, training and support docs | Operational readiness approved |

## Delivery rules

- Do not estimate dates until Phase 1 and the major integration unknowns are resolved.
- Build vertical slices with tests rather than generating all modules at once.
- Validate calculation outputs against Power BI before business acceptance.
- Do not treat a completed coding task as approval.
- Keep the roadmap updated only when scope or delivery decisions are approved.

# SNOP — Sales & Operations Planning

SNOP is a JARC internal web application supporting collaborative Sales & Operations Planning between the Supply Chain Department (SCD) and Sales & Marketing Department (SMD).

## Approved technology baseline

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript |
| Backend | Node.js + NestJS |
| Architecture | Modular Monolith |
| Operational database | MongoDB Atlas |
| Analytical source | Microsoft Fabric SQL Analytics Endpoint |

## System boundaries

- Fabric Gold tables are the read-only source for analytical and reference data.
- MongoDB Atlas stores SNOP operational data: workspaces, decisions, lead-time configuration, approvals, and audit history.
- NestJS owns business calculations, validation, authorization, and workflow rules.
- React accesses data only through the backend REST API.
- Approved planning data remains in MongoDB. No automatic write-back to Fabric is in scope.

## Repository structure

~~~text
apps/api/       NestJS backend (planned)
apps/web/       React frontend (planned)
docs/           Architecture, decisions, and roadmap
CLAUDE.md       Development governance
~~~

The application scaffold will be added after the remaining foundation decisions and integration proof of concept are confirmed.

## Governance

- GitHub is the source of truth.
- Claude Code is the primary implementer.
- ChatGPT (SD) supports architecture and independently validates implementation.
- The project owner makes final approvals.
- Implementation completion is not technical approval.

See [CLAUDE.md](CLAUDE.md) and [Technical Architecture](docs/technical-architecture.md).

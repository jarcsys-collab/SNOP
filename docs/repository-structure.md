# SNOP Repository Structure

**Status:** Proposed repository layout.

## Repository strategy

Use one Git repository with a workspace/monorepo layout. The frontend and backend remain independently buildable applications, while the backend is a single modular-monolith deployment.

Proposed layout:

~~~text
SNOP/
├── apps/
│   ├── api/                       # NestJS backend
│   │   └── src/
│   │       ├── modules/
│   │       ├── infrastructure/
│   │       ├── common/
│   │       └── main.ts
│   └── web/                       # React + TypeScript frontend
│       └── src/
│           ├── app/
│           ├── features/
│           ├── components/
│           ├── services/
│           └── main.tsx
├── docs/
│   ├── technical-architecture.md
│   ├── repository-structure.md
│   ├── open-decisions.md
│   └── roadmap.md
├── packages/
│   └── api-contracts/             # Optional generated/shared API types
├── tooling/
├── .github/
│   └── workflows/
├── .editorconfig
├── .gitignore
├── CLAUDE.md
├── package.json
├── pnpm-workspace.yaml
└── README.md
~~~

## Repository conventions

- Use TypeScript for frontend and backend.
- Use a single package manager consistently. pnpm is the current recommendation, pending approval.
- Keep frontend-specific dependencies in apps/web and backend-specific dependencies in apps/api.
- Share API contracts only where it reduces drift; do not share domain implementation between browser and backend.
- Keep environment files out of Git. Commit .env.example files containing names and safe placeholders only.
- Keep architecture decisions in docs/adr/ as the project grows.
- Use feature branches and pull requests for reviewed changes. Protect main before production development begins.

## Scaffold policy

Do not generate the full application scaffold until the open foundation decisions are resolved. The first implementation task should create the minimal workspace, React app, NestJS app, lint/type-check/test configuration, and local developer instructions. It should not implement SNOP business features.

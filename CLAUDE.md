# SNOP Development Governance

## Project roles

- Project owner: owns requirements, priorities, and final approvals.
- Claude Code: primary software implementer.
- ChatGPT (SD): architecture partner and independent technical validator.
- GitHub: source of truth for approved requirements, architecture, code, and decisions.

## Non-negotiable rules

1. Follow the approved technology baseline and documented architecture.
2. Do not silently change an approved architecture or business rule.
3. Do not introduce microservices, queues, caches, or infrastructure components without a documented need and approval.
4. Keep frontend, API, domain logic, and infrastructure responsibilities separated.
5. Enforce authorization in the backend. Frontend visibility controls are not security controls.
6. Treat Fabric as read-only. Do not implement write-back to Fabric.
7. Never expose credentials, tokens, secrets, or real environment files in source control.
8. Use parameterized SQL. Never concatenate user-controlled values into SQL statements.
9. Add tests for business rules and workflow transitions. Calculation tests must compare against approved Power BI reference outputs where available.
10. Never claim tests, builds, or integrations passed unless they were executed. Include commands and results.
11. Identify assumptions, unresolved decisions, and risks explicitly.
12. Document significant architectural decisions in an ADR before implementing a material deviation.
13. Keep changes scoped. Do not mix unrelated refactoring into a feature task.
14. Do not commit, push, merge, deploy, or modify production resources unless explicitly authorized.

## Implementation workflow

1. Read relevant requirements and architecture documents before coding.
2. Summarize the proposed implementation and identify missing decisions.
3. Implement one bounded task at a time.
4. Run relevant formatting, lint, type-check, unit, and integration tests.
5. Report changed files, behavior, test evidence, known limitations, and follow-up risks.
6. Submit significant work for independent review.

## Validation rule

A completed implementation is not automatically approved. SD reviews the implementation and evidence independently. The project owner makes the final approval decision.

## Documentation precedence

If instructions conflict, stop and report the conflict. Do not silently choose.

Approved decisions in GitHub take precedence over assumptions in prompts or generated code. Update documentation only after the project owner approves a change.

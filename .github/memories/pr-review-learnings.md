# PR Review learnings (Anonymized Template)

## User Preferences

- Review Depth: Deep analysis of logic, security, and schema boundaries.
- Formatting: Always separate findings with clean Markdown H3 headers. No details/summary tags.

## Project Patterns & Rules

### Core Web API (ASP.NET & Node.js Services)

- Naming: Service classes and implementation files must share suffixes (e.g., UserService and UserServiceImpl).
- Security: Tenant validation must utilize contextual session contexts (e.g., GetSessionTenantId()). Do not trust client-supplied tenant identifiers in request payloads.
- HTTP Clients: Instantiate HTTP clients via specialized factories to prevent DNS resolution issues. Do not mutate authorization headers on shared singletons.

### Frontend App (React & TypeScript)

- Tailwind CSS styling is preferred over custom CSS variables.
- Always implement type guards for arbitrary event payloads instead of utilizing untyped arguments.

## Recurring Guardrails

- [P0] Hardcoded credentials, private keys, or API tokens committed in configuration payloads.
- [P0] Insecure Direct Object References (IDOR) on cross-tenant operations.
- [P1] Mismatched Dependency Injection lifetimes (e.g., injecting transient instances into singleton lifecycles).
- [P1] Outdated test assertions that fail to verify correct messaging payloads after schema mutations.

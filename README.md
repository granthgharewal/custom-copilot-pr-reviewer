# Stateful Pull Request Reviewer

This repository contains the configuration and memory pattern for a custom GitHub Copilot agent designed to perform context-aware code reviews.

Most code review systems evaluate diffs in isolation. They have no recollection of prior discussions, leaving teams to re-flag the same anti-patterns week after week. This project addresses that problem by pairing a Copilot system instruction prompt with a stateful markdown memory loop.

![Workflow Architecture](images\workflow.png)

## Core Architecture

The system operates around a feedback loop:

1. **Load Context:** Before analyzing a pull request, the agent reads `.github/memories/pr-review-learnings.md` to load active team agreements, approved architectural technical debt, and known pitfalls.
2. **Review:** The agent evaluates the code changes and grades findings into three priorities:
   - **P0 Blockers:** Critical security risks such as hardcoded credentials or multi-tenant scope leaks.
   - **P1 Requirements:** Lifetime mismatches in dependency injection, missing test files, or pattern deviations.
   - **P2 Suggestions:** Layout cleanup, minor refactoring, and general documentation.
3. **Persist:** Newly discovered repository constraints or resolved PR issues are appended back to the memory file, ensuring the agent remains aligned as the codebase evolves.

## Repository Structure

- `.github/copilot-instructions.md`: The system guidelines that define the Copilot agent's persona, rules, and scope.
- `.github/memories/pr-review-learnings.md`: The living context file loaded during active sessions.

## Real-World Case Studies

The anonymized guidelines in `pr-review-learnings.md` reflect real, recurring issues caught during active production reviews:

- **Tenant Isolation:** Client-provided parameters are ignored in favor of backend context lookups to prevent Insecure Direct Object References (IDOR).
- **Dependency Lifecycles:** Detecting transient services mistakenly nested inside singleton lifecycles.
- **Data Serialization:** Preventing mismatches between Newtonsoft.Json and System.Text.Json interfaces during API integration.

## Installation and Usage

To configure this reviewer for your local workspace:

1. Clone or copy `.github/copilot-instructions.md` into your repository.
2. Initialize `.github/memories/pr-review-learnings.md` using the provided template.
3. When inside VS Code, the Copilot chat panel automatically reads these local system instructions and references your memory file during workspace inquiries.

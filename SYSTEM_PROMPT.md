# SYSTEM_PROMPT — Detroit Choices Automation Case Study

## Purpose of This Repository

This repository is a portfolio-ready technical case study documenting the design, implementation, and architecture of a real-world small-business automation system.

It is intended to demonstrate Malcolm Frank's ability to:

- identify operational inefficiencies in a small business context
- design a practical event-driven automation architecture
- implement that architecture using accessible, production-ready tools
- document the system clearly for technical and non-technical audiences

---

## Intended Audience

This repository is written for:

- **AI consulting founders** evaluating implementation capability and technical judgment
- **Solutions architects** reviewing system design, integration patterns, and architecture decisions
- **Technical delivery leads** assessing project execution and documentation quality
- **Engineering hiring managers** looking for evidence of hands-on automation and systems work

The documentation should be readable and credible for all of the above without requiring domain-specific explanation.

---

## Documentation Principles

All documentation in this repository follows these standards:

**Clarity over completeness.** Each file should communicate its core point quickly. Reviewers should be able to skim and understand the system within 5 minutes.

**Implementation-focused.** Describe what was built and why, not what could theoretically be built. Avoid speculation and hypothetical roadmaps unless clearly labeled as future improvements.

**Technically credible.** Use accurate terminology. Name the actual tools, integrations, and architectural patterns used. Do not abstract away implementation details.

**No fabricated metrics.** If exact quantitative outcomes are not known, describe value qualitatively. Do not include invented numbers or exaggerated claims.

**No sensitive data.** This repository contains no API keys, credentials, private endpoints, or identifiable customer data. All examples use sanitized placeholder values.

---

## Repository Governance

### Adding new documentation

When adding new files to this repository:

1. Follow the existing folder structure
2. Use consistent markdown heading levels (H1 for file title, H2 for major sections, H3 for subsections)
3. Write in the same professional, implementation-focused tone
4. Link to related files where helpful
5. Do not add sensitive data or speculative content

### Updating existing documentation

When updating files:

1. Preserve the existing structure unless restructuring is clearly necessary
2. Note any architectural changes accurately
3. Keep the Mermaid diagram in `architecture/system-architecture.md` in sync with any architecture changes

### Screenshot and artifact management

- Screenshots go in `screenshots/`
- Meeting notes, workflow exports, and project artifacts go in `artifacts/`
- Do not commit files containing credentials or private client data

---

## File Index

| File | Purpose |
|---|---|
| `README.md` | Primary landing page for the repository |
| `PROJECT_SUMMARY.md` | Full case study narrative |
| `TECH_STACK.md` | Technology stack breakdown |
| `DATA_MODEL.md` | Database schema and status lifecycle |
| `EVENT_FLOW.md` | Step-by-step event flow |
| `TASK_LIST.md` | Completion and review checklist |
| `SYSTEM_PROMPT.md` | This file — repo governance |
| `SYSTEM.md` | Repo setup and operating guide |
| `architecture/system-architecture.md` | Architecture documentation and Mermaid diagram |
| `workflows/zapier-flow-description.md` | Zapier workflow documentation |
| `workflows/sample-webhook-payload.json` | Sample webhook payload |
| `artifacts/meeting-notes.md` | Project notes and decisions |
| `artifacts/workflow-logic.md` | Workflow logic documentation |
| `artifacts/implementation-notes.md` | Implementation and integration notes |
| `demo/walkthrough-notes.md` | Demo script |

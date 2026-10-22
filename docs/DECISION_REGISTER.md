# Decision register

This register distinguishes confirmed direction from implementation proposals. Update it when the user confirms, rejects, or changes a decision.

| ID | Decision | Status | Rationale / notes |
|---|---|---|---|
| D-001 | Use Obsidian as the initial Markdown workspace rather than rebuilding note-taking features | Confirmed | Reduce scope and reuse existing tools |
| D-002 | Develop locally first with Docker and `.env` configuration | Confirmed | Practical local development and reproducibility |
| D-003 | Use React, TypeScript, Python, FastAPI, Pydantic, SQLAlchemy, PostgreSQL and Axios | Confirmed | User-selected stack |
| D-004 | Use LangChain, Google as initial model provider, and optional LangSmith observability | Confirmed direction | Verify current package/API compatibility during implementation |
| D-005 | AI processing must be explicitly user-triggered | Confirmed | Avoid surprise token usage and unwanted processing |
| D-006 | Use database-managed lookup values for configurable selections | Confirmed | Add values without changing code or migrations |
| D-007 | Application proposals to edit Obsidian-origin content stay pending until reviewed | Confirmed | Avoid silent overwrites |
| D-008 | Preserve raw/source material and process progressively | Confirmed direction | Capture first; deeper processing only when useful |
| D-009 | Use deterministic NLP / spaCy suggestions where practical | Confirmed direction | Reduce unnecessary LLM calls; suggestions remain reviewable |
| D-010 | Keep the primary UI lean: Home, Knowledge, Projects & Tasks, optional Connections | Confirmed direction | Avoid feature overload |
| D-011 | PostgreSQL relational search first; add vector retrieval when needed | Proposed | Validate against implementation needs and evaluation set |
| D-012 | Use a graph-node registry for cross-entity relationships | Proposed | Flexible graph navigation; requires service-level endpoint validation |
| D-013 | Use a local watcher with debounce plus periodic reconciliation | Proposed | Filesystem events alone can be missed |
| D-014 | Store source chunks/embeddings only when deep processing is requested | Proposed | Control storage and processing cost |
| D-015 | Web clipper implementation mechanism | Open | Choose stable/easy option during implementation research |
| D-016 | Exact note/relationship/lookup vocabulary | Open | Start with documented defaults; values are database-managed |
| D-017 | Write-back from application to Obsidian | Open implementation detail | Proposed changes remain pending; explicit apply action and conflict check required |

# Personal Knowledge Workspace

A local-first personal knowledge application built around an Obsidian vault. It helps you capture learning, connect research to projects, maintain plans and actions, and resume interrupted work without repeatedly reconstructing context.

## Current scope

- Obsidian Markdown is authoritative for notes that originate in the vault.
- PostgreSQL stores an indexed/structured representation of vault notes plus application-native records such as projects, tasks, activity history, processing runs, and pending changes.
- The web app is for reading, searching, annotating, connecting, and revisiting knowledge.
- AI is optional and runs only when explicitly requested by the user.
- Deterministic parsing and conventional NLP may suggest metadata without requiring an LLM.
- Changes proposed by the application to an Obsidian-origin note are staged as pending changes for review; they do not silently overwrite the vault.
- Local development runs through Docker Compose and environment variables.

## Technology choices

| Area | Initial choice |
|---|---|
| Frontend | React + TypeScript |
| HTTP client | Axios |
| Backend | Python + FastAPI |
| Validation / API schemas | Pydantic |
| ORM / migrations | SQLAlchemy + Alembic |
| Database | PostgreSQL |
| AI orchestration | LangChain |
| Initial model provider | Google models through supported LangChain integration |
| Observability | LangSmith, optional and configured by environment |
| NLP suggestions | spaCy and deterministic matching where useful |
| Deployment for development | Docker Compose |
| Knowledge source | Local Obsidian Markdown vault |

Provider packages and APIs change. Pin tested versions in dependency manifests and verify current Google/LangChain integration before implementation.

## Documentation

- [Product brief](docs/PRODUCT_BRIEF.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Data model and sync contract](docs/DATA_MODEL_AND_SYNC.md)
- [Ingestion and AI strategy](docs/INGESTION_AND_AI.md)
- [Development standards](docs/DEVELOPMENT_STANDARDS.md)
- [Security and configuration](docs/SECURITY_AND_CONFIGURATION.md)
- [Decision register](docs/DECISION_REGISTER.md)
- [Enum / lookup values](docs/enums/README.md)
- [ERD source](draw/knowledge_workspace_erd.drawio)

## Repository status

This is the initial specification and documentation scaffold. It does **not** claim that the application, API, database migrations, or sync watcher are implemented yet. The frontend and backend directories are placeholders for the implementation.

## Local-first development plan

The first implementation milestone is the core data and sync contract—not a broad feature roadmap:

1. Create the backend project structure, SQLAlchemy models, Alembic migrations, and database-managed lookup values.
2. Implement a local vault scanner/sync service with stable IDs, content hashes, idempotency, tombstones, retry handling, and a reconciliation command.
3. Add API endpoints and tests for notes, projects, tasks, relationships, pending changes, and lookup values.
4. Build the minimal React interface: Home, Knowledge, Projects & Tasks, and optional Connections view.
5. Add optional deterministic suggestions, then user-triggered AI processing and tracing.

## Important rules

- Do not silently overwrite Obsidian files.
- Do not silently merge topics or invent canonical tags.
- AI-derived summaries, tags, entities, and relationships must retain provenance and review state.
- Keep API keys out of source control. Commit `.env.example`, never `.env`.
- Database schema changes must use Alembic migrations.
- Configurable categories and states belong in lookup tables, not hard-coded UI arrays or PostgreSQL native ENUMs.

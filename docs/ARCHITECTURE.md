# Architecture

## Context

The application is local-first during development. The Obsidian vault stays on the host machine. PostgreSQL stores a structured mirror of vault notes plus application-native records. The React app communicates with FastAPI; it does not connect directly to PostgreSQL.

## Components

- **Obsidian vault:** authoritative source for Markdown content and Obsidian-native frontmatter, tags, and wiki-links.
- **Local sync service:** scans or watches the vault, parses files deterministically, and upserts database records. A periodic reconciliation scan is required even if filesystem events are used.
- **FastAPI:** application API, validation, business rules, and access to SQLAlchemy repositories/services.
- **PostgreSQL:** notes, sources, chunks, topics, tags, relationships, projects, tasks, activity, pending changes, processing runs, and lookup tables.
- **React + TypeScript:** lightweight UI.
- **Axios:** API client with central base URL and error handling.
- **Optional NLP:** spaCy, deterministic keyword matching, and duplicate candidate detection.
- **Optional AI service:** LangChain with a Google model provider; LangSmith tracing only when enabled. AI is invoked by explicit user action.
- **Docker Compose:** local PostgreSQL initially; API and frontend services are added when their Dockerfiles exist.

## Source-of-truth ownership

| Data | Authority | Notes |
|---|---|---|
| Markdown content for vault-origin notes | Obsidian file | Database content is a searchable mirror |
| Frontmatter, tags, wiki-links | Obsidian file, parsed by sync | Unknown fields are preserved in raw metadata where possible |
| Application-native notes | Database | May optionally be exported to Markdown later; no initial write-back assumption |
| Projects, tasks, task history, evidence links | Database | Application-managed |
| Canonical topic records and typed relationships | Database | Explicitly created or approved |
| AI extraction proposals | Database, pending review | Not authoritative until accepted |
| Lookup values / configurable enums | Database lookup tables | UI obtains options from API |
| Sync status and source hashes | Database plus local scan state | Supports recovery and reconciliation |

## Sync flow

1. Enumerate Markdown files under the configured vault root.
2. Read bytes and calculate SHA-256 content hash.
3. If file hash and parser version are unchanged, skip expensive parsing and derived work.
4. Parse frontmatter, title, tags, wiki-links, headings, and content. Never execute embedded content.
5. Upsert the note by stable `note_id` from frontmatter when present; otherwise maintain a stable mapping from canonical vault-relative path and introduce a generated ID into frontmatter only through an explicit migration/approval operation.
6. Replace parsed tag/link associations transactionally so removed links do not remain stale.
7. Record file path, hash, parser version, last observed time, and sync status.
8. Use a periodic full reconciliation scan to detect missed events, moves, and deletions.
9. On deletion, mark the database record as a tombstone first. Do not immediately hard-delete linked history or evidence.
10. Retry transient database errors with bounded backoff; sync operations must be idempotent.

## File watcher and Docker

Filesystem notifications can be unreliable across operating systems and mounted volumes. Implement a local Python sync process with event debounce plus periodic reconciliation. For Docker development, mount the vault read-only into the sync service. If host filesystem events are unreliable, poll for changes. The service must never write to the vault by default.

## Application-proposed edits

When a user edits content belonging to an Obsidian-origin note in the web UI, store a `pending_change` with the proposed patch, base content hash, author, and status. The UI can show a diff. Applying the change back to the file is a separate explicit action and must verify that the current file hash still matches the base hash; otherwise raise a conflict.

## API boundaries

- Frontend uses Axios to call FastAPI.
- Pydantic schemas validate request and response payloads.
- SQLAlchemy models and service functions enforce persistence rules.
- CORS origins are configured through environment variables; do not use wildcard origins with credentials.
- Database migrations are managed with Alembic.
- Do not place business logic in route handlers or React components.

## AI boundaries

- AI features are called only after an explicit user action.
- Retrieve only relevant chunks/records; never send the whole vault by default.
- Use schema-constrained extraction, validation, retry limits, and provenance.
- Preserve provider/model, prompt version, input hash, token usage, latency, status, and error metadata.
- Keep LangSmith tracing configurable and document what content may be transmitted to the tracing service.

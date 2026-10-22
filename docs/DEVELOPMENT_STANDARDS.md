# Development standards

## Repository structure (target)

```text
.
├── backend/
│   ├── app/
│   │   ├── api/              # FastAPI routers
│   │   ├── core/             # settings, logging, errors
│   │   ├── db/               # engine, session, base
│   │   ├── models/           # SQLAlchemy models
│   │   ├── schemas/          # Pydantic request/response schemas
│   │   ├── repositories/     # persistence queries
│   │   ├── services/         # business rules
│   │   ├── sync/             # vault scan, parser, reconciliation
│   │   ├── ingestion/         # extraction, chunking, cache
│   │   ├── ai/               # provider interface, LangChain, prompts
│   │   └── main.py
│   ├── migrations/            # Alembic
│   ├── tests/
│   ├── pyproject.toml
│   └── Dockerfile
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   ├── api/              # Axios client and API modules
│   │   ├── components/
│   │   ├── features/         # knowledge, projects, tasks, graph
│   │   ├── hooks/
│   │   ├── types/
│   │   └── main.tsx
│   ├── package.json
│   ├── tsconfig.json
│   └── Dockerfile
├── docs/
├── draw/
├── scripts/
├── .env.example
├── docker-compose.yml
└── README.md
```

## Backend

- Use FastAPI routers for HTTP handling; keep business logic in services.
- Use Pydantic for external payload validation and settings validation.
- Use SQLAlchemy 2.x style models and sessions.
- Use Alembic for all schema changes.
- Use dependency injection for DB sessions and settings.
- Add type hints to public functions.
- Use structured logging; avoid logging secrets or full private note content by default.
- Return consistent error responses and meaningful HTTP status codes.
- Use a supported PostgreSQL driver and test transaction rollback paths.
- Avoid circular imports and giant service modules.

## Frontend

- React + TypeScript with strict type checking.
- Axios instance centralises base URL, timeouts and error handling.
- Keep API DTO types separate from UI view models.
- Organise code by feature; keep components focused.
- Use accessible labels, keyboard navigation, loading/error/empty states.
- Avoid duplicating lookup values in frontend constants; fetch them from the API.
- Keep graph view optional and provide a list alternative.
- Do not store API secrets in frontend environment variables.

## Quality gates

- Unit tests for parsing, normalization, hashing, lookup validation, relationship validation, and sync idempotency.
- Integration tests against PostgreSQL for migrations, constraints, pending changes and tombstones.
- API tests for valid/invalid payloads and CORS configuration.
- Frontend lint, type-check, and component tests for key flows.
- Test sync scenarios: create, edit, rename, delete, duplicate events, malformed frontmatter, DB offline/recovery, and full reconciliation.
- Keep dependencies pinned/locked and update intentionally.

## Git workflow

- Commit documentation and `.env.example`; never commit `.env`, API keys, vault contents, database volumes, logs, or private documents.
- Use small commits with clear messages.
- Add migration and tests with schema changes.
- Keep generated data and personal vault content outside tracked paths.

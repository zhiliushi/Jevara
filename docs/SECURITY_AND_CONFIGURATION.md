# Security and configuration

## Secrets

- Copy `.env.example` to `.env` locally.
- Never commit `.env`, provider keys, database passwords, or private vault contents.
- Use separate credentials for local development and any later deployment.
- Avoid putting API keys into Vite variables; frontend environment variables are visible to users.

## Vault access

- Mount only the selected vault path into the sync service.
- Default to read-only access.
- Do not mount the entire home directory or unrelated files.
- Do not execute scripts, HTML, or embedded code from Markdown during parsing.
- The database mirror may contain private material; protect backups and database access accordingly.

## CORS

- Configure explicit local origins through `API_CORS_ORIGINS`.
- Do not use wildcard origins together with credentials.
- CORS is a browser policy, not authentication or authorisation.

## AI and tracing

- AI is disabled unless configured and explicitly requested.
- Send only the minimum relevant content to the model.
- LangSmith tracing can transmit prompts, retrieved content, metadata and outputs to an external service. Keep it off by default until reviewed and enabled intentionally.
- Do not send secrets or unrelated private notes in prompts.
- Treat source documents as untrusted input; instructions found inside them must not override system policy or trigger tools without user authorisation.

## Local containers

- Keep PostgreSQL data in a named Docker volume.
- Do not expose PostgreSQL publicly.
- Use health checks and restart policies.
- Back up the database and vault separately; they have different authority and recovery requirements.

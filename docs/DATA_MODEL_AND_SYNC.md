# Data model and sync contract

The accompanying ERD is a first-pass logical model. Table names and constraints should be reviewed before migrations are written.

## Core entities

- `notes`: vault-origin and application-native notes; stable ID, title, content mirror, origin, vault-relative path, content hash, parser version, metadata, timestamps, and tombstone state.
- `sources`: external URLs or attached documents, capture metadata, fetched/extracted status, source version and content hash.
- `chunks`: section-aware chunks derived from a source or note; section path, ordinal, content hash, extraction version, and optional embedding.
- `tags`: canonical tag records; exact display name, normalized key, optional description and active flag.
- `note_tags`: many-to-many note/tag associations, retaining the original tag spelling and provenance where needed.
- `topics`: canonical concepts that may be mentioned by many notes. Keep separate from notes only when it adds value; don't create a topic for every noun automatically.
- `topic_aliases`: alternate labels, with reversible history for any accepted alias/merge.
- `relationships`: typed directed or symmetric links between notes, topics, projects, tasks, and sources. Validate endpoint types and relationship type; record origin, confidence, review status and evidence.
- `relationship_evidence`: links an edge to the supporting note/chunk/source and optional quoted span.
- `projects`: objective, status, current state, next action, start/target dates and last reviewed time.
- `tasks`: project-linked or standalone action, description, status, priority, due/completed dates and optional parent task.
- `activity_events`: append-only meaningful events such as task completed, decision changed, project resumed, or evidence attached.
- `decisions`: question/context, decision, rationale, status and decision date.
- `attachments`: metadata for evidence files; keep actual files in a controlled local data directory initially, not in the PostgreSQL row.
- `pending_changes`: proposed edits to vault-origin content or proposed metadata/relationships; base hash, diff/payload, status, and review metadata.
- `lookup_categories` and `lookup_values`: database-managed values used for UI dropdowns and validation.
- `ingestion_runs` and `extraction_cache`: explicit processing run history and cached derived results keyed by content hash, operation/prompt version and model.
- `sync_state`: file path, observed hash, last successful sync, parser version, status, error, and tombstone/reconciliation metadata.

## Relationship model

Use foreign keys for relationships where endpoints are known entity tables. For a flexible graph spanning multiple entity types, choose one of these approaches before implementation:

1. **Typed relation tables** (stronger database integrity, more joins), or
2. A central `graph_nodes` registry mapping `(entity_type, entity_id)` and a `relationships` table referencing graph-node IDs (flexible graph queries, but cross-table integrity needs service-level checks or triggers).

The draft ERD shows a graph-node registry to make graph traversal and relationship editing uniform. The service layer must validate that every graph node points to a real entity. Add tests for orphan prevention.

## Configurable enums / lookup values

Do not use Python-only enums or PostgreSQL native ENUMs for values users may add through the UI. Store values in database tables and expose them through API endpoints. A code enum may still be used for truly internal, fixed protocol values, but user-configurable note types, workflow states, task states, relationship types, source types, origins and review states should be lookup-backed.

Each lookup value should have:
- stable ID
- category key
- machine key (unique within category)
- display label
- description/help text
- active flag
- sort order
- system-provided flag
- created/updated timestamps

Deactivation is preferred to deletion once a value is referenced. API must reject an inactive value for new records while continuing to display it for historical records.

## Sync invariants

1. Sync is idempotent: repeating the same file event does not create duplicate notes, tags, or edges.
2. Content hashes are calculated from raw file bytes; parser version is tracked separately.
3. A changed file refreshes parsed data and invalidates only derived artefacts dependent on the old content hash.
4. A moved file preserves note identity when stable frontmatter ID is available.
5. Without stable ID, path changes must be treated as a possible rename and require conservative matching; do not silently merge on title alone.
6. Removed tags and wiki-links must be removed from current associations, while audit events can preserve history.
7. File deletion creates a tombstone and is visible for recovery/review.
8. Unrecognised frontmatter fields and relationship syntax are preserved or flagged, not discarded.
9. A database outage must not corrupt the vault; failed sync is retried.
10. Application proposals to edit vault-origin notes remain pending until the user explicitly applies them.

## Indexing

Initial search should use PostgreSQL full-text search and normal indexes. Add section-aware chunks and pgvector when document Q&A or semantic retrieval is implemented. Chunk by headings/sections where possible, not arbitrary character counts. Rebuild chunks transactionally for the changed source version.

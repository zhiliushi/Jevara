# ERD notes and implementation caveats

The draw.io file is a **logical first-pass ERD**, intended for review and discussion before migrations. It is not a guarantee that every field/table should be implemented in the first coding pass.

Important modelling choices to validate:
- `graph_nodes.entity_type/entity_id` is a polymorphic registry; SQL foreign keys cannot directly enforce the referenced row across multiple tables. Validate it in the service layer and tests, or choose explicit typed relationship tables if stronger database-level integrity is preferred.
- `lookup_values` should be referenced by semantic category as well as ID. The service layer must reject a task-status lookup value where a project status is expected.
- `chunks.embedding` is optional and requires the pgvector extension if implemented as a vector column. Do not enable it until semantic retrieval is needed.
- `relationship_evidence` must contain at least one valid evidence reference for AI-extracted factual edges. Manually asserted links may use a different validation rule.
- `notes` can include application-native notes and Obsidian-mirrored notes; use an explicit origin and unique vault path where applicable.
- Deletion should begin as a tombstone/reviewable event, not a hard delete.
- Avoid duplicating task status, knowledge workflow state, and review status; each answers a different question.

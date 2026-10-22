# Relationship types

Category key: `relationship_type`

These are candidate defaults. Keep the vocabulary small and specific; users may add values through the database-backed UI.

| Key | Label | Meaning |
|---|---|---|
| `related_to` | Related to | Broad association without a stronger claim |
| `part_of` | Part of | Source/node is a component of the target |
| `prerequisite_of` | Prerequisite of | Source concept is needed before the target |
| `uses` | Uses | Source entity uses the target |
| `contrasts_with` | Contrasts with | The items differ in a meaningful way |
| `supports` | Supports | Evidence or idea supports a claim/decision |
| `contradicts` | Contradicts | Evidence or claim conflicts with another |
| `mentioned_in` | Mentioned in | Entity is mentioned by a source/document |
| `derived_from` | Derived from | Item was derived from another item |
| `depends_on` | Depends on | Work or concept depends on the target |
| `applies_to` | Applies to | Knowledge is relevant to a project/context |

Store direction explicitly. Do not assume that every relationship is symmetric. Store evidence and provenance for extracted relationships.

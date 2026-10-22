# Database-managed enums and lookup values

These Markdown files document initial seed values. In the application, configurable values must be stored in PostgreSQL lookup tables and returned by the API. The UI must not hard-code these values.

Recommended generic schema:

- `lookup_categories`: `id`, `key`, `label`, `description`, `is_system`, timestamps
- `lookup_values`: `id`, `category_id`, `key`, `label`, `description`, `is_active`, `is_system`, `sort_order`, timestamps

Use a unique constraint on `(category_id, key)`. Prefer deactivation over deletion after a value is referenced. Seed values through a repeatable migration or seed script. User-added values should be created through a validated API endpoint.

These lists are proposed initial defaults, not immutable PostgreSQL ENUM definitions. Update the database values through the application; update the corresponding Markdown file in Git when the documented default vocabulary changes.

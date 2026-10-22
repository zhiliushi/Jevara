# Lookup maintenance notes

- Lookup values are database data, not hard-coded frontend options.
- Seed values must be repeatable and safe to run more than once.
- API responses should include IDs, keys, labels, active status and descriptions where available.
- The UI should fetch values at runtime and refresh after adding a new value.
- Existing records may reference deactivated values; show the historical label but prevent new assignment.
- Keep machine keys stable. Changing a display label should not require rewriting all linked records.
- For seeded values, `is_system=true` means the seed is application-provided; it does not mean the user can never deactivate it unless an explicit rule says so.

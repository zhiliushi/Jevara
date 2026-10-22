# Review statuses

Category key: `review_status`

| Key | Label | Purpose |
|---|---|---|
| `pending` | Pending | Awaiting user review |
| `approved` | Approved | User accepted the proposal |
| `rejected` | Rejected | User rejected the proposal |
| `deferred` | Deferred | Kept for later consideration |
| `applied` | Applied | Approved change was applied to its target |

Do not conflate `approved` with `applied`: a change may be approved but fail to apply because the underlying file changed.

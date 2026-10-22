# Processing statuses

Category key: `processing_status`

| Key | Label | Purpose |
|---|---|---|
| `not_requested` | Not requested | No extraction job has been requested |
| `queued` | Queued | Explicitly requested and waiting |
| `processing` | Processing | A worker is handling the request |
| `succeeded` | Succeeded | Completed successfully |
| `failed` | Failed | Processing failed and can be retried |
| `stale` | Stale | Source or prompt version changed, output may be outdated |

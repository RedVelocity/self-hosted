# Streamystats

This Compose stack runs the Streamystats web application, background job
server, and a PostgreSQL database with the VectorChord extension. PostgreSQL
data is stored in the named volume `vectorchord_data`; the UI is routed through
Traefik on the external `proxy` network.

## Requirements

- Docker Compose and an existing external Docker network named `proxy`.
- `SST_HOST`: hostname used by the Traefik router.
- `SESSION_SECRET`: secret used for sessions and, by default, the internal API
  key. Generate and keep it private.
- `NEXT_SERVER_ACTIONS_ENCRYPTION_KEY`: stable encryption key for the Next.js
  server actions; set it so deployments retain the same key.

The UI is published on host port `3030` (container port `3000`). Configure the
following database values in production instead of relying on the Compose
defaults:

| Variable | Default |
| --- | --- |
| `POSTGRES_USER` | `postgres` |
| `POSTGRES_PASSWORD` | `postgres` |
| `POSTGRES_DB` | `streamystats` |

`VERSION` defaults to `latest`. `INTERNAL_API_KEY` defaults to
`SESSION_SECRET`. The job server's synchronization schedules and polling
interval can also be overridden with the `CRON_*` and
`SESSION_POLL_INTERVAL_MS` variables defined in `compose.yaml`.

## Start

```sh
docker compose --env-file /path/to/stack.env -f streamystats/compose.yaml up -d
```

Keep the environment file and all secrets out of version control.

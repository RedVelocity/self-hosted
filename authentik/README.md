# Authentik

This Compose stack runs Authentik with PostgreSQL and Redis. The Authentik
server and worker depend on both backing services; database and media files are
stored under `${STACKS_PATH}/authentik/`.

## Requirements

- Docker Compose.
- Existing external Docker networks named `proxy` and `arrs_default`.
- The environment variables below, provided to Compose.

| Variable | Purpose |
| --- | --- |
| `STACKS_PATH` | Host directory for persistent Authentik data |
| `POSTGRES_USER` | PostgreSQL account used by Authentik |
| `POSTGRES_PASSWORD` | PostgreSQL password; keep it secret |
| `AUTHENTIK_SECRET_KEY` | Required Authentik secret key; keep it secret |
| `AUTHENTIK_HOST` | Hostname used by the Traefik router |

The server is published on host ports `9001` (HTTP) and `9444` (HTTPS), and
Traefik routes HTTPS traffic on the `proxy` network. Adjust the published ports
or router settings in `compose.yaml` if they conflict with your setup.

## Start

From the repository root, provide your Compose environment file and start the
stack:

```sh
docker compose --env-file /path/to/stack.env -f authentik/compose.yaml up -d
```

Do not commit the environment file or its secrets.

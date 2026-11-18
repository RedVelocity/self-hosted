# `homarr` Compose directory

Despite this directory's name, [`compose.yaml`](compose.yaml) currently runs
Dynacat (`panonim/dynacat:latest`), not Homarr. It uses the container name and
Traefik router `dynacat`, and mounts configuration and assets from
`${STACKS_PATH}/dynacat/`. Update the Compose file if this directory is meant
to deploy Homarr instead.

## Requirements and access

- Docker Compose and an existing external Docker network named `proxy`.
- `STACKS_PATH` and `TZ` in the Compose environment.
- The Compose file references `/etc/komodo/stacks/dynacat/.env` as its
  container `env_file`; for a non-Komodo deployment, edit that entry to point
  to the appropriate file (the local `.env` alternative is shown in a comment).

The dashboard is published on host port `8099` (container port `8080`). The
Traefik router hostname is currently hard-coded as `dash.esh.redvelo.city`.

## Start

```sh
docker compose --env-file /path/to/stack.env -f homarr/compose.yaml up -d
```

# Backrest

This Compose stack runs [Backrest](https://github.com/garethgeorge/backrest)
for managing Restic backups. It mounts backup and restore locations, the stack
directory, and Docker data into the container.

## Requirements

- Docker Compose and an existing external Docker network named `proxy`.
- `STACKS_PATH`: host path for Backrest data, configuration, cache, temporary
  files, and shared rclone configuration.
- `BACKUP_PATH` and `RESTORE_PATH`: host paths mounted at `/backups` and
  `/restore`.
- `BACKREST_HOST`: hostname used by the Traefik router.

The web UI is published on host port `9898` and routed through Traefik on the
`proxy` network. The container also mounts the Docker socket and Docker volume
directory; treat access to this service as privileged host administration.

## Start

```sh
docker compose --env-file /path/to/stack.env -f backrest/compose.yaml up -d
```

The Compose file sets the container timezone to `Europe/Madrid`. Change it in
`compose.yaml` if needed. Keep environment files and backup credentials out of
version control.

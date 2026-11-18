# Portainer

This Compose stack runs Portainer Business Edition and persists its data at
`${STACKS_PATH}/portainer/data`. It mounts the Docker socket so Portainer can
manage the local Docker environment; treat the UI as privileged host
administration.

## Requirements

- Docker Compose and an existing external Docker network named `proxy`.
- `STACKS_PATH`: host directory for Portainer data.
- `PORTAINER_HOST`: hostname used by the Traefik router.

Portainer is published on host port `9000` and is also routed by Traefik over
the `proxy` network.

## Start

```sh
docker compose --env-file /path/to/stack.env -f portainer/compose.yaml up -d
```

Keep the environment file and deployment-specific values out of version
control.

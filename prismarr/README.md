# Prismarr

This Compose stack runs [Prismarr](https://github.com/Shoshuo/Prismarr) and
persists its application data at `${STACKS_PATH}/prismarr/data`.

## Requirements

- Docker Compose and an existing external Docker network named `proxy`.
- `STACKS_PATH`: host directory for persistent application data.
- `PRISMARR_HOST`: hostname used by the Traefik router.

The web service listens on port `7070`, published as host port `7070`, and is
also routed by Traefik over the `proxy` network.

## Start

```sh
docker compose --env-file /path/to/stack.env -f prismarr/compose.yaml up -d
```

Keep the environment file and deployment-specific values out of version
control.

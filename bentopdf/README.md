# BentoPDF

This Compose stack runs the BentoPDF simple image and makes it available on
host port `8090` (container port `8080`). Traefik can route HTTPS requests to
it over the external `proxy` Docker network.

## Requirements

- Docker Compose and an existing external Docker network named `proxy`.
- `BENTOPDF_HOST`: hostname used by the Traefik router.
- `TZ`: timezone passed to the container.

## Start

```sh
docker compose --env-file /path/to/stack.env -f bentopdf/compose.yaml up -d
```

Set `BENTOPDF_HOST` to the hostname configured in DNS and Traefik. Do not
commit environment files containing deployment-specific values.

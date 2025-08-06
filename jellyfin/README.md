# Jellyfin with GPU Acceleration

This compose file deploys **Jellyfin** with optional GPU support (NVIDIA), connected to both:

- **`arrs_default`** network — for direct connectivity with your existing Jellyseerr app on Gluetun‑based [Arrs stack](https://github.com/RedVelocity/self-hosted/tree/main/arrs).
- **`proxy`** network — for exposure to a reverse proxy like [Traefik](https://doc.traefik.io/traefik/).

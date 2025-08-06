# Traefik Reverse Proxy for Self‑Hosted Stack

This subfolder provides a **Traefik v2/v3** reverse proxy implementation to front-end your Docker‑Compose applications.

Traefik automatically discovers containers, handles TLS (Let's Encrypt), and routes traffic via Docker labels—perfect for configurable self-hosted stacks.:contentReference[oaicite:1]{index=1}

---

## Setup Instructions

1. **Copy `traefik.yaml` to `./config/traefik.yaml`** and update essential variables (e.g. `DOMAIN`, email).
2. **Ensure Traefik network exists**:
   
   ```bash
   docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
   ```
   

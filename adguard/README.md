# 🧰 AdGuard Home + Traefik (Docker Compose Stack)

This Compose file deploys [**AdGuard Home**](https://github.com/AdguardTeam/AdGuardHome) — a powerful network-wide ad blocker and DNS server — behind a **Traefik reverse proxy**, with static IP networking and persistent data volumes.

---

## 🚀 Overview

**Stack Components:**
- **AdGuard Home** – DNS and ad-blocking service.
- **Traefik** – Handles HTTPS routing, TLS certificates, and domain routing.
- **Docker Networks**  
  - `proxy`: shared Traefik network (external).  
  - `static`: internal static network providing fixed IP `172.7.7.7`.

---

## 🌐 Traefik Integration

AdGuard Home is exposed securely through Traefik using the following labels:

```yaml
labels:
  - traefik.enable=true
  - traefik.http.routers.adguard.entrypoints=websecure
  - traefik.http.routers.adguard.rule=Host(`${AG_HOST}`)
  - traefik.http.routers.adguard.tls=true
  - traefik.http.services.adguard.loadbalancer.server.port=5002
```

## ⚙️ Environment Variables

Create a .env file in the same directory as your docker-compose.yml:

```bash
# Base path for stack data (absolute host path)
STACKS_PATH=/srv/docker/stacks

# Domain used by Traefik to access AdGuard Home
AG_HOST=adguard.example.com
```

---
## 🧾 Notes

- The static IP (172.7.7.7) allows consistent DNS access across containers.
- Traefik must already be running on the shared proxy network.
- DNS ports (53 TCP/UDP) require host-level binding; ensure no local DNS service conflicts.

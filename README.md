# 🏠 [RedVelocity](https://redvelo.city) Self-Hosted Stack

A collection of Docker Compose stacks for a complete **self-hosted home server** environment — including networking setup, reverse proxy (Traefik), ad blocking (AdGuard Home), media management (Arrs & Jellyfin), and more.

---

## 📦 Overview

This project provides ready-to-deploy Docker Compose configurations for a modular and privacy-focused self-hosted ecosystem.

Each stack is isolated but interconnected through shared Docker networks:
- **`proxy`** — shared reverse proxy network (for Traefik & routed services)
- **`static`** — internal static IP network (for services that need fixed addresses)

---

## 🧰 Prerequisites

Before deploying any containers, create the required **Docker networks**.

> ⚠️ Adjust the `--subnet` values to fit your environment and avoid conflicts with your LAN.

#### 1️⃣ Static Network (for containers with fixed IPs)

Use this network for services like DNS or VPN containers that benefit from stable internal IPs.

```bash
docker network create --driver=bridge --subnet=172.7.7.0/24 static
```

#### 2️⃣ Proxy Network (for containers behind a reverse proxy)

If you’re using Traefik to manage HTTPS, routing, and certificates:

```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```
---

### 🚀 Setup & Deployment

1. Create the Docker networks (from the prerequisites above).
2. Deploy the stacks in the following sequence to ensure dependencies start correctly:
   
| Order | Stack | Notes |
|--------|----------------|------------------------------------------------------------|
| 1️⃣ | **Traefik** | Core reverse proxy. Must be running first. |
| 2️⃣ | **AdGuard Home** | DNS and ad-blocking service. Routed through Traefik. |
| 3️⃣ | **Komodo** | Container Management and deployment |
| 4️⃣ | **Other stacks** | Deploy any remaining apps in any order using Komodo. |

---
### 🛠️ Troubleshooting

#### ❌ Error: *Error response from daemon: Pool overlaps with other one on this address space*

This means the subnet you tried to create overlaps with an existing Docker network or your LAN.

#### ✅ Solution

- Choose a different subnet range that doesn’t collide with your network or existing Docker networks.  
- Remove the conflicting networks if they are unused:

   ```bash
   docker network rm <conflicting-network>
   ```
- Recreate with new subnets:
   ```bash
   docker network create --driver=bridge --subnet=172.8.8.0/24 static
   docker network create --driver=bridge --subnet=172.6.6.0/24 proxy
   ```

#### ⚙️ DNS / Port Conflicts

- Ports 53 (DNS) and 80/443 (HTTP/HTTPS) may already be used by host services. Stop or reconfigure host services before binding these ports.

- If Traefik manages TLS, ensure it has access to ACME/DNS challenge credentials (e.g., Cloudflare API token) if using DNS-based challenge.

⚠️ **FULL DISCLOSURE:** This README is generated with the help of AI/GPT but the config is all mine.

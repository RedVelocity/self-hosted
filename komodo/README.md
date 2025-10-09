# 🦎 Komodo + Mongo Stack

This stack deploys **[Komodo](https://github.com/moghtech/komodo)** — a modern DevOps automation and management platform — along with its required **MongoDB** backend.  
It includes the **Komodo Core**, **Komodo Periphery**, and **MongoDB** services, designed for seamless orchestration under Docker Compose and optional Traefik HTTPS access.

---

## 🚀 Features

- 🧩 One-command deployment of **Komodo Core**, **Periphery**, and **Mongo**
- 🌐 Optional **Traefik** integration for HTTPS and routing
- 🗄️ Persistent MongoDB storage
- 🐳 Periphery with full Docker socket access for stack management
- ⚙️ Environment-variable–driven configuration

---
## 🧱 Prerequisites

Komodo uses a shared **`proxy`** network to communicate with other services (e.g., through Traefik or Gluetun-based stacks).  
Before deploying, ensure this network exists:

```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```

---

## ⚙️ Environment Variables

Create a `.env` file alongside your `docker-compose.yml`:

```bash
# MongoDB credentials
KOMODO_DB_USERNAME=komodo
KOMODO_DB_PASSWORD=supersecret

# Komodo settings
COMPOSE_KOMODO_IMAGE_TAG=latest
KOMODO_LOCAL_AUTH=true

# Paths
STACKS_PATH=/srv/docker
PERIPHERY_ROOT_DIRECTORY=/etc/komodo

# Logging
COMPOSE_LOGGING_DRIVER=local

# Timezone
TZ=Europe/Madrid
```

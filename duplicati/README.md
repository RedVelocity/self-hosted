### ⚠️⚠️⚠️ I have switched to **[Backrest](https://github.com/RedVelocity/self-hosted/tree/main/backrest)**

---

# 🧾 Duplicati Backup Stack

This stack deploys **[Duplicati](https://www.duplicati.com/)** using the **LinuxServer.io** image, with integrated Docker access via the `universal-docker` mod.  
It’s designed to back up your entire Docker stack safely — including configurations — while being accessible through Traefik.

---

## 📂 Directory Structure

```bash
${STACKS_PATH}/
├── duplicati/
│   ├── config/        # Duplicati config and database
│   ├── scripts/       # Custom pre/post backup scripts
│   └── docker-compose.yml
│   └── .env
├── portainer/
├── gluetun/
└── ...
```

---
## 🧱 Prerequisites

Komodo uses a shared **`proxy`** network to communicate with other services (e.g., through Traefik or Gluetun-based stacks).  
Before deploying, ensure this network exists:

```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```
---

## ⚙️ Environment Variables
Create a .env file in the same directory as your docker-compose.yml:

```bash
PUID=0                          # Use 0 (root) if you have permission issues
PGID=0
TZ=Europe/Madrid                # Change to your TZ

DUPLICATI_PASSWORD=your_admin_password
DUPLICATI_ENCRYPTION_KEY=your_encryption_key

STACKS_PATH=/srv/docker
BACKUP_PATH=/srv/backups

DUPLICATI_HOST=duplicati.example.com
```
---

## 🤖 Scripts

### pre-backup.sh
```bash
#!/bin/bash
echo "🔧 Stopping all containers except Duplicati..."

# Get running containers except Duplicati
containers=$(docker ps --format '{{.Names}}' | grep -v duplicati)

if [ -n "$containers" ]; then
  docker stop $containers
else
  echo "No other containers running."
fi
echo "✅ All other containers stopped."
```

### post-backup.sh
```bash
#!/bin/bash
echo "🚀 Starting containers (ensuring Gluetun is first)..."

# Start Gluetun first if stopped
if ! docker ps --format '{{.Names}}' | grep -q '^gluetun$'; then
  echo "Starting Gluetun..."
  docker start gluetun
  sleep 10  # Give it a few seconds to initialize
fi

# Then start other stopped containers except Duplicati and Gluetun
containers=$(docker ps -a --format '{{.Names}}' --filter "status=exited" | grep -Ev 'duplicati|gluetun')

if [ -n "$containers" ]; then
  docker start $containers
else
  echo "No other containers to start."
fi
echo "✅ All containers started successfully."
```

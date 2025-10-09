# 🧾 Duplicati Backup Stack

This stack deploys **[Duplicati](https://www.duplicati.com/)** using the **LinuxServer.io** image, with integrated Docker access via the `universal-docker` mod.  
It’s designed to back up your entire Docker stack safely — including configurations — while being accessible through Traefik.

---

## 🚀 Features

- 🌐 Web UI via **Traefik** (secure HTTPS reverse proxy)
- 🐳 Full Docker daemon access (via `/var/run/docker.sock`)
- 🧠 Pre/post backup scripting support
- 💾 Tailored for backup of all stack configurations
- ⚙️ Configurable through `.env` variables

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

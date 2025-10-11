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
# Custom variables
STACKS_PATH=/mnt/host/d/Docker/stacks

####################################
# 🦎 KOMODO COMPOSE - VARIABLES 🦎 #
####################################

## These compose variables can be used with all Komodo deployment options.
## Pass these variables to the compose up command using `--env-file komodo/compose.env`.
## Additionally, they are passed to both Komodo Core and Komodo Periphery with `env_file: ./compose.env`,
## so you can pass any additional environment variables to Core / Periphery directly in this file as well.

## Stick to a specific version, or use `latest`
COMPOSE_KOMODO_IMAGE_TAG=latest

## Note: 🚨 Podman does NOT support local logging driver 🚨. See Podman options here:
## `https://docs.podman.io/en/v4.6.1/markdown/podman-run.1.html#log-driver-driver`
COMPOSE_LOGGING_DRIVER=local # Enable log rotation with the local driver.

## DB credentials - Ignored for Sqlite
KOMODO_DB_USERNAME=admin
KOMODO_DB_PASSWORD=supersecret

## Configure a secure passkey to authenticate between Core / Periphery.
KOMODO_PASSKEY=supersecret

#=-------------------------=#
#= Komodo Core Environment =#
#=-------------------------=#

## Full variable list + descriptions are available here:
## 🦎 https://github.com/mbecker20/komodo/blob/main/config/core.config.toml 🦎

## Note. Secret variables also support `${VARIABLE}_FILE` syntax to pass docker compose secrets.
## Docs: https://docs.docker.com/compose/how-tos/use-secrets/#examples

## Used for Oauth / Webhook url suggestion / Caddy reverse proxy.
KOMODO_HOST=komo.example.com
## Displayed in the browser tab.
KOMODO_TITLE=Komodo
## Create a server matching this address as the "first server".
## Use `https://host.docker.internal:8120` when using systemd-managed Periphery.
KOMODO_FIRST_SERVER=https://periphery:8120
## Make all buttons just double-click, rather than the full confirmation dialog.
KOMODO_DISABLE_CONFIRM_DIALOG=true

## Rate Komodo polls your servers for
## status / container status / system stats / alerting.
## Options: 1-sec, 5-sec, 15-sec, 1-min, 5-min.
## Default: 15-sec
KOMODO_MONITORING_INTERVAL="15-sec"
## Rate Komodo polls Resources for updates,
## like outdated commit hash.
## Options: 1-min, 5-min, 15-min, 30-min, 1-hr.
## Default: 5-min
KOMODO_RESOURCE_POLL_INTERVAL="5-min"

## Used to auth incoming webhooks. Alt: KOMODO_WEBHOOK_SECRET_FILE
KOMODO_WEBHOOK_SECRET=supersecretsupersecret
## Used to generate jwt. Alt: KOMODO_JWT_SECRET_FILE
KOMODO_JWT_SECRET=supersecretjwt

## Enable login with username + password.
KOMODO_LOCAL_AUTH=true
## Disable new user signups.
KOMODO_DISABLE_USER_REGISTRATION=false
## All new logins are auto enabled
KOMODO_ENABLE_NEW_USERS=false
## Disable non-admins from creating new resources.
KOMODO_DISABLE_NON_ADMIN_CREATE=true
## Allows all users to have Read level access to all resources.
KOMODO_TRANSPARENT_MODE=false

## Time to live for jwt tokens.
## Options: 1-hr, 12-hr, 1-day, 3-day, 1-wk, 2-wk
KOMODO_JWT_TTL="1-day"

## OIDC Login
KOMODO_OIDC_ENABLED=false
## Must reachable from Komodo Core container
# KOMODO_OIDC_PROVIDER=https://oidc.provider.internal/application/o/komodo
## Change the host to one reachable be reachable by users (optional if it is the same as above).
## DO NOT include the `path` part of the URL.
# KOMODO_OIDC_REDIRECT_HOST=https://oidc.provider.external
## Your client credentials
# KOMODO_OIDC_CLIENT_ID= # Alt: KOMODO_OIDC_CLIENT_ID_FILE
# KOMODO_OIDC_CLIENT_SECRET= # Alt: KOMODO_OIDC_CLIENT_SECRET_FILE
## Make usernames the full email.
# KOMODO_OIDC_USE_FULL_EMAIL=true
## Add additional trusted audiences for token claims verification.
## Supports comma separated list, and passing with _FILE (for compose secrets).
# KOMODO_OIDC_ADDITIONAL_AUDIENCES=abc,123 # Alt: KOMODO_OIDC_ADDITIONAL_AUDIENCES_FILE

## Github Oauth
KOMODO_GITHUB_OAUTH_ENABLED=false
# KOMODO_GITHUB_OAUTH_ID= # Alt: KOMODO_GITHUB_OAUTH_ID_FILE
# KOMODO_GITHUB_OAUTH_SECRET= # Alt: KOMODO_GITHUB_OAUTH_SECRET_FILE

## Google Oauth
KOMODO_GOOGLE_OAUTH_ENABLED=false
# KOMODO_GOOGLE_OAUTH_ID= # Alt: KOMODO_GOOGLE_OAUTH_ID_FILE
# KOMODO_GOOGLE_OAUTH_SECRET= # Alt: KOMODO_GOOGLE_OAUTH_SECRET_FILE

## Aws - Used to launch Builder instances and ServerTemplate instances.
KOMODO_AWS_ACCESS_KEY_ID= # Alt: KOMODO_AWS_ACCESS_KEY_ID_FILE
KOMODO_AWS_SECRET_ACCESS_KEY= # Alt: KOMODO_AWS_SECRET_ACCESS_KEY_FILE

## Hetzner - Used to launch ServerTemplate instances
## Hetzner Builder not supported due to Hetzner pay-by-the-hour pricing model
KOMODO_HETZNER_TOKEN= # Alt: KOMODO_HETZNER_TOKEN_FILE

#=------------------------------=#
#= Komodo Periphery Environment =#
#=------------------------------=#

## Full variable list + descriptions are available here:
## 🦎 https://github.com/mbecker20/komodo/blob/main/config/periphery.config.toml 🦎

## Periphery passkeys must include KOMODO_PASSKEY to authenticate.
PERIPHERY_PASSKEYS=${KOMODO_PASSKEY}
## Specify the root directory used by Periphery agent.
PERIPHERY_ROOT_DIRECTORY=${STACKS_PATH}/komodo/periphery

## Enable SSL using self signed certificates.
## Connect to Periphery at https://address:8120.
PERIPHERY_SSL_ENABLED=false

## If the disk size is overreporting, can use one of these to 
## whitelist / blacklist the disks to filter them, whichever is easier.
## Accepts comma separated list of paths.
## Usually whitelisting just /etc/hostname gives correct size.
PERIPHERY_INCLUDE_DISK_MOUNTS=/etc/hostname
# PERIPHERY_EXCLUDE_DISK_MOUNTS=/snap,/etc/repos
```

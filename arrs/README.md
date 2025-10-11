# The Arrs Stack with Gluetun

This stack includes:

- **Gluetun** (VPN container)
- **qBittorrent**
- **Prowlarr**
- **Sonarr**
- **Radarr**
- **Jellyseerr**
- **FlareSolverr**

All `arrs` services route through the **Gluetun** VPN container for privacy.  
Optional **Traefik** labels are included for reverse proxy setups.

---

## Prerequisites

If you want Jellyseerr (running inside the Gluetun network) to communicate with other containers in your environment, you must have the **`proxy`** network created beforehand.

Run:

```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```
---

## Environment Variables
```bash
# Gluetun VPN config
VPN_SERVICE_PROVIDER=private internet access
OPENVPN_USER=XXXXXXXX
OPENVPN_PASSWORD=XXXXXXXX
SERVER_REGIONS=Germany
# Path to store arrs config
STACKS_PATH=/srv/docker
# Media path
DOWNLOADS_PATH=/srv/downloads
MOVIES_PATH=/srv/movies
TV_PATH=/srv/tvshows
# Base url for Traefik deployment
ARRS_HOST=example.com
```

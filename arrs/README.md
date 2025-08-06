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

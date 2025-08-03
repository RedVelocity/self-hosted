## Pre-Requisites

> Make sure to run these commands **before creating containers**

For containers with static IP's
```bash
docker network create --driver=bridge --subnet=172.7.7.0/24 static
```
For containers which are exposed to a reverse proxy like [Traefik](https://doc.traefik.io/traefik/)
```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```

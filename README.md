# Prerequisites

Before creating containers, you need to set up custom Docker networks.  
Adjust the `--subnet` values to fit your environment and avoid conflicts with your LAN.

### 1. For containers with static IPs
This network is useful when you want containers to have fixed IP addresses.

```bash
docker network create --driver=bridge --subnet=172.7.7.0/24 static
```

### 2. For containers behind a reverse proxy
For example, if you are using Traefik to route traffic.

```bash
docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
```

# Troubleshooting
```Error: Error response from daemon: Pool overlaps with other one on this address space```

This means your chosen subnet overlaps with an existing Docker network or your LAN. Choose a different subnet range.

### View existing networks:
```bash
docker network inspect <network_name>
```

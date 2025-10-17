# Traefik Reverse Proxy for Self‑Hosted Stack

This subfolder provides a **Traefik** reverse proxy implementation to front-end your Docker‑Compose applications.

Traefik automatically discovers containers, handles TLS (Let's Encrypt), and routes traffic via Docker labels—perfect for configurable self-hosted stacks.

---

## Setup Instructions

1. **Copy `traefik.yaml` to `./config/traefik.yaml`** and update essential variables (e.g. `DOMAIN`, email).
   The content must be as follows for Cloudflare DNS with wildcard certificates
```yaml
   ---
global:
  checkNewVersion: false
  sendAnonymousUsage: false

# --> (Optional) Change log level and format here ...
#     - level: [TRACE, DEBUG, INFO, WARN, ERROR, FATAL]
# log:
#  level: ERROR
# <--

# --> (Optional) Enable accesslog here ...
# accesslog: {}
# <--

# --> (Optional) Enable API and Dashboard here, don't do in production
# api:
#   dashboard: true
#   insecure: true

# -- Change EntryPoints here...
entryPoints:
  web:
    address: :80
    # --> (Optional) Redirect all HTTP to HTTPS
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https
    # <--
  websecure:
    address: :443
    http:
      tls:
        # Generate a wildcard domain certificate
        certResolver: cloudflare
        domains:
          - main: domain.xyz
            sans:
              - '*.domain.xyz'
          - main: sub.domain.xyz
            sans:
              - '*.sub.domain.xyz'

# -- Configure your CertificateResolver here...
certificatesResolvers:
  cloudflare:
    acme:
      email: abcxyz@gmail.com # <-- Change this to your email
      storage: /var/traefik/certs/acme.json
      caServer: 'https://acme-v02.api.letsencrypt.org/directory'
      dnsChallenge:
        provider: cloudflare # <-- (Optional) Change this to your DNS provider
        resolvers:
          - '1.1.1.1:53'
          - '8.8.8.8:53'

# --> (Optional) Disable TLS Cert verification check
#serversTransport:
#  insecureSkipVerify: true
# <--

providers:
  docker:
    exposedByDefault: false # <-- (Optional) Change this to true if you want to expose all services
    # Specify discovery network - This ensures correct name resolving and possible issues with containers, that are in multiple networks.
    # E.g. Database container in a separate network and a container in the frontend and database network.
    network: proxy
  file:
    directory: /etc/traefik
    watch: true
```
3. **Ensure Traefik network exists**:
   
   ```bash
   docker network create --driver=bridge --subnet=172.5.5.0/24 proxy
   ```
   

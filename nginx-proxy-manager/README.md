# Nginx Proxy Manager

Web-based reverse proxy with a GUI — handles SSL termination and subdomain routing for all homelab services without editing config files.

## Port Requirements

| Port | Purpose |
|---|---|
| 80 | HTTP (redirects to HTTPS) |
| 81 | NPM Admin UI |
| 443 | HTTPS (SSL termination) |

> ⚠️ Ports 80 and 443 must be free on the host. If running Pi-hole on the same machine, move Pi-hole's web UI to a different port or run them on separate hosts.

## Setup

### 1. Start

```bash
docker compose up -d
```

### 2. Log in to the admin UI

Go to `http://<server-ip>:81`

Default credentials:
- **Email:** `admin@example.com`
- **Password:** `changeme`

> ⚠️ Change these immediately.

### 3. Add a Proxy Host

1. Hosts → Proxy Hosts → Add Proxy Host
2. **Domain:** `service.yourdomain.com`
3. **Scheme:** `http`
4. **Forward Hostname/IP:** container name or LAN IP (e.g. `192.168.1.49`)
5. **Forward Port:** the service's internal port
6. SSL tab → Request a new SSL Certificate → Enable Force SSL

### 4. Port forward on your router

For external HTTPS access, forward ports **80** and **443** from your router to this server's LAN IP.

## Common Proxy Hosts

| Domain | Forward to | Port |
|---|---|---|
| `cloud.yourdomain.com` | `192.168.1.x` | `8082` |
| `paperless.yourdomain.com` | `192.168.1.x` | `8000` |
| `bookstack.yourdomain.com` | `192.168.1.x` | `6875` |

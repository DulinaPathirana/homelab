# Utilities

Infrastructure services that run alongside the main stacks. You can run all of them together or pick individual services.

## Services

| Container | Port | Purpose |
|---|---|---|
| Portainer | 9000 / 9443 | Docker GUI — manage all containers visually |
| Glance | 3005 | Minimal status dashboard |
| Organizr | 8888 | Tabbed media service portal |
| Monitorr | 8083 | Service uptime monitor |
| Watchtower | — | Auto-updates container images daily |
| Twingate | — | Zero-trust remote access (no VPN client needed) |
| Anisette | 6969 | Apple ID anisette data server |

## Setup

```bash
cp .env.example .env
# Fill in paths and Twingate tokens
docker compose up -d
```

To run only specific services:
```bash
docker compose up -d portainer glance watchtower
```

## Portainer

Go to `https://<server-ip>:9443` → create admin account on first visit.
Connect to the local Docker socket via **Environment → Local**.

## Twingate

Used for secure remote access to homelab services without exposing ports.

1. Sign up at [twingate.com](https://www.twingate.com) (free tier available)
2. Create a network → add a Remote Network → add a Connector
3. Copy the tokens into `.env`
4. Add Resources in the Twingate console pointing to your internal service IPs/ports

## Watchtower

Runs daily at midnight, pulls latest images, and restarts updated containers. The `--cleanup` flag removes old image layers automatically.

To trigger an immediate update check:
```bash
docker exec watchtower /watchtower --run-once
```

## Anisette

Provides Apple ID authentication data, used as a dependency for services like macless-haystack. No configuration needed beyond starting the container.

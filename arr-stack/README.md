# Arr Stack

Full media automation pipeline — all services routed through a **Gluetun VPN container** so no torrent traffic ever leaves unencrypted.

## Architecture

```
Internet
   │
   ▼
┌──────────┐   WireGuard/OpenVPN tunnel
│  Gluetun │ ──────────────────────────▶ NordVPN
└────┬─────┘
     │ network_mode: service:gluetun
     ├── qBittorrent  (downloads)
     ├── Sonarr       (TV automation)
     ├── Radarr       (movie automation)
     ├── Prowlarr     (indexer aggregator)
     ├── Jackett      (indexer proxy)
     └── FlareSolverr (Cloudflare bypass)
```

All arr services use `network_mode: service:gluetun` — if the VPN drops, they lose internet entirely (kill-switch by design).

## Services & Ports

| Container | Port | Purpose |
|---|---|---|
| qBittorrent | 8090 | WebUI — download client |
| Sonarr | 8989 | TV series automation |
| Radarr | 7878 | Movie automation |
| Prowlarr | 9696 | Unified indexer manager |
| Jackett | 9117 | Indexer proxy / Torznab |
| FlareSolverr | 8191 | Cloudflare CAPTCHA bypass |

## Setup

### 1. Get WireGuard credentials (NordVPN)

Log in at my.nordaccount.com → NordVPN → Set up NordVPN manually → WireGuard → copy your **Private Key**.

### 2. Configure

```bash
cp .env.example .env
# Set WIREGUARD_PRIVATE_KEY, paths, and HOST_INTERFACE
```

Find your NIC name:
```bash
ip link | grep -E '^[0-9]+:' | awk '{print $2}'
```

### 3. Start

```bash
docker compose up -d
# Verify VPN is working:
docker exec gluetun wget -qO- https://ipinfo.io
```

The IP returned should be a NordVPN server, not your home IP.

### 4. Connect arr apps to qBittorrent

In Sonarr/Radarr → Settings → Download Clients → Add qBittorrent:
- Host: `localhost`
- Port: `8090`

### 5. Add indexers via Prowlarr

Prowlarr → Indexers → Add → search for your indexer → Add to apps (Sonarr + Radarr auto-sync).

## Troubleshooting

| Issue | Fix |
|---|---|
| Gluetun unhealthy | Check `WIREGUARD_PRIVATE_KEY` is correct |
| Arr apps can't reach qBittorrent | Ensure they use `network_mode: service:gluetun` |
| Wrong NIC | Run `ip link` on host, set `HOST_INTERFACE` |
| Downloads not moving | Check `DOWNLOADS_PATH` matches volume in all services |

# 🏠 Homelab

A self-hosted infrastructure stack running across two home servers — built around privacy, automation, and full data ownership.

## Stack Overview

| Service | Category | Description |
|---|---|---|
| [Nextcloud](./nextcloud) | Cloud Storage | Self-hosted Google Drive / iCloud alternative |
| [Paperless-NGX](./paperless) | Documents | AI-powered document management and OCR |
| [BookStack](./bookstack) | Knowledge Base | Self-hosted wiki and documentation |
| [Arr Stack](./arr-stack) | Media Automation | Sonarr · Radarr · Prowlarr · Jackett · qBittorrent behind Gluetun VPN |
| [Pi-hole](./pihole) | DNS / Ad-blocking | Network-wide ad and tracker blocking |
| [Jellystat](./jellystat) | Monitoring | Jellyfin playback statistics |
| [Homepage](./homepage) | Dashboard | Unified service dashboard |
| [Nginx Proxy Manager](./nginx-proxy-manager) | Reverse Proxy | SSL termination and subdomain routing |
| [Utilities](./utilities) | Infrastructure | Portainer · Glance · Organizr · Monitorr · Watchtower · Twingate · Anisette |

## Quick Start

**Prerequisites:** Docker Engine ≥ 24, Docker Compose v2, Linux host

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/homelab.git
cd homelab/<service>

# 2. Configure
cp .env.example .env
# Edit .env with your values

# 3. Launch
docker compose up -d
```

## Generating Secrets

```bash
# Strong password / app key
openssl rand -base64 32

# Hex secret key (Paperless, BookStack)
openssl rand -hex 32
```

## Not Included

These require custom builds or account-specific setup:

| Service | Link |
|---|---|
| macless-haystack (AirTag network) | https://github.com/dchristl/macless-haystack |
| ClawdBot (Claude AI gateway) | https://openclaw.ai/ |

---

> All sensitive values are kept in `.env` files (git-ignored). Only `.env.example` templates are committed.

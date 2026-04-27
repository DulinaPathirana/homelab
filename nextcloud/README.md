# Nextcloud

Self-hosted cloud storage — replaces Google Drive / iCloud. Runs as PHP-FPM behind nginx, with Postgres for the database, Redis for caching, and a Cloudflare tunnel for HTTPS access without port-forwarding.

## Architecture

```
Internet
   │
   ▼
Cloudflare Tunnel (cloudflared)
   │
   ▼
nginx (nextcloud_web) :8082
   │  FastCGI (port 9000)
   ▼
nextcloud_app (FPM)
   ├── nextcloud_db  (Postgres 16)
   └── nextcloud_redis (Redis)

nextcloud_cron  ← runs background jobs every 5 min
```

## Files

| File | Purpose |
|---|---|
| `docker-compose.yml` | All services |
| `.env` | Domain, bind IP, data path |
| `db.env` | Postgres credentials (git-ignored) |
| `cloudflared.env` | Cloudflare tunnel token (git-ignored) |
| `nginx.conf` | Nextcloud-specific nginx config |

## Setup

### 1. Create the Cloudflare Tunnel

1. Go to [Cloudflare Zero Trust](https://one.dash.cloudflare.com) → Networks → Tunnels
2. Create a tunnel → Docker → copy the token
3. Add a Public Hostname: `cloud.yourdomain.com` → Service: `http://nextcloud_web:80`

### 2. Configure env files

```bash
# Main env
cp .env.example .env

# Database credentials
cp db.env.example db.env

# Cloudflare tunnel token
cp cloudflared.env.example cloudflared.env
```

Edit all three files with your values.

### 3. Create data directory

```bash
sudo mkdir -p /mnt/storage/nextcloud
sudo chown -R www-data:www-data /mnt/storage/nextcloud
```

### 4. Start

```bash
docker compose up -d
# Watch logs until healthy:
docker compose logs -f nextcloud_app
```

First boot takes ~2 min while Nextcloud initialises the database.

### 5. Create admin account

Navigate to `https://cloud.yourdomain.com` → follow the setup wizard.

## Useful Commands

```bash
# Run occ commands (Nextcloud CLI)
docker exec -u www-data nextcloud_app php occ <command>

# Trigger a file scan
docker exec -u www-data nextcloud_app php occ files:scan --all

# Check background job status
docker exec -u www-data nextcloud_app php occ background:cron
```

## Troubleshooting

| Issue | Fix |
|---|---|
| Cloudflared keeps restarting | Verify `TUNNEL_TOKEN` in `cloudflared.env` |
| Trusted domain error | Check `NEXTCLOUD_DOMAIN` matches your actual domain |
| Slow uploads | Increase `client_max_body_size` in `nginx.conf` |
| Redis not connecting | Check `nextcloud_redis` is healthy: `docker ps` |

# Jellystat

Playback statistics dashboard for Jellyfin — tracks watch history, user activity, and library stats with charts and reports.

## Services

| Container | Purpose |
|---|---|
| `jellystat` | Web UI (port 3000) |
| `jellystat-db` | Postgres — stores playback history |

## Setup

### 1. Configure

```bash
cp .env.example .env
# Set DB_PASSWORD, JWT_SECRET, and DB_PATH
```

Generate JWT secret:
```bash
openssl rand -hex 32
```

### 2. Start

```bash
docker compose up -d
```

### 3. Connect to Jellyfin

1. Go to `http://<server-ip>:3000`
2. Complete the setup wizard
3. Enter your Jellyfin server URL (e.g. `http://jellyfin:8096` if on same host, or the LAN IP)
4. Enter a Jellyfin **API key** — generate one in Jellyfin → Dashboard → API Keys → + button

Jellystat will begin syncing historical playback data on first connect.

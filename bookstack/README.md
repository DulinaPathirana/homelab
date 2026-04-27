# BookStack

Self-hosted wiki and documentation platform. Organises content into **Shelves → Books → Chapters → Pages** — great for runbooks, project notes, and internal docs.

## Services

| Container | Image | Purpose |
|---|---|---|
| `bookstack` | linuxserver/bookstack | Web app (PHP/Laravel) |
| `bookstack_db` | linuxserver/mariadb | Database |

**Port:** `6875` (HTTP)

## Setup

### 1. Generate APP_KEY

```bash
echo "base64:$(openssl rand -base64 32)"
```

Copy the output into `BOOKSTACK_APP_KEY` in your `.env`.

### 2. Configure

```bash
cp .env.example .env
# Edit BOOKSTACK_URL, APP_KEY, DB passwords, and paths
```

### 3. Start

```bash
docker compose up -d
```

### 4. Log in

Default credentials:
- **Email:** `admin@admin.com`
- **Password:** `password`

> ⚠️ Change these immediately after first login.

## Backup

```bash
# Database
docker exec bookstack_db mysqldump -u bookstack -p bookstack > bookstack_backup.sql

# App files (uploads, attachments)
tar -czf bookstack_config.tar.gz /opt/bookstack/config
```

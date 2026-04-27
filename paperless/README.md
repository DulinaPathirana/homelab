# Paperless-NGX

AI-powered document management — scan or drop documents into the consume folder and Paperless automatically OCRs, tags, and indexes them for full-text search.

## Architecture

```
consume/ ← drop PDFs/images here (or auto-import from scanner)
   │
   ▼
paperless-webserver
   ├── paperless-tika      (text extraction from Office docs)
   ├── paperless-gotenberg (PDF rendering)
   ├── paperless-broker    (Redis — task queue)
   └── paperless-db        (Postgres — document index)
```

## Services

| Container | Purpose |
|---|---|
| `paperless-webserver-1` | Web UI + API (port 8000) |
| `paperless-db-1` | Postgres — stores document metadata |
| `paperless-broker-1` | Redis — background task queue |
| `paperless-tika-1` | Apache Tika — extracts text from Word/Excel/etc |
| `paperless-gotenberg-1` | PDF conversion service |

## Setup

### 1. Configure

```bash
cp .env.example .env
# Set PAPERLESS_URL, PAPERLESS_SECRET_KEY, DB password, and data path
```

### 2. Create data directories

```bash
mkdir -p /mnt/storage/paperless-ngx/{data,media,consume,export}
```

### 3. Start

```bash
docker compose up -d
```

### 4. Create superuser

```bash
docker exec -it paperless-webserver-1 python3 manage.py createsuperuser
```

Then go to `http://localhost:8000`.

## Ingesting Documents

**Drop files:** Copy any PDF, image, or Office doc into the `consume/` directory — Paperless processes it automatically within ~30 seconds.

**Email import:** Configure a mail account in Settings → Mail → Add mail account.

**Mobile scan:** Use the [Paperless Mobile app](https://github.com/astubenbord/paperless-mobile) (Android/iOS).

## Backup

```bash
# Export all documents + metadata to the export folder
docker exec paperless-webserver-1 document_exporter /usr/src/paperless/export

# Then archive the export folder
tar -czf paperless_backup_$(date +%F).tar.gz /mnt/storage/paperless-ngx/export
```

# Pi-hole

Network-wide DNS-based ad and tracker blocker. Acts as the DNS server for your entire LAN — no per-device configuration needed.

## Port Requirements

| Port | Protocol | Purpose |
|---|---|---|
| 53 | TCP + UDP | DNS — must be open on the host |
| 80 | TCP | Web UI (HTTP) |
| 443 | TCP | Web UI (HTTPS) |

> ⚠️ If port 80 or 443 is already in use (e.g. by Nginx Proxy Manager), either run Pi-hole on a different host or change NPM's ports.

## Setup

### 1. Ensure port 53 is free

On Ubuntu, `systemd-resolved` occupies port 53 by default. Disable it:

```bash
sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved
# Set a fallback DNS so you don't lose connectivity
echo "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf
```

### 2. Configure

```bash
cp .env.example .env
# Set PIHOLE_PASSWORD and PIHOLE_DATA_PATH
```

### 3. Start

```bash
docker compose up -d
```

### 4. Access the dashboard

Go to `http://<server-ip>/admin` and log in with `PIHOLE_PASSWORD`.

### 5. Point your router's DNS to Pi-hole

In your router admin panel → DHCP/DNS settings → set Primary DNS to your Pi-hole server's IP.

## Recommended Blocklists

In Pi-hole → Adlists, add:

```
https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts
https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/pro.txt
```

Then run `pihole -g` (or Tools → Update Gravity) to apply.

## Useful Commands

```bash
# Update gravity (blocklists)
docker exec pihole pihole -g

# View query log
docker exec pihole pihole -t

# Whitelist a domain
docker exec pihole pihole -w example.com
```

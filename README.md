## 👋 Welcome to technitium 🚀

Open-source DNS server with advanced features

## 📋 Description

Open-source DNS server with advanced features

## 🚀 Services

- **app**: casjaysdevdocker/technitium:latest

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/technitium/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/technitium" ~/.local/srv/docker/technitium
cd ~/.local/srv/docker/technitium
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install technitium
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=...
BASE_HOST_NAME=...
BASE_DOMAIN_NAME=...
APP_ADMIN_PASS=...
DNS_SERVER_FORWARDERS=...
```

See `docker-compose.yaml` for the complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

The stack also boots with the inline defaults in `docker-compose.yaml`, so no
env file is strictly required.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64365` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no web port; the web UI is reached only through the
proxy network. All three files publish the DNS service ports on all host
interfaces, because they are not HTTP: `53` (tcp and udp, DNS), `67` (udp,
DHCP), `443` (tcp, DNS-over-HTTPS), `853` (tcp, DNS-over-TLS) and `53443`
(tcp, DNS-over-HTTPS). Port `67` only works as a DHCP server with host
networking; port `443` conflicts with any web server already on the host.

## 🌐 Access

- **Web Interface**: <http://172.17.0.1:64365>

## 📂 Volumes

- `./volumes/config/technitium` - DNS zones and server configuration

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖
🦄 composemgr: [Github](https://github.com/composemgr) 🦄

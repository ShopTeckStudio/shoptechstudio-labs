# cloudflared

Cloudflare Tunnel daemon for the 5150-lab homelab stack. Routes external traffic securely to internal services without exposing a public IP or opening firewall ports.

## What It Does

- Creates an outbound-only tunnel from the homelab to Cloudflare's edge network
- Allows external access to internal services via subdomains (e.g. `nextcloud.5150-lab.com`) without port forwarding
- Works alongside Nginx Proxy Manager — Cloudflare handles external ingress, NPM handles internal routing
- Eliminates the need for Tailscale split DNS or dynamic DNS workarounds for remote access

## Stack Info

| Property | Value |
|---|---|
| Image | `cloudflare/cloudflared:latest` |
| Container | `cloudflared` |
| Network | `proxy` (external) |
| Host | `192.168.4.200` (M4 Mac mini) |

## Prerequisites

1. A domain managed by Cloudflare (e.g. `5150-lab.com`)
2. A Cloudflare account with the domain added
3. A Cloudflare Tunnel created via the dashboard or CLI

## Setup

### 1. Create the Tunnel (Cloudflare Dashboard)

1. Go to **Cloudflare Dashboard → Zero Trust → Networks → Tunnels**
2. Click **Create Tunnel**
3. Name it (e.g. `5150-lab-tunnel`)
4. Copy the tunnel token — you'll need it for the compose file

### 2. Configure Public Hostnames in Cloudflare

For each service you want to expose, add a public hostname in the tunnel config:

| Subdomain | Service | URL |
|---|---|---|
| `nextcloud` | Nextcloud | `http://192.168.4.200:8080` |
| `jellyfin` | Jellyfin | `http://192.168.4.200:8096` |

> **Note:** Only expose services you intend to be publicly accessible. Keep admin services like Portainer, Grafana, Pi-hole, and Uptime Kuma off the tunnel — access those via Tailscale only.

### 3. Deploy

```bash
docker compose up -d
```

## docker-compose.yml

```yaml
services:
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: cloudflared
    restart: unless-stopped
    command: tunnel --no-autoupdate run
    environment:
      TUNNEL_TOKEN: ${TUNNEL_TOKEN}
    networks:
      - proxy

networks:
  proxy:
    external: true
```

## .env

```env
TUNNEL_TOKEN=your-tunnel-token-here
```

> ⚠️ Never commit `.env` to Git. Add it to `.gitignore`.

## Architecture

```
External User
     │
     ▼
Cloudflare Edge (DNS + Proxy)
     │
     ▼
cloudflared (outbound tunnel)
     │
     ▼
Nginx Proxy Manager (proxy network)
     │
     ▼
Internal Services (Nextcloud, Jellyfin, etc.)
```

## What to Expose vs. Keep Private

| Service | Expose via Tunnel | Access Method |
|---|---|---|
| Nextcloud | ✅ Yes | Public via tunnel |
| Jellyfin | ✅ Yes | Public via tunnel |
| Portainer | ❌ No | Tailscale only |
| Grafana | ❌ No | Tailscale only |
| Pi-hole | ❌ No | Tailscale only |
| Uptime Kuma | ❌ No | Tailscale only |
| Home Assistant | ⚠️ Optional | Tailscale preferred |
| Prometheus | ❌ No | Tailscale only |

## Notes

- The tunnel token is sensitive — store it in your secrets tracker and `.env` file only
- Cloudflare handles SSL automatically for tunnel-exposed services — no cert management needed on the NPM side for those
- NPM access lists still apply for local network access regardless of tunnel config
- After setup, update Pi-hole local DNS records if subdomains change

## Related Stacks

- [`nginx-proxy-manager`](../nginx-proxy-manager/README.md) — internal SSL termination and routing
- [`nextcloud`](../nextcloud/README.md) — primary service exposed via tunnel


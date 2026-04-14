# 🌐 cloudflared

> Cloudflare Tunnel daemon for a self-hosted homelab stack.
> Routes external traffic securely to internal services — no open ports, no exposed home IP.

![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

---

## 📌 What It Does

- 🔒 Creates an **outbound-only tunnel** from the homelab to Cloudflare's edge — no port forwarding required
- 🌍 Enables external access to internal services via clean subdomains (`nextcloud.yourdomain.com`)
- 🤝 Works alongside **Nginx Proxy Manager** — Cloudflare handles external ingress, NPM handles internal routing
- 🚫 Eliminates the need for Tailscale split DNS or dynamic DNS workarounds for remote access

---

## 🖥️ Stack Info

| Property | Value |
|---|---|
| 🐳 Image | `cloudflare/cloudflared:latest` |
| 📦 Container | `cloudflared` |
| 🌐 Network | `proxy` (external) |
| 🖥️ Host | `your-server-ip` |

---

## ✅ Prerequisites

- 🌐 A domain managed by Cloudflare (e.g. `yourdomain.com`)
- 👤 A Cloudflare account with the domain added
- 🔑 A Cloudflare Tunnel token (created via dashboard)

---

## 🚀 Setup

### 1️⃣ Create the Tunnel (Cloudflare Dashboard)

1. Go to **Cloudflare Dashboard → Zero Trust → Networks → Tunnels**
2. Click **Create Tunnel**
3. Name it (e.g. `homelab-tunnel`)
4. Select **Docker** as the environment
5. Copy the tunnel token — you'll need it for the `.env` file

### 2️⃣ Configure Public Hostnames

For each service to expose, add a public hostname in the tunnel config:

| 🌐 Subdomain | 🛠️ Service | 🔗 Internal URL |
|---|---|---|
| `nextcloud.yourdomain.com` | Nextcloud | `http://your-server-ip:8080` |
| `jellyfin.yourdomain.com` | Jellyfin | `http://your-server-ip:8096` |

> ⚠️ **Only expose services intended to be public.** Admin services like Portainer, Grafana, Pi-hole, and Uptime Kuma should stay off the tunnel — access those via Tailscale or VPN only.

## 3️⃣ ⚡ Quick Setup (Copy & Paste)

Run on your Docker host:

```
cd ~/docker
mkdir cloudflared
cd cloudflared
touch docker-compose.yml
touch .env
```

Then edit the compose file

```
nano docker-compose.yml
```

Input contents from the compose file.
---



---

## 🔐 .env

Create the .env file

```
nano .env
```

```env
TUNNEL_TOKEN=your-tunnel-token-here
```

> ⚠️ Never commit `.env` to Git. Add it to `.gitignore` and store the token in a password manager or secrets tracker.

---

### 3️⃣ Deploy

```bash
docker compose up -d
```

---
## 🏗️ Architecture

```
🌍 External User
       │
       ▼
☁️  Cloudflare Edge  (DNS + Proxy + SSL)
       │
       ▼
🔒 cloudflared  (outbound tunnel — no open ports)
       │
       ▼
🔀 Nginx Proxy Manager  (proxy network, SSL termination)
       │
       ▼
🛠️  Internal Services  (Nextcloud, Jellyfin, etc.)
```

---

## 🔓 What to Expose vs. Keep Private

| Service | Via Tunnel | Access Method |
|---|---|---|
| ☁️ Nextcloud | ✅ Yes | Public via tunnel |
| 🎬 Jellyfin | ✅ Yes | Public via tunnel |
| 🐳 Portainer | ❌ No | VPN / Tailscale only |
| 📊 Grafana | ❌ No | VPN / Tailscale only |
| 🕳️ Pi-hole | ❌ No | VPN / Tailscale only |
| 📡 Uptime Kuma | ❌ No | VPN / Tailscale only |
| 🏠 Home Assistant | ⚠️ Optional | Tailscale preferred |
| 📈 Prometheus | ❌ No | VPN / Tailscale only |

---

## 📝 Notes

- 🔑 The tunnel token is sensitive — store in `.env` only, never in the compose file directly
- 🔒 Cloudflare handles SSL automatically for tunnel-exposed services — no extra cert management needed
- 🛡️ NPM access lists still apply for local network access regardless of tunnel config
- 📋 After setup, update your local DNS records (Pi-hole etc.) if any subdomains change
- 🔄 `--no-autoupdate` is set intentionally — update the image tag manually to control versions

---

## 🔗 Related Stacks

| Stack | Description |
|---|---|
| [`nginx-proxy-manager`](../nginx-proxy-manager/README.md) | 🔀 Internal SSL termination and routing |
| [`nextcloud`](../nextcloud/README.md) | ☁️ Primary service exposed via tunnel |
| [`jellyfin`](../jellyfin/README.md) | 🎬 Media server exposed via tunnel |

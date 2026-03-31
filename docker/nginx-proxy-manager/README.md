# 🌐 Nginx Proxy Manager

This folder contains a Docker-based Nginx Proxy Manager (NPM) stack used as a reverse proxy within a homelab environment.

NPM provides a clean web UI for managing proxy hosts, SSL certificates, and access control — no manual config file editing required.

---

## 🔍 What This Stack Does

- Reverse proxy for internal services via web UI
- Automatic SSL certificates via Let's Encrypt
- Central entry point for homelab applications
- Access control lists and basic authentication
- Scalable foundation for Cloudflare Tunnel integration

---

## 📁 Repository Structure

```
docker/
├── monitoring/
├── nginx-proxy-manager/
│   ├── docker-compose.yml
│   ├── data/
│   └── letsencrypt/
└── pihole/
```

This repository structure is for documentation and organization.  
Your Docker host should only contain files referenced by `docker-compose.yml`.

- `data` stores NPM's database, config, and proxy host definitions — populated automatically on first run
- `letsencrypt` stores SSL certificates — populated automatically when you create a proxy host with SSL

---

## ⚡ Quick Setup (Copy & Paste)

Run on your Docker host:

```
cd docker
mkdir nginx-proxy-manager
cd nginx-proxy-manager

mkdir -p data letsencrypt

touch docker-compose.yml
```

---

## ⚙️ Docker Compose Configuration

Edit the compose file:

```
nano docker-compose.yml
```

Paste in the complete `docker-compose.yml`, then save and exit:

- CTRL + O → Enter
- CTRL + X

---

## ▶️ Start the Stack

```
docker compose up -d
```

Check status:

```
docker compose ps
```

---

## 🌐 Access

Open in your browser:

```
http://<server-ip>:81    or the appropriate port you mapped.
```

Replace `<server-ip>` with the IP address of the machine running the container.

## 🔑 Default login credentials:

- Email: admin@example.com
- Password: changeme

You will be prompted to change both on first login.

## ➕ Adding Proxy Hosts
All proxy configuration is done through the web UI — no config files needed. For each service, you'll enter:

- Domain name — e.g. homeassistant.local or nextcloud.yourdomain.com e.g. Pi-hole
- Forward hostname/IP — the container name or local IP of the service e.g. pihole
- Forward port — the service's port e.g. 8082

### Websockets Support
Some services require Websockets enabled in the NPM proxy host Details tab:

- If the service has real-time updates or a live dashboard — websockets on. 
- If it's just a static web UI you click around in — websockets off.
---

## 🔒 SSL Certificates
NPM handles SSL automatically via Let's Encrypt. For a homelab setup using a custom domain, use the DNS Challenge method with Cloudflare:

- Go to Certificates → Add SSL Certificate → Let's Encrypt via DNS
- Enter *.yourdomain.com and yourdomain.com as domain names
- Select Cloudflare as the DNS provider
- Paste your Cloudflare API token in the credentials field in this format:

⚠️Email on your NPM account must be a real email — not the default admin@example.com — or Let's Encrypt cert requests will fail

Cloudflare API token
dns_cloudflare_api_token=yourTokenHere

-Hit Save — NPM will issue the wildcard cert automatically

Once issued, assign the cert to each proxy host under the SSL tab and enable Force SSL.
- ℹ️ A wildcard cert (*.yourdomain.com) covers all subdomains — you only need to issue it once.

---

## 🔀 Docker Network — Proxy Connectivity
NPM can only route traffic to services it can reach on the Docker network. If a service is on an isolated network (its own compose stack), NPM won't be able to reach it until it's connected.

# Option 1 — Quick Fix (Live Only)

Connect NPM to a service's network manually:

```
docker network connect <network_name> nginx-proxy-manager
```

⚠️ This does not persist across container restarts. Use Option 2 to make it permanent.

# Option 2 — Shared Proxy Network (Recommended)

Create a shared external network that all services and NPM join: 
- Only required one time ever.

```
docker network create proxy
```

# Add the following to each service's docker-compose.yml:

- Must be done for each container you add.

```
networks:
  - proxy

networks:
  proxy:
    external: true
```

# Add the each service to the proxy network: 

Only one time ever for each container, not required if you ever restart the container as long as you updated the compose file and added it to the proxy network.

```
docker network connect proxy nginx-proxy-manager
docker network connect proxy <service-container-name>
```

---

## 🔁 Updating Configuration

Changes are made live through the UI. To restart the stack if needed:

```
docker compose restart nginx-proxy-manager
```

---

## 📊 Logs

Logs are stored locally:

```
 docker/nginx-proxy-manager/data/logs/
```

---

## 🧱 Design Approach

- UI-driven configuration — no manual nginx config editing
- Persistent data handling via mounted volumes
- Easy SSL management built in
- Simple to expand as new services are added

---

## 🧰 Part of Homelab Stack

This stack integrates with:

- Monitoring (Prometheus + Grafana)
- DNS (Pi-hole)
- Smart Home (Home Assistant)
- Cloud (Nextcloud)
- Remote access (Tailscale + Cloudflare Tunnel)

---

## Troubleshooting

To clear your dns if you are haveing troubles getting into one (macOS only).

```
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```
### Nextcloud
Nextcloud requires additional configuration to work behind a reverse proxy, or it will redirect back to the raw IP address.

Run these commands after setting up the proxy host:
```bash
docker exec nextcloud php occ config:system:set trusted_domains 2 --value=nextcloud.yourdomain.com
docker exec nextcloud php occ config:system:set overwritehost --value=nextcloud.yourdomain.com
docker exec nextcloud php occ config:system:set overwriteprotocol --value=https
docker exec nextcloud php occ config:system:set trusted_proxies 0 --value=<npm-docker-ip>    ## UPDATE
docker exec nextcloud php occ config:system:set trusted_proxies 1 --value=<npm-docker-ip>    ## UPDATE
docker restart nextcloud
```

Get the NPM Docker IP with:

```
docker inspect nginx-proxy-manager | grep IPAddress
```

> ⚠️ Also make sure Nextcloud is connected to the proxy network — a 502 error from NPM usually means the containers can't reach each other.

## 📝 Optional Commands (Quick Reference)

### Stop the stack
```
cd docker/nginx-proxy-manager
docker compose down
```

### Start the stack
```
cd docker/nginx-proxy-manager
docker compose up -d
```

### Restart the stack
```
docker compose down && docker compose up -d
```

Use this repeatedly to clear the file line by line before pasting new content. A serious time saver if you need to make a lot of change, paste an entire new file in, need to replace an entire line!
```
CTRL + K
```

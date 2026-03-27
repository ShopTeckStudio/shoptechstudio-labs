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


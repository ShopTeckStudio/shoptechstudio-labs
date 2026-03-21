# 🌐 NGINX Stack

This folder contains a Docker-based NGINX stack used as a reverse proxy and static file server within a homelab environment.

The stack is designed to be modular, lightweight, and easy to rebuild, while providing a clean foundation for routing traffic to internal services.

---

## 🔍 What This Stack Does

- Reverse proxy for internal services
- Static file hosting (HTML and assets)
- Central entry point for homelab applications
- Persistent logging for debugging and monitoring
- Scalable foundation for SSL and multi-service routing

---

## 📁 Repository Structure

```
docker/
├── monitoring/
├── nginx/
│   ├── docker-compose.yml
│   ├── config/
│   │   ├── nginx.conf
│   │   ├── conf.d/
│   │   │   └── default.conf
│   │   └── certs/
│   ├── html/
│   │   └── index.html
│   └── logs/
└── pihole/
```

This repository structure is for documentation and organization.  
Your Docker host should only contain files referenced by `docker-compose.yml`.

- `config/nginx.conf` contains the main NGINX configuration
- `config/conf.d/default.conf` site-specific configuration (reverse proxy + static site rules)
- `config/certs/` stores SSL certificates
- `html/index.html` contains static web files
- `logs/` stores persistent access and error logs, populated automatically by Nginx on first run

---

## ⚡ Quick Setup (Copy & Paste)

Run on your Docker host:

```
cd docker
mkdir nginx
cd nginx

mkdir -p config/conf.d config/certs html logs

touch docker-compose.yml
touch config/nginx.conf
touch config/conf.d/default.conf
touch html/index.html
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

## ⚙️ NGINX Configuration

Edit the main configuration file:

```
nano config/nginx.conf
```

Paste in the complete `nginx.conf`, then save and exit:

- CTRL + O → Enter
- CTRL + X

---

## ⚙️ Site Configuration

Edit the site config file:

Two things must be updated before running:

`server_name` — replace IP with your actual local IP, i.g. 192.168.1.199

`proxy_pass` — replace http://app:3000 with your upstream container name and port when you're ready to proxy something, i.g. http://myappname:3001  or   http://nextcloud:80

```
nano config/conf.d/default.conf
```

Paste in the complete `default.conf`, then save and exit:
- CTRL + O → Enter
- CTRL + X


---

## 📄 Test Page

Edit the HTML file:

```
nano html/index.html
```

Paste in the complete `index.html`, then save and exit:

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
http://<server-ip>
```

Replace `<server-ip>` with the IP address of the machine running the container.

---

## 🔁 Updating Configuration

After making changes:

```
docker compose restart nginx
```

---

## 📊 Logs

Logs are stored locally:

```
docker/nginx/logs/
```

---

## ➕ Future Enhancements

- SSL with Let's Encrypt
- Reverse proxy to internal services
- Domain-based routing
- Integration with monitoring stack
- NGINX metrics and dashboards

---

## 🧱 Design Approach

- Minimal configuration
- Clear file structure
- Persistent data handling
- Easy to expand into a full reverse proxy system

---

## 🧰 Part of Homelab Stack

This stack integrates with:

- Monitoring (Prometheus + Grafana)
- DNS (Pi-hole)
- Smart Home (Home Assistant)
- Remote access (Tailscale)

---


## 📝 Optional Commands (Quick Reference)

### Stop the stack
```
cd docker/nginx
docker compose down
```

### Start the stack
```
cd docker/nginx
docker compose up -d
```

Use this repeatedly to clear the file line by line before pasting new content. A serious time saver if you need to make a lot of change, paste an entire new file in, need to replace an entire line!
```
CTRL + K
```

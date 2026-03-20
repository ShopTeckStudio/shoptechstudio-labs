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
│   ├── html/
│   └── logs/
├── pihole/
```

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
│   ├── html/
│   └── logs/
├── pihole/
```

This repository structure is for documentation and organization.
Your Docker host should only contain files referenced by `docker-compose.yml`.

---

## 💻 Expected Local Host Structure

```
docker/nginx/
├── docker-compose.yml
├── config/
│   └── default.conf
├── html/
│   └── index.html
└── logs/
```

- `config/` contains NGINX configuration files
- `html/` contains static web files
- `logs/` stores persistent access and error logs

---

## ⚡ Quick Setup (Copy & Paste)

Run on your Docker host:

```
cd docker
mkdir nginx
cd nginx

mkdir config html logs

touch docker-compose.yml
touch config/default.conf
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

Edit config/default file:

```
nano config/default.conf
```

Paste in the complete `default.conf`, then save and exit:

- CTRL + O → Enter
- CTRL + X

---

## 📄 Test Page

Edit html/index file:

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

Open in browser:


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
nginx/logs/
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


## ⚙️ Optional Commands

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

Use this to completely delete all text in the `docker-compose.yml` if you are replacing everything.
```
CTRL + K
```


---

## 💻 Expected Local Host Structure

```
docker/nginx/
├── docker-compose.yml
├── config/
│   └── default.conf
├── html/
│   └── index.html
└── logs/
```

- `config/` contains NGINX configuration files
- `html/` contains static web files
- `logs/` stores persistent access and error logs

---

## ⚡ Quick Setup (Copy & Paste)

Run on your Docker host:

```
cd docker
mkdir nginx
cd nginx

mkdir config html logs

touch docker-compose.yml
touch config/default.conf
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

Edit config/default file:

```
nano config/default.conf
```

Paste in the complete `default.conf`, then save and exit:

- CTRL + O → Enter
- CTRL + X

---

## 📄 Test Page

Edit html/index file:

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
nginx/logs/
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


## ⚙️ Optional Commands (Quick Reference)

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

Use this to completely delete all text in the `docker-compose.yml` if you are replacing everything.
```
CTRL + K
```

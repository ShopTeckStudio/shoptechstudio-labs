# 🌐 NGINX Stack

This folder contains a Docker-based NGINX stack used as a reverse proxy and static file server within a homelab environment.

The stack is designed to be modular, lightweight, and easy to rebuild, while providing a clean foundation for routing traffic to internal services.

---

## 🔍 What This Stack Does

* Reverse proxy for internal services
* Static file hosting (HTML and assets)
* Central entry point for homelab applications
* Persistent logging for debugging and monitoring
* Scalable foundation for SSL and multi-service routing

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

* `config/` contains NGINX configuration files
* `html/` contains static web files
* `logs/` stores persistent access and error logs

---

## 🛠️ Initial Setup

Run the following on your Docker host:

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

Paste:

```

services:
  nginx:
    image: nginx:latest
    container_name: nginx
    restart: unless-stopped

    ports:
      - "80:80"
      - "443:443"

    volumes:
      - ./config:/etc/nginx/conf.d
      - ./html:/usr/share/nginx/html
      - ./logs:/var/log/nginx

    networks:
      - nginx_net

networks:
  nginx_net:
    driver: bridge
```

Save and exit:

* CTRL + O → Enter
* CTRL + X

---

## ⚙️ NGINX Configuration

Edit:

```
nano config/default.conf
```

Paste:

```
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```

---

## 📄 Test Page

Edit:

```
nano html/index.html
```

Paste:

```
<!DOCTYPE html>
<html>
<head>
    <title>NGINX</title>
</head>
<body>
    <h1>NGINX is running</h1>
</body>
</html>
```

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

* SSL with Let's Encrypt
* Reverse proxy to internal services
* Domain-based routing
* Integration with monitoring stack
* NGINX metrics and dashboards

---

## 🧱 Design Approach

* Minimal configuration
* Clear file structure
* Persistent data handling
* Easy to expand into a full reverse proxy system

---

## 🧰 Part of Homelab Stack

This stack integrates with:

* Monitoring (Prometheus + Grafana)
* DNS (Pi-hole)
* Smart Home (Home Assistant)
* Remote access (Tailscale)

---








This folder contains a Docker-based Nginx used to observe system health and container performance within a homelab environment.

The stack is designed to be modular, lightweight, and easy to rebuild. All services are defined in a single `docker-compose.yml` file, while only required runtime configuration files live in service-specific subfolders.

At a high level, this stack focuses on:
- Server resource monitoring (CPU, memory, disk, network)
- Docker container metrics
- Historical visibility and trend analysis
- A clean, reproducible setup that can be extended over time

Configuration files included here are intentionally minimal and exclude environment-specific values, secrets, and credentials.

As this lab evolves, additional dashboards, alerts, and tooling may be added to this stack.

## Local File Structure

This stack follows a consistent on-disk file structure on the host system.  
Docker Compose references files relative to the location of `docker-compose.yml`, so the local folder layout is intentional and important.

All services in this stack are defined in a single `docker-compose.yml` file, while only services that require host-side configuration files have folders alongside it. 

⚠️ Important:

The GitHub folder structure is for documentation and examples.  
Your actual Docker host should only contain files referenced by `docker-compose.yml`.

Example configuration files are provided in this repository and can be copied into place as needed.

### Expected Local Folder Structure

This stack typically lives inside a parent `docker/` directory on the host.

```
docker/monitoring/
├── docker-compose.yml
│
├── prometheus/
│   └── prometheus.yml
```
- prometheus/ contains the active Prometheus configuration
- Persistent data is stored in Docker-managed volumes
- Grafana, node-exporter, and cAdvisor require no host-side configuration files

## Initial Setup (Docker Host)
Run the following commands from your Docker stacks directory:

```
cd docker
mkdir monitoring
cd monitoring

mkdir prometheus

touch docker-compose.yml
touch prometheus/prometheus.yml
```

```
nano docker-compose.yml
```

Paste in the complete `docker-compose.yml`, then save and exit:

- Ctrl + O → Enter
- Ctrl + X

## Prometheus Configuration File

Prometheus requires a local configuration file to start.  
This file is referenced directly by `docker-compose.yml` and **must exist on the Docker host**.

Create the file inside the `prometheus/` directory:

```
nano prometheus/prometheus.yml
```

Paste in the complete `docker-compose.yml`, then save and exit:

- Ctrl + O → Enter
- Ctrl + X

```
docker compose up -d
```
Running this command will check your container status and up times.
```
docker compose ps
```
Use Grafana Dashboard 3662 to view metrics.

## ACCESSING YOUR CONTAINER
You will need to update <server-ip> with the ip of the computer you installed the container.

Prometheus Open:
```
http://<server-ip>:9090
```
Grafana Open:
```
http://<server-ip>:3000
```

Login: admin / admin
Change password when prompted
User name can be changed after loging in under profile

add connection to grafana

```
http://prometheus:9090
```
## OPTIONAL SERVICES/INFO 


```
cd docker/nginx
docker compose down
```

```
Use this to completely delte all text in the `docker-compose.yml` if you are replacing every thing.
- Ctrl + k
```


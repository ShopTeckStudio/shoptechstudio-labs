# Monitoring Stack

This folder contains a Docker-based monitoring stack used to observe system health and container performance within a homelab environment.

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
## OPTIONAL SERVICES 
To add node_exporter to the stack, first take the contain down

```
cd docker/monitoring
docker compose down
```
```
nano docker-compose.yml
```
Use thit to completely delte all text in the `docker-compose.yml` if you are replacing every thing.
- Ctrl + k

Paste in or remove hashtags from node_exporter in `docker-compose.yml`, then save and exit:

- Ctrl + O → Enter
- Ctrl + X

Next update the nano prometheus/prometheus.yml file so it will scrape the data for node_exporter
```
nano prometheus/prometheus.yml
```

Paste in the complete `docker-compose.yml`, then save and exit:

- Ctrl + O → Enter
- Ctrl + X

```
docker compose up -d
```



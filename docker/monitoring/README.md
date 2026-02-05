# Monitoring Stack

This folder contains a Docker-based monitoring stack used to observe system health and container performance within a homelab environment.

The stack is designed to be modular, lightweight, and easy to rebuild. All services are defined in a single `docker-compose.yml` file, while only required runtime configuration files live in service-specific subfolders.

At a high level, this stack focuses on:
- Server resource monitoring (CPU, memory, disk, network)
- Docker container metrics
- Historical visibility and trend analysis
- A clean, reproducible setup that can be extended over time

Configuration files included here are examples and templates only. Environment-specific values, secrets, and credentials are intentionally excluded.

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
│
└── data/
    ├── prometheus/
    └── grafana/
```
- prometheus/ contains the active Prometheus configuration
- data/ stores persistent container data
- Grafana, node-exporter, and cAdvisor require no host-side configuration files

## Initial Setup (Docker Host)
Run the following commands from your Docker stacks directory:

```
cd docker
mkdir monitoring
cd monitoring

mkdir prometheus
mkdir -p data/prometheus data/grafana

touch docker-compose.yml
touch prometheus/prometheus.yml
```

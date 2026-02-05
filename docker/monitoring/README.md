# Monitoring Stack

This folder contains a Docker-based monitoring stack used to observe system health and container performance within a homelab environment.

The stack is designed to be modular, lightweight, and easy to rebuild. All services are defined in a single `docker-compose.yml` file, while individual service configuration files live in their respective subfolders.

At a high level, this stack focuses on:
- Server resource monitoring (CPU, memory, disk, network)
- Docker container metrics
- Historical visibility and trend analysis
- A clean, reproducible setup that can be extended over time

Configuration files included here are examples and templates only. Environment-specific values, secrets, and credentials are intentionally excluded.

As this lab evolves, additional dashboards, alerts, and tooling may be added to this stack.


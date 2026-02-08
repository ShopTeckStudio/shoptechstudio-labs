# Monitoring Stack

This folder contains a a standalone pihole container used to do DNS stuff.

The container is designed to be lightweight, and easy to rebuild. All services are defined in a single `docker-compose.yml` file, while only required runtime configuration files live in service-specific subfolders.

This stack focuses on:
- edit for me

Configuration files included here are intentionally minimal and exclude environment-specific values, secrets, and credentials.


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
docker/pihole/
├── docker-compose.yml
```
- Persistent data is stored in Docker-managed volumes


## Initial Setup (Docker Host)
Run the following commands from within your terminal:

```
mkdir -p ~/docker/pihole/
```

```
cd ~/docker/pihole
```

```
nano docker-compose.yml
```

Paste in the complete `docker-compose.yml`, then save and exit:

```
- Ctrl + O → Enter
- Ctrl + X
```

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
## ACCESSING YOUR CONTAINER
You will need to update <server-ip> with the ip of the computer you installed the container.

OPen Pihole:
```
http://<server-ip>:9090
```


Login: admin / admin
Change password when prompted
User name can be changed after loging in under profile





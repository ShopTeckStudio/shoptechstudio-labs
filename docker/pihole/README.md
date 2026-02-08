# Pi-hole (Standalone)

This folder contains a standalone Pi-hole container used to provide network-wide DNS filtering and ad blocking.

The container is designed to be lightweight, easy to understand, and simple to rebuild. All services are defined in a single `docker-compose.yml` file, while only required runtime configuration files live alongside it.

This stack focuses on:
- Network-wide DNS-based ad and tracker blocking
- Low resource usage
- Simple, reproducible Docker configuration
- Easy rebuilds and migrations
- Clear separation between configuration and runtime data

Configuration files included here are intentionally minimal and exclude environment-specific values, secrets, and credentials.

## Local File Structure

This stack follows a consistent on-disk file structure on the Docker host.  
Docker Compose references files relative to the location of `docker-compose.yml`, so the folder layout is intentional and important.

All services in this stack are defined in a single `docker-compose.yml` file.  
Only services that require host-side configuration files have folders alongside it.

⚠️ **Important**

The GitHub folder structure exists for documentation and reference only.  
Your actual Docker host should contain **only** files referenced by `docker-compose.yml`.

Example configuration files provided in this repository can be copied into place as needed.

### Expected Local Folder Structure

This stack typically lives inside a parent `docker/` directory on the host system.

```
docker/pihole/
├── docker-compose.yml
├── etc-pihole/
└── etc-dnsmasq.d/
```
- Persistent Pi-hole data is stored in the mapped volumes and will survive container restarts.



## Initial Setup (Docker Host)

Run the following commands from your terminal:


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
```
docker compose up -d
```
## Accessing Pi-hole

Replace `<server-ip>` with the IP address of the Docker host.

Open the Pi-hole web interface:

```
http://<server-ip>:8080
```


Loggin: admin / admin
Change password when prompted
User name can be changed after loging in under profile





# Hytale Docker Server

A Docker container for running a Hytale dedicated game server with automatic updates and persistent data storage

## Features

- Automatic server download and installation using the official `hytale-downloader`
- Automatic update checking on container startup (optional)

## Requirements

- Docker and Docker Compose
- A valid Hytale account for authentication
- Some amounts of ram allocated (configurable via `JAVA_OPTS`)

I have only tested the image running on linux but if docker does what it promises it should work on any os that supports docker 

## Quick Start

### Using Docker Compose (Recommended)

1. Clone this repository or create a [docker-compose.yml](https://github.com/Brodino96/hytale-server/blob/main/docker-compose.yml)

2. Start the server:

```bash
docker compose up
```

3. On first run, authenticate with your Hytale account

### Using Docker Run

```bash
docker run -it \
  -p 5520:5520/udp \
  -v ./data:/data \
  -e JAVA_OPTS="-Xmx2G" \
  ghcr.io/brodino96/hytale-server:latest
```

### Building Locally

```bash
docker build -t hytale-server .
docker run -it -p 5520:5520/udp -v ./data:/data hytale-server
```

## Configuration

### Environment Variables

| Variable              | Default     | Description                                                               |
|-----------------------|-------------|---------------------------------------------------------------------------|
| `JAVA_OPTS`           | `-Xmx2G`    | JVM options (memory allocation, garbage collection, etc.)                 |
| `UPDATE_ON_STARTUP`   | `false`     | Automatically check for and install server updates on container start     |
| `PATCHLINE`           | `release`   | Server version channel (e.g., `release`)                                  |

### Ports

| Port | Protocol |
|------|----------|
| 5520 | UDP      |

### Volumes

The `/data` volume contains all persistent server data:

```
data/
├── Server/              # Server binaries (HytaleServer.jar)
├── Assets.zip           # Game assets
├── universe/            # World data
├── config.json          # Server configuration
├── auth.enc             # Encrypted authentication token
├── bans.json            # Banned players
├── whitelist.json       # Whitelisted players
├── permissions.json     # Player permissions
├── mods/                # Server modifications
└── logs/                # Server logs
```

## Server Management

### Viewing Logs

```bash
docker logs -f hytale-server
```

### Accessing Server Console

```bash
docker attach hytale-server
# Detach with Ctrl+P, Ctrl+Q (don't use Ctrl+C as it stops the server)
```

### Stopping the Server

```bash
docker compose down
# or
docker stop hytale-server
```

### Updating the Server

Set `UPDATE_ON_STARTUP=true` in your environment variables and restart the container, or manually trigger an update by restarting with:

```bash
docker compose down && docker compose up -d
```

## License

This project is provided as-is for running Hytale dedicated servers. Hytale is a trademark of Hypixel Studios.

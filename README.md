# LEVIY Workspace

[![Build Status](https://travis-ci.com/leviy/workspace.svg?token=gMER3s1tPFa8FEEFhAM4&branch=master)](https://travis-ci.com/leviy/workspace)

The Docker Compose file in this repository contains an instance of
[Traefik](https://traefik.io/) for development purposes. It allows you to access
microservices through your browser using a hostname (such as
`accounts.leviy.localhost`) and to run multiple microservices at the same time
without port binding conflicts.

## Getting started

### Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/)
- A DNS resolver that resolves any hostname ending in ".localhost" to 127.0.0.1 (see [instructions](docs/dns.md))

### Installation

```bash
git clone git@github.com:leviy/workspace.git
cd workspace
docker-compose up -d
```

The Docker Compose file contains the `restart: always` directive so Docker will
restart the container every time you boot your machine. If for some reason you
need to start another container or project that needs to bind to port 80, you
can stop the containers using:

```bash
docker-compose stop
```

### Usage

When the containers are running, the Traefik dashboard can be found at
http://localhost:8080/dashboard/. Other microservices can register themselves to
Traefik by adding the following to their `docker-compose.yml`:

```yaml
version: '3'

services:
  microservice-name:
    image: nginx
    networks:
      - workspace
    labels:
      - "traefik.frontend.rule=Host:subdomain.leviy.localhost"
      - "traefik.enable=true"

networks:
  workspace:
    external:
      name: workspace_default
```

When started, Traefik will automatically detect this service and start routing
traffic to it.
To see it in action simply open `http://subdomain.leviy.localhost` in a
browser.

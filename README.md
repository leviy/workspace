<img src="docs/logo.svg" width="200" alt="Leviy logo" align="right" />

# Leviy Workspace

[![Build Status](https://travis-ci.com/leviy/workspace.svg?token=gMER3s1tPFa8FEEFhAM4&branch=master)](https://travis-ci.com/leviy/workspace)

The Docker Compose file in this repository contains an instance of
[Traefik](https://traefik.io/) for development purposes. It allows you to access
microservices through your browser using a hostname (such as
`accounts.test.leviy.com`) and to run multiple microservices at the same time
without port binding conflicts.

## Getting started

### Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/)

### Installation

```bash
git clone git@github.com:leviy/workspace.git
cd workspace
docker compose up -d
```

The Docker Compose file contains the `restart: always` directive so Docker will
restart the container every time you boot your machine. If for some reason you
need to start another container or project that needs to bind to port 80, you
can stop the containers using:

```bash
docker compose stop
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
      - default

    labels:
      - "traefik.enable=true"
      - "traefik.docker.network=traefik-public"
      - "traefik.http.routers.microservice-name.rule=Host(`microservice-name.test.leviy.com`)"
    networks:
      traefik-public:
        aliases:
          - microservice-name.test.leviy.com

networks:
  traefik-public:
    external: true
```

Services that need to access the workspace for either reverse proxying or to access any of the services provided by it should be
put in the `traefik-public` network.

When started, Traefik will automatically detect this service and start routing
traffic to it.
To see it in action simply open `http://microservice-name.test.leviy.com` in a
browser.

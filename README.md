<img src="docs/logo.svg" width="200" alt="Leviy logo" align="right" />

# Leviy Workspace

[![Build Status](https://travis-ci.com/leviy/workspace.svg?token=gMER3s1tPFa8FEEFhAM4&branch=master)](https://travis-ci.com/leviy/workspace)

The Docker Compose file in this repository contains an instance of
[Traefik](https://traefik.io/) for development purposes. It allows you to access
microservices through your browser using a hostname (such as
`accounts.leviy.test`) and to run multiple microservices at the same time
without port binding conflicts.

## Getting started

### Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/)
- A DNS resolver that resolves any hostname ending in ".test" to 127.0.0.1 (see [instructions](docs/dns.md))

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
      traefik.frontend.rule: Host:subdomain.leviy.test
      traefik.enable: 'true'

networks:
  workspace:
    external:
      name: workspace_default
```

Web server services that should be reverse proxied should be linked to both the
`workspace` network and the `default` network of that Docker Compose project.
Non-web services (such as databases, etc.) should only be linked to the
`default` network.

When started, Traefik will automatically detect this service and start routing
traffic to it.
To see it in action simply open `http://subdomain.leviy.test` in a
browser.

# Configuring DNS

traefik routes traffic that matches the hostname of a Docker service's
that is configured using the appropiate labels for traefik.

We use service names like `<service>.test.leviy.com`. For this to work we've added the following A record to our leviy.com DNS.

| Type | Name              | Value     | TTL |
|------|-------------------|-----------|-----|
| A    | *.test.leviy.com  | 127.0.0.1 | 300 |

No changes are needed to developer machines.
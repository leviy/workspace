# Configuring DNS

Traefik routes traffic that matches the hostname of a Docker service's 
`traefik.frontend.rule` label. However, for applications running on your host
machine (curl, wget, the browser, etc.) to be able to know where to find the
host you need to configure your DNS to resolve all hostnames ending on
".localhost" to your local IP address.

## Linux (Ubuntu)

On Ubuntu 20.04 you have to manually add lines to the /etc/hosts file for all domains that
you want to resolve locally:

```
127.0.0.1	dashboard.leviy.test
127.0.0.1	files.leviy.test
127.0.0.1	accounts.leviy.test
```

On Ubuntu 24.04 you can use the `systemd-resolve` to resolve all domains ending on ".test" to 127.0.0.1.

(work in progress, will add additional instructions as soon as we updated to Ubuntu 24.04)

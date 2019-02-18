# Configuring DNS

Traefik routes traffic that matches the hostname of a Docker service's 
`traefik.frontend.rule` label. However, for applications running on your host
machine (curl, wget, the browser, etc.) to be able to know where to find the
host you need to configure your DNS to resolve all hostnames ending on
".localhost" to your local IP address.

## Linux (Ubuntu)

On Ubuntu 18.04 no action is required, because [systemd-resolved](https://manpages.ubuntu.com/manpages/bionic/man8/systemd-resolved.service.8.html)
already resolves hostnames ending on ".localhost" to 127.0.0.1 and ::1.

## macOS High Sierra

On macOS the same can be achieved by installing dnsmasq and configuring it to
route all traffic for ".localhost" to 127.0.0.1:

```bash
brew install dnsmasq

mkdir -pv $(brew --prefix)/etc/
echo 'address=/.localhost/127.0.0.1' >> $(brew --prefix)/etc/dnsmasq.conf
echo 'port=53' >> $(brew --prefix)/etc/dnsmasq.conf

sudo brew services start dnsmasq

sudo mkdir -v /etc/resolver
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/localhost'

ping dashboard.leviy.localhost
```

[Source](https://medium.com/@kharysharpe/automatic-local-domains-setting-up-dnsmasq-for-macos-high-sierra-using-homebrew-caf767157e43)

[Caddy Web Server](https://caddyserver.com/) with [caddy-docker-proxy](https://github.com/lucaslorentz/caddy-docker-proxy), [cloudflare](https://github.com/caddy-dns/cloudflare), and [caddy-l4](https://github.com/mholt/caddy-l4)

[![Docker Build and Publish](https://github.com/fcolasuonno/caddy-docker-proxy-cloudflare-dns/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/fcolasuonno/caddy-docker-proxy-cloudflare-dns/actions/workflows/docker-publish.yml)

**DockerHub:** [fcolasuonno/caddy-docker-proxy-cloudflare-dns](https://hub.docker.com/r/fcolasuonno/caddy-docker-proxy-cloudflare-dns)

**Docker Pull Command**

* from GitHub Container Registry: 

```
docker pull ghcr.io/fcolasuonno/caddy-docker-proxy-cloudflare-dns:latest
```
* from Docker Hub:

```
docker pull fcolasuonno/caddy-docker-proxy-cloudflare-dns:latest
```

**Example**
```yaml
services:
  caddy:
    restart: unless-stopped
    container_name: caddy
    image: fcolasuonno/caddy-docker-proxy-cloudflare-dns
    ports:
      - 443:443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ${DOCKER_DIR}/caddy/data:/data
      - ${DOCKER_DIR}/caddy/config:/config
    labels:
      caddy.email: ${CLOUDFLARE_CADDY_EMAIL}
      caddy.acme_dns: "cloudflare ${CLOUDFLARE_CADDY_API_TOKEN}"

  whoami:
    image: traefik/whoami
    labels:
      caddy: whoami.example.com
      caddy.reverse_proxy: "{{upstreams 80}}"

  ssh-server:
    labels:
      'caddy.layer4.:22.route.proxy': "{{ upstreams 22 }}"
```
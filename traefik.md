---
title: Traefik
category: Node.js
layout: 2017/sheet
prism_languages: [yaml]
updated: 2021-12-22
weight: -3
---

### Basic

```yaml
version: '2'

services:
  traefik:
    image: traefik:v2.5
    restart: unless-stopped
    command:
      - '--providers.docker.exposedByDefault=false'
      - '--entryPoints.web.address=:80'
    ports:
      - 80:80
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro

  nginx:
    image: nginx:lts-alpine
    restart: unless-stopped
    labels:
      - traefik.enable=true
      - traefik.http.routers.portal-nginx.entrypoints=web
      - traefik.http.routers.portal-nginx.rule=PathPrefix(`/`)
      - traefik.http.services.portal-nginx.loadbalancer.server.port=80
```

{: data-line="13"}

Highlighted lines are required.

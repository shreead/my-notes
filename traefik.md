# Traefik reverse proxy
## Automatic discovery docker labels
Using Ansible playbook:
```
labels:
  traefik.enable: 'true'
  traefik.http.routers.frigate.entrypoints: 'https'
  traefik.http.routers.frigate.rule: Host(`frigate.{{ MY_DOMAIN }}`)
  traefik.http.routers.frigate.tls.certresolver: 'cloudflare'
  traefik.http.services.frigate.loadbalancer.server.port: '8971'
```
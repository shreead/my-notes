# Nginx Proxy Manager reverse proxy setup

Proxy host:
- Domain Name: ha.my.lan
- Scheme: http
- IP/Port: 10.10.0.9:8123

Add this to configuration.yaml:
```
homeassistant:
  external_url: "https://ha.my.lan"
  internal_url: "http://10.10.0.9:8123"

http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.10.0.4 # the IP of my NPM container
    - 10.10.0.0/16
```    

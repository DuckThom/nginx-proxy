# nginx-proxy
Docker compose wrapper for [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) which I use primarily when developing

## Clone
`git clone https://github.com/DuckThom/nginx-proxy proxy`

## Setup
`jwilder/nginx-proxy` settings and options can be found [here](https://github.com/jwilder/nginx-proxy/blob/master/README.md)

## Config
- Proxy-wide settings should go in `conf.d/overrides.conf`, NOT in `conf.d/default.conf` as that file is auto-generated and will be overwritten when a container state changes. [More info](https://github.com/jwilder/nginx-proxy#proxy-wide)
- Vhost-specific settings should go in `vhost.d/{VIRTUAL_HOST}`. If the VIRTUAL_HOST env var of a container is `blaat.dev` then the config file should also be named `blaat.dev`. [More info](https://github.com/jwilder/nginx-proxy#per-virtual_host)

## Connect other containers
```
version: '2'

services:
    someWebServerContainer:
        image: nginx
        environment:
            VIRTUAL_HOST: "app.dev" # Required for nginx-proxy
        expose:
            - "80"
            - "443"
        networks:
            - default # Keep the container connected to it's own network
            - proxy # The network where nginx-proxy is running

networks:
    proxy:
        external:
            name: proxy_default
```

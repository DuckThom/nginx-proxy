# nginx-proxy
Docker compose wrapper for [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) which I use primarily when developing

## Clone
`git clone https://github.com/DuckThom/nginx-proxy proxy`

## Setup
`jwilder/nginx-proxy` settings and options can be found [here](https://github.com/jwilder/nginx-proxy/blob/master/README.md)

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

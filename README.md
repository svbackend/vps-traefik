# vps-traefik

### How to use

On the server you need to clone this repository:

`git clone git@github.com:svbackend/vps-traefik.git traefik`

and launch it:

`cd traefik/prod && docker compose up -d`

Once traefik up & running you can add your site gateway using docker-compose labels, for example:

```
version: "3.7"
services:
    some-project-gateway:
        restart: always
        build:
            context: gateway/docker
            dockerfile: production/nginx/Dockerfile
        expose:
            - "80"
        depends_on:
            - app-backend
            - app-frontend
        networks:
            - vps-traefik
            - default
        labels:
            - "traefik.docker.network=vps-traefik"
            - "traefik.enable=true"
            - "traefik.http.routers.some_domain.rule=Host(`some-domain.com`)"
            - "traefik.http.routers.some_domain.entrypoints=websecure"
            - "traefik.http.routers.some_domain.tls.certresolver=myresolver"
```

and in networks section of docker-compose.yml - add external network for Traefik to know about your service:

```
networks:
    default:
    vps-traefik:
        external: true
```

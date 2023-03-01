# Memo Docker

## Instaling Docker

- install docker desktop [get-docker](https://docs.docker.com/get-docker/)

If docker Desktop is stucked in Starting Mode it is probably a WLS Problem.

- Unistall Docker Desktop

and follow this https://learn.microsoft.com/fr-fr/windows/wsl/install-manual

- reboot window after WL2 installation
- install docker DeskTop
- reboot Pc 
- Launch Docker


## Get started

- [Docker get started](https://docs.docker.com/get-started)
                       
- A Docker image is a private file system just for your container. It provides all the files and code your container needs.  
*Files --build--> Image --run--> Container*

- Volumes are persisted by docker.  
They are persistent through `docker compose down`
In Windows + WSL 2 here : `\\wsl$\docker-desktop-data\data\docker\volumes`

## Docker compose

- networking  
https://docs.docker.com/compose/networking  /  https://docs.docker.com/network/bridge/  
By default Compose sets up a single network for your app. Each container for a service joins the default network and is both reachable by other containers on that network, and discoverable by them at a hostname identical to the container name.
```
networks:
  internal:
    name: ${COMPOSE_PROJECT_NAME:-silex}-internal
    driver: bridge
```

## Docker Commands

- `docker --help` `docker build --help` Help and command help  

- `docker cp CONTAINER:SRC_PATH DEST_PATH` Copy files/folders between a container and the local filesystem
- `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]` Run a command in a new container (run a container and a command in it)
- `docker run` opions : `-d` detached, in background, `-p` publish port(s) to host, `-t` terminal, `-v` bind mount a volume, `--name`  

- `docker image ls` List images  
- `docker volume ls` List volumes
- `docker ps` List running containers
- `docker ps -a` List all containers

- `docker exec [OPTIONS] CONTAINER COMMAND [ARG...]` runs a command on a running container (ex: `docker exec -ti CONTAINER sh`)
- `docker compose exec CONTAINER COMMAND` runs a command on a running container (ex: `docker compose exec CONTAINER sh`)

- `docker compose --env-file .env up -d` 
- `docker compose restart` `docker compose down`

## Docker Misc

- `docker start portainer` start portainer image

- Run portainer initial : (untested)
```
docker run -d -p 8000:8000 -p 9443:9443 -p 9000:9000 --name portainer \                                                                                    
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest
```

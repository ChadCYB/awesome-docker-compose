## Compose sample application
### docker registry application

Project structure:
```
.
├── docker-compose.yaml
└── registry-data
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  registry:
    image: registry:2
    ports:
      - "5000:5000"
  ...
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - "5080:80"
  ...
```
The compose file defines an application with two services `registry` and `ui`.

As specified in the file when deploying the application, docker-compose maps:
- port `5000` of the registry container to port `5000` of the host
- port `5080` of the ui container to port `80` of the host

Make sure port `5000, 5080` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
Before deploy it, don't forget to **change your REGISTRY_TITLE**.

```
$ docker-compose up -d

Creating network "docker-registry_registry-ui-net" with the default driver
Creating docker-registry_registry_1 ... done
Creating docker-registry_ui_1       ... done
...
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                             COMMAND                  CREATED             STATUS              PORTS                    NAMES
fa4b2e7e7d21   joxit/docker-registry-ui:static   "/docker-entrypoint.…"   Up About a minute   Up About a minute   0.0.0.0:5080->80/tcp     docker-registry_ui_1
944b09ecab60   registry:2                        "/entrypoint.sh /etc…"   Up About a minute   Up About a minute   0.0.0.0:5005->5000/tcp   docker-registry_registry_1
```

After the application starts, navigate to `http://localhost:8080` in your web browser to access.

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```

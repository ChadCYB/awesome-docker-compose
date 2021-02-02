## Compose sample application
### portainer application

Project structure:
```
.
├── docker-compose.yaml
└── portainer_data
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  portainer:
    image: portainer/portainer-ce
    ports:
      - "9000:9000"
      - "8000:8000"
    ...
```
The compose file defines an application with one service `portainer`.
- portainer: A lightweight Docker environments management UI.

As specified in the file when deploying the application, docker-compose maps:
- port `9000` of the web service container to port `9000` of the host 
- port `8000` of the edge agent communicates service container to port `8000` of the host

Make sure port `8000, 9000` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
Before deploy it, don't forget to **config your mysecrettoken**.

```
$ docker-compose up -d

Creating network "portainer_default" with the default driver
Creating portainer_registry_1 ... done
...
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                    COMMAND        CREATED              STATUS              PORTS                                            NAMES
f3e239311529   portainer/portainer-ce   "/portainer"   About a minute ago   Up About a minute   0.0.0.0:9000->9000/tcp, 0.0.0.0:9001->8000/tcp   portainer_registry_1
```

After the application starts, navigate to `http://localhost:9000` in your web browser to access portainer.

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```

## More Document
- [portainer/readthedocs](https://portainer.readthedocs.io/en/stable/deployment.html#)
- [edge agent guide](https://documentation.portainer.io/v2.0/endpoints/edge/)
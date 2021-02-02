## Compose sample application
### gitea application with MySQL

Project structure:
```
.
├── docker-compose.yaml
├── gitea-mysql
└── gitea-data
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  gitea_server:
    image: gitea/gitea:1.12.4
    ports:
       - "3000:3000"
       - "3022:22"
    ...
  gitea_db:
    image: mysql:5.7
    ...
```
The compose file defines an application with two services `gitea_server` and `gitea_db`.
- gitea_server: A self-hosted Git service.
- gitea_db: MySQL container.

As specified in the file when deploying the application, docker-compose maps:
- port `3000` of the gitea_server container to port `3000` of the host
- port `22` of the gitea_server container to port `3022` of the host

Make sure port `3000, 3022` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
Before deploy it, don't forget to **change your database password**.
```
$ docker-compose up -d

Creating network "gitea_net_gitea" with the default driver
Creating gitea_gitea_db_1 ... done
Creating gitea_gitea_server_1 ... done
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                COMMAND                  CREATED              STATUS              PORTS                                          NAMES
9fcf27c38260   gitea/gitea:1.12.4   "/usr/bin/entrypoint…"   About a minute ago   Up About a minute   0.0.0.0:3000->3000/tcp, 0.0.0.0:3022->22/tcp   gitea_gitea_server_1
9f3fbdb683b8   mysql:5.7            "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp, 33060/tcp                            gitea_gitea_db_1
```

After the application starts, navigate to `http://localhost:3000` in your web browser.

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```

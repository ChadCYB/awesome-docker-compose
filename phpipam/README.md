## Compose sample application
### phpipam application with phpipam cron and MariaDB

Project structure:
```
.
├── docker-compose.yaml
└── phpipam-db-data
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  phpipam-web:
    image: phpipam/phpipam-www:latest
    ports:
      - "80:80"
    ...
  phpipam-cron:
    image: phpipam/phpipam-cron:latest
    ...
  phpipam-mariadb:
    image: mariadb:latest
    ...
```
The compose file defines an application with three services `phpipam-web`, `phpipam-cron` and `phpipam-mariadb`.
- phpipam-www: Frontend Apache/PHP container.
- phpipam-cron: cron container for scheduled network discovery jobs.
- phpipam-mariadb: MariaDB container.

As specified in the file when deploying the application, docker-compose maps:
- port `80` of the web service container to port `8080` of the host

Make sure port `8080` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
Before deploy it, don't forget to **change your password and key**.

```
$ docker-compose up -d

Creating network "phpipam_default" with the default driver
Creating phpipam_phpipam-mariadb_1 ... done
Creating phpipam_phpipam-cron_1    ... done
Creating phpipam_phpipam-web_1     ... done
...
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                         COMMAND                  CREATED              STATUS              PORTS                  NAMES
eb218a04a180   phpipam/phpipam-cron:latest   "/sbin/tini -- /bin/…"   About a minute ago   Up About a minute   80/tcp                 phpipam_phpipam-cron_1
2b68b8c86e09   phpipam/phpipam-www:latest    "/sbin/tini -- /bin/…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp   phpipam_phpipam-web_1
5fd640980a7f   mariadb:latest                "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp               phpipam_phpipam-mariadb_1
```

After the application starts, navigate to `http://localhost:8080` in your web browser to access phpipam.

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```
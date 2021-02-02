## Compose sample application
### wordpress application with MySQL and phpmyadmin

Project structure:
```
.
├── docker-compose.yaml
├── data_db
└── data_wordpress
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  db:
    image: mysql:5.7
    ...
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    ports:
      - 8081:80
    ...
  wordpress:
    image: wordpress:latest
    ports:
      - 8080:80
    ...
```
The compose file defines an application with three services `db`, `phpmyadmin` and `wordpress`.
- db: MySQL container.
- phpmyadmin: web interface for mysql database.
- wordpress: wordpress container.

As specified in the file when deploying the application, docker-compose maps:
- port `80` of the phpmyadmin container to port `8081` of the host
- port `80` of the wordpress container to port `8080` of the host

Make sure port `8080, 8081` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
Before deploy it, don't forget to **change your database password**.

```
$ docker-compose up -d

Creating network "wordpress_wpsite" with the default driver
Creating wordpress_db_1 ... done
Creating wordpress_wordpress_1  ... done
Creating wordpress_phpmyadmin_1 ... done
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                   COMMAND                  CREATED              STATUS              PORTS                  NAMES
3b56957f3637   phpmyadmin/phpmyadmin   "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8081->80/tcp   wordpress_phpmyadmin_1
f4721924ea2d   wordpress:latest        "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp   wordpress_wordpress_1
c135e5097a25   mysql:5.7               "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp, 33060/tcp    wordpress_db_1
```

After the application starts, navigate to `http://localhost:8080` in your web browser to access wordpress, and navigate to `http://localhost:8081` in your web browser to access phpmyadmin.

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```

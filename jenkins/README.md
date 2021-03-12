## Compose sample application
### jenkins application

Project structure:
```
.
├── docker-compose.yaml
├── jenkins-docker-certs
├── jenkins-data
└── etc_docker
```

[_docker-compose.yml_](docker-compose.yml)
```
services:
  jenkins_server:
    image: jenkinsci/blueocean
    ports:
      - "8080:8080"
      - "50000:50000"
    ...
  docker:
    image: docker:dind
    ...
```
The compose file defines an application with two services `jenkins_server`, `docker`.

As specified in the file when deploying the application, docker-compose maps:
- port `8080` of the jenkins_server container to port `8080` of the host
- port `50000` of the jenkins_server container to port `50000` of the host

Port `8080` exposes the web interface and port `50000` gives you access to a remote Java (JIRA) API. Make sure port `8080, 50000` on the host is not being used by another container, otherwise the port should be changed.

## Deploy with docker-compose
```
$ docker-compose up -d

Creating network "jenkins_net_jenkins" with the default driver
Creating jenkins_docker_1 ... done
Creating jenkins_jenkins_server_1 ... done
```

## Expected result

Listing containers must show three containers running and the port mapping as below:
```
$ docker ps

CONTAINER ID   IMAGE                 COMMAND                  CREATED              STATUS              PORTS                                              NAMES
6f5c8c9ab5b3   jenkinsci/blueocean   "/sbin/tini -- /usr/…"   About a minute ago   Up About a minute   0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   jenkins_jenkins_server_1
ead0fb5e501a   docker:dind           "dockerd-entrypoint.…"   About a minute ago   Up About a minute   2375-2376/tcp                                      jenkins_docker_1
```

After the application starts, navigate to `http://localhost:8080` in your web browser.

Using `docker logs` to get jenkins initialAdminPassword.
```
$ sudo docker logs jenkins_jenkins_server_1 -f

...
Jenkins initial setup is required. An admin user has been created and a password generated.           
Please use the following password to proceed to installation:

2a604bb7a91645b19f8c29d9eac53dac

This may also be found at: /var/jenkins_home/secrets/initialAdminPassword
...
```

Stop and remove the containers
```
$ docker-compose down
```
or add the `-v` flag to remove all volumes too
```
$ docker-compose down -v
```
## Troubleshooting
If you get the permission error like this:
```
jenkins_server_1  | touch: cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied                                                                
jenkins_server_1  | Can not write to /var/jenkins_home/copy_reference_file.log. Wrong volume permissions? 
```
Change the folder owner to current user.
```
$ sudo chown -R $USER:$USER jenkins-data/
$ sudo chown -R $USER:$USER jenkins-docker-certs/
```

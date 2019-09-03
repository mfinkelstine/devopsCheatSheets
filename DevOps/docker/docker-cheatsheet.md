# docker


## Removing Docker from Linux

```bash
dpkg -l | grep -i docker
apt-get purge -y docker-ce docker-ce-cli
apt-get autoremove -y --purge docker-ce docker-ce-cli
apt-get autoclean
rm -rf /var/lib/docker
```
## Lifecycle
- **docker create** creates a container but does not start it.
- **docker rename** allows the container to be renamed.
- **docker run** creates and starts a container in one operation.
- **docker rm** deletes a container.
- **docker update** updates a container's resource limits.

## Deleting Containers
- Kill running containers
  ```bash
  docker kill $(docker ps -q)
  ```
- Delete all containers (force!! running or stopped containers)
  ```bash
  docker rm -f $(docker ps -qa)
  ```
- Delete old containers
  ```bash 
  docker ps -a | grep 'weeks ago' | awk '{print $1}' | xargs docker rm
  ```
- Delete stopped containers
  ```bash 
  docker rm -v $(docker ps -a -q -f status=exited)
  ```
- Delete containers after stopping
  ```bash 
  docker stop $(docker ps -aq) && docker rm -v $(docker ps -aq)
  ```
- Delete dangling images
  ```bash 
  docker rmi $(docker images -q -f dangling=true)
  ```
- Delete all images
  ```bash 
  docker rmi $(docker images -q)
  ```
- Delete dangling volumes As of Docker 1.9:
  ```bash 
  docker volume rm $(docker volume ls -q -f dangling=true)
  ```
  In 1.9.0, the filter dangling=false does not work - it is ignored and will list all volumes.

- Show image dependencies
  ```bash 
  docker images -viz | dot -Tpng -o docker.png
  ```
- Slimming down Docker containers
    - Cleaning APT in a RUN layer
      This should be done in the same layer as other apt commands. Otherwise, the previous layers still persist the original information and your images will still be fat.
      ```bash
      RUN {apt commands} \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
      ```
    - Flatten an image
      ```bash
       ID=$(docker run -d image-name /bin/bash)
      docker export $ID | docker import – flat-image-name
      ```
    - For backup
      ```bash
      ID=$(docker run -d image-name /bin/bash)
      (docker export $ID | gzip -c > image.tgz)
      gzip -dc image.tgz | docker import - flat-image-name
      ```

## Monitor system resource utilization for running containers
To check the CPU, memory, and network I/O usage of a single container, you can use:

```bash
docker stats <container>
```
For all containers listed by id:
```bash
docker stats $(docker ps -q)
```
For all containers listed by name:
```bash
docker stats $(docker ps --format '{{.Names}}')
```
For all containers listed by image:
```bash
docker ps -a -f ancestor=ubuntu
```
Remove all untagged images
```bash
docker rmi $(docker images | grep “^” | awk '{split($0,a," "); print a[3]}')
```
Remove container by a regular expression
```bash
docker ps -a | grep wildfly | awk '{print $1}' | xargs docker rm -f
```
Remove all exited containers
```bash
docker rm -f $(docker ps -a | grep Exit | awk '{ print $1 }')
```
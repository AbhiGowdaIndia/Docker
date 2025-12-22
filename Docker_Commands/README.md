# Commonly used Dokcer commands

### Docker basic commands

* Check Docker version

  ```cmd
  docker --version
  ```

* Check Docker system info

  ```cmd
  docker info
  ```

* Check Docker help

  ```cmd
  docker help
  docker run --help
  ```

### Docker image related commands

* Pull an image

  ```cmd
  docker pull nginx
  docker pull ubuntu:22.04
  ```

* List images

  ```cmd
  docker images
  ```

* List Images Id's only

  ```cmd
  dockwer images -q
  ```

* Remove an image

  * Normal remove

    ```cmd
    docker rmi <image-name or ID>
    ```

  * Force remove

    ```cmd
    docker -f rmi <image-name or ID>
    ```

* Build an image

  ```cmd
  docker biuld -t myapp:1.0 .
  ```

* Tag an image

  ```cmd
  docker tag myapp:1.0 myrepo/myapp:latest
  ```

* Push an image

  ```cmd
  docker login
  docker push myrepo/myapp:latest
  ```

### Container-related commands

* Run a container

  ```cmd
  docker run nginx
  ```
  
  * Common options:

    ```cmd
    docker run -d -p 8080:80 --name web nginx
    ```

    * -d → detached mode

    * -p → port mapping

    * --name → container name

* List running containers

  ```cmd
  docker ps
  ```

* List All containers

  ```cmd
  docker ps -a
  ```

* Stop a container

  ```cmd
  docker stop <container-name or ID>
  ```

* Start a stopped container

  ```cmd
  docker start <container-name or ID>
  ```

* Restart a container

  ```cmd
  docker restart <container-name or ID>
  ```

* Remove a container

  * Normal remove

    ```cmd
    docker rm <container-name or ID>
    ```

  * Force remove

    ```cmd
    docker rm -f <container-name or ID>
    ```

### Container interaction commands

* Execute command inside container

  ```cmd
  docker exec -it <container-name or ID> /bin/bash
  ```

* View container logs

  ```cmd
  docker logs web
  docker logs -f web
  ```

* Inspect container

  ```cmd
  docker inspect <container-name or ID>
  ```

* View resource usage

  ```cmd
  docker stat
  ```

### Volume & storage commands

* List volumes

  ```cmd
  docker volume ls
  ```

* Create volume

  ```cmd
  docker volume create <volume-name>
  ```

* Use volume in container

  ```cmd
  docker run -v mydata:/data nginx
  ```

* Remove volume

  ```cmd
  docker volume rm <volume-name>
  ```

### Network commands

* List networks

  ```cmd
  docker network ls
  ```

* Create network

  ```cmd
  docker network create <network-name>
  ```

* Run container in network

  ```cmd
  docker run --network mynet nginx
  ```

### Cleanup commands

* Remove unused containers

  ```cmd
  docker container prune
  ```

* Remove unused images

  ```cmd
  docker images prune
  ```

* Remove everything unused

  ```cmd
  docker system prune
  docker system prune -a
  ```

###  Some of the aaditional commands and using commands in different ways.

* Delete all the currently available images in local repo

  ```cmd
  docker images -q | xargs I{} docker rmi {}
  ```

  or

  ```cmd
  docker rmi $(docker images -q)
  ```

* To run command inside a runing container

  ```cmd
  docker rxec -it <container-name or ID> ls /home
  ```

* To create new shell termnal to work inside a container

  ```cmd
  docker exec -it <conainer-name> bash
  ```

* To lis all the process running insid a container

  ```cmd
  docker top <container-name>
  ```

* To run a container in detached mode

  ```cmd
  docker run -d <imgae-name or ID>
  ```

* To remove a container once it get stopped

  ```cmd
  docker run -it --rm --name <container-name> <image-name> /bin/bash
  ```

* To create ENV variables while creating a container

  ```cmd
  docker run -it --name <container-name> -e TEST=testenv <image-name> /bin/bash
  ```

* If we have huge number of env's, we can put all these ENV's in file and cal load that file during run command

  **.env**
  ```dockerfile
  TEST1=test1
  TEST2=test2
  TEST3=test3
  ```

  * Load this file during creation of the container

  ```cmd
  docker run -it --name <container-name> --env-file .env <image-name>
  ```

* Passing value for ARG during build time

  **Dockerfile**
  ```dockerfile
  FROM Ubuntu
  ARG Input
  RUN apt install -y $Input
  ```

  * Build Image

    ```cmd
    docker build -t <image-name> --build-arg Input=git .
    ```

* Copy file into the running container

  * docker cp \<sourec-local_path> \<container-name>:\<dest_path>
 
  Example:

    ```cmd
    docker cp ./myfile mycontainer:/home
    ```

* Copy file from running container to host machine

  * docker cp \<container-name>:\<dest_path> \<sourec-local_path>

    ```cmd
    docker cp ./myfile mycontainer:/home/filename ./myfile 
    ```

* To override command defined at ENTRYPOINT in Dockerfile

  * Initial we con't, but now after docker 1.8 version we can override the ENTRYPOINT command.

    **Dockerfile**
    ```dockerfile
    FROM Ubuntu
    CMD ["-l","-rt"]
    ENTRYPOINT ["ls"]
    ```

    * Override command mentioned at ENTRYPOINT

      ```cmd
      docker run -it --entrypoint echo --name <conatiner-name> <image-name> "hello world"
      ```

    * here the command which will executed will be **echo "hello world"**

* To change the USER specified in Dockerfile at runtime

  ```cmd
  docker run -it --name co1 --user root myimage
  ```

* To check the logs of a container in follow mode

  ```cmd
  docker logs -f <container-name or ID>
  ```

* Create a new Docker image from an existing container’s current state

  ```cmd
  docker commit <container-name or ID> <Image-name>
  ```
* To map a container port to a host port

  * The -p (publish) option is used to map a container port to a host port, so the application running inside the container can be accessed from outside (browser, API, other systems).

    **docker run -p \<host_port>:\<container_port> image**

    ```cmd
    docker run -p 8080:80 image
    ```

    * Multiple port mappings

      ```cmd
      docker run -p 8080:80 -p 8443:443 nginx
      ```


* Bind mount to container

  * Mounts a specific directory or file from the host into the container.

  ```cmd
  docker run -v /host/data:/container/data nginx
  ```

* Bind volume to a contaiener

  * A Docker-managed storage location, independent of host paths.

  * Stored under Docker directory (/var/lib/docker/volumes)

    ```cmd
    docker volume create mydata
    docker run -v mydata:/data nginx
    ```

    or

    ```cmd
    docker run --mount type=volume,source=mydata,target=/data nginx
    ```





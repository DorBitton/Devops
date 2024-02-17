# Docker Commands

## Basic Docker Commands

* Docker run command:
    * `docker run nginx` Will run the nginx application on docker host/docker hub
    * Docker Run - attach and detach modes:
        * `docker run kodekloud/simple-webapp` Runs in attached mode
        * `docker run -d kodekloud/simpe-webapp` Runs in detach mode - background
    * Docker Run - Tag:
        * `docker run redis` = `docker run redis:latest` Running the default application name without a Tag will download the latest version
        * `docker run redis:4.0` Will download redis version 4.0
    * Docker Run - STDIN
        * ` docker run -it kodekloud/simple_prompt-docker` Runs in attached and interactive mode
    * Docker Run - PORT mapping
        * `docker run -p 80:5000 name` Will run the application inside on port 5000 and on the host running the container on port 80
    * Docker Run - Volume mapping
        * `docker run -v /opt/datadir:/var/lib/mysql mysql` Mount host directory inside the docker container. /opt/datadir Is the host volume.
* Docker List containers:
    * `docker ps` 
    * `docker ps -a` All
* Docker Stop a container:
    * `docker stop name`
* Docker Remove a container:
    * `docker rm name`
* Docker List images:
    * `docker images`
* Docker Remove images:
    * `docker rmi name` ! Delete all dependent containers to remove image
* Docker Download an image:
    * `docker pull name`
* Docker execute a command:
    * `docker exec name command` Example: `docker exec docker_name cat /etc/hosts`
* Docker Inspect container:
    * `docker inspect name`
* Docker Container Logs:
    * `docker logs name`


https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/How%20to%20create%20my%20own%20image.PNG

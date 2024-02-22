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

## Environment Variables

* We can set an Environment Variable as the following in the docker image:
    * `var = os.environ.get("Var")` Example: `color = os.environ.get('APP_COLOR')`
        * We can now state the Environment Variable in the Docker Run command: `docker run -e APP_COLOR=blue simple-webapp-color`
    * Inspect Environment Variable: `docker inspect blissful_hopper`

## Docker Storage
    
* We can create a shared folder on the Host machine that will use as persistent storage inside the container:
    * `docker volume create data_volume` Will create a shared folder in: `/var/lib/docker/volumes/data_volume`
    * We can also create or use the shared folder in the run command: 
        * `docker run -v data_volume:/var/lib/mysql mysql`
    * If we provide a complete path we can also use a folder no in the docker location:
        * `docker run -v /data/mysql:/var/lib/mysql mysql` This will use the folder `/data/mysql` as the storage for the container.
    
    * The preffered way to set up storage:
        * `docker run \ --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql`


## Docker Images

* Docker Build:
    * `docker build Dockerfile -t repo/name`
* Docker push:
    * `docker push repo/name` Publish my built docker image

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/How%20to%20create%20my%20own%20image.PNG)

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/DockerFile.PNG)

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/LayeredArchitecture.PNG)

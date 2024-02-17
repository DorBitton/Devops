# Docker Commands

## Basic Docker Commands

* Docker run command:
    * `docker run nginx` Will run the nginx application on docker host/docker hub. 
    * Docker Run - attach and detach modes:
        * `docker run kodekloud/simple-webapp` Runs in attached mode.
        * `docker run -d kodekloud/simpe-webapp` Runs in detach mode - background.
* Docker list command: 
    * `docker ps` 
    * `docker ps -a` All
* Docker stop a container
    * `docker stop name`
* Docker remove a container
    * `docker rm name`
* Docker list images
    * `docker images`
* Docker Remove images
    * `docker rmi name` ! Delete all dependent containers to remove image
* Docker Download an image
    * `docker pull name`

* Docker execute a command
    * `docker exec name command` Example: `docker exec docker_name cat /etc/hosts`


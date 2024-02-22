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

## Docker Networking

* Containers have networking enabled by default, and they can make outgoing connections. A container has no information about what kind of network it's attached to, or whether their peers are also Docker workloads or not. A container only sees a network interface with an IP address, a gateway, a routing table, DNS services, and other networking details. That is, unless the container uses the none network driver.

* You can create custom, user-defined networks, and connect multiple containers to the same network. Once connected to a user-defined network, containers can communicate with each other using container IP addresses or container names.

* The following example creates a network using the bridge network driver and running a container in the created network:
    * `docker network create -d bridge my-net`
    * `docker run --network=my-net -itd --name=container3 busybox`

* Drivers
    * The following network drivers are available by default, and provide core networking functionality:

| Driver    | Description                                                              |
| --------  | --------                                                                 |
| bridge	| The default network driver.                                              |
| host	    | Remove network isolation between the container and the Docker host.      |
| none  	| Completely isolate a container from the host and other containers.       |
| overlay   | Overlay networks connect multiple Docker daemons together.               |
| ipvlan	| IPvlan networks provide full control over both IPv4 and IPv6 addressing. |
| macvlan	| Assign a MAC address to a container.                                     |

* The following example runs a Redis container, with Redis binding to localhost, then running the redis-cli command and connecting to the Redis server over the localhost interface.
    * `docker run -d --name redis example/redis --bind 127.0.0.1`
    * `docker run --rm -it --network container:redis example/redis-cli -h 127.0.0.1`

* By default, when you create or run a container using `docker create` or `docker run`, the container doesn't expose any of its ports to the outside world. Use the `--publish` or `-p` flag to make a port available to services outside of Docker. This creates a firewall rule in the host, mapping a container port to a port on the Docker host to the outside world. Here are some examples:

| Flag value    |	Description                                                            |
| --------      | --------                                                                 |
| -p 8080:80	| Map port 8080 on the Docker host to TCP port 80 in the container. |
| -p 192.168.1.100:8080:80	| Map port 8080 on the Docker host IP 192.168.1.100 to TCP port 80 in the container. |
| -p 8080:80/udp	| Map port 8080 on the Docker host to UDP port 80 in the container. |
| -p 8080:80/tcp -p 8080:80/udp	| Map TCP port 8080 on the Docker host to TCP port 80 in the container, and map UDP port 8080 on the Docker host to UDP port 80 in the container. |


## Docker Images

* Docker Build:
    * `docker build Dockerfile -t repo/name`
* Docker push:
    * `docker push repo/name` Publish my built docker image

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/How%20to%20create%20my%20own%20image.PNG)

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/DockerFile.PNG)

![alt text](https://github.com/DorBitton/certified-kubernetes-administrator/blob/main/Docker%20for%20Beginners/Images/LayeredArchitecture.PNG)

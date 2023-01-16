# Docker

# Containers

Instead of partitioning physical resources (via hypervisor) into virtual machines (virtualization), ___containers___ create virtual operating systems to run isolated (containerized) applications

## Benefits of Containers

- Applications are isolated from other applications
- Portability - containers are platform agnostic, can be run on any combination of OS and hardware


## Commands
All commands are prefixed with the `docker` command

|Command|Description|Format|Example|
|:-:|:-|:-:|:-|
|images|Dsplay a detailed list of docker images|`docker images`|docker images|
|info|Provides general settings/details of your docker host|`docker info`|docker info|
|ps|Display a detailed list of running containers|`docker ps`|docker ps|
|pull|Make a local copy of an image found in the registry|`docker pull <image>`|docker pull alpine
|rm|Remove a docker container|`docker rm <container-name/id>`|docker rm hello-world|
|rmi|Remove a docker image|`docker rmi <image-name/id>`|docker rmi hello-world|
|run|Create and run a new container with given image|`docker run <image>`|docker run hello-world|
|start|Start up an existing container|`docker start <container>`|docker start hello-world|
|stop|Stop a running container|`docker stop <container>`|docker stop hello-world|
|swarm|Create a swarm of container nodes|`docker swarm init`|docker swarm init|
|service|Create a service|`docker serivce create <docker-image>`|docker service create hello-world|
|version|Provides information on the Client and Host|`docker version`|docker version|



## Docker run

The `docker run` command will start a container with the given docker _image_. Docker will search the local daemon for a container matching the image. If no matching container was found in the daemon, Docker will search the _Docker Hub_ Registry and pull down the matching image and use it to create a container

```bash
#  run a container named 'web' (--name)
#+ in the background detached (-d)
#+ on port 80 (-p)
#+ from the image my-web-image
docker run -d --name web -p 80:8080 <user>/my-web-image

#  run a container named temp (--name)
#+ in interactive mode (-it)
#+ and use the bash terminal (/bin/bash) 
#+ using the latest ubuntu image
docker run -it --name temp ubuntu:latest /bin/bash
```

## Docker ps

The `docker ps` command shows a detailed list of the running containers with the following information: _container id_, _image_, _command_, _created_, _status_, _ports_, _names_

```bash
# display the list of containers - output of docker ps
~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# display list of exited containers
docker ps -a
```

## Docker images
The `docker images` command will display a list of images from the daemon with the following information: _repository_, _tag_, _image id_, _created_, _size_

```bash
# display the list of local images - output of docker images
~$ docker images
REPOSITORY          TAG       IMAGE ID       CREATED       SIZE 
docker101tutorial   latest    49cd38324a91   2 hours ago   47MB 
```

_Note:_ You can consider the repository heading as the image name


# Swarm
Native clustering for a group of docker enginers. Each docker engine is said to be running in _swarm mode_. The cluster contains both __manager__ nodes and __worker__ nodes.

## Manager Nodes
Manager nodes are highly available and maintain the integrity of the swarm. There should be an odd number (max 3-5) of manager nodes to maintain high availability. Only one manager node is considered the _leader_ and all other manager nodes proxy commands to the lead manager node. Manager nodes are considered worker nodes

## Worker Nodes
Worker nodes receive commands from the manager nodes and execute tasks

## Service
Declarative way of running and scaling tasks. A ___task___ is the unit of work assigned to the worker nodes. You can define a service to run a certain number of container instances (tasks). Docker will ensure that the service will always be running on that set number of container instances, and will reconciliate any tasks that are stopped


```bash
#  initialize a swarm and set the current node as a manager node
#+ with the advertise address (IP that will be associated with swarm activities)
#+ and listen address (IP that will listen for manager node traffic)
mgr_node1: docker swarm init --advertise-addr <IP>:<PORT> --listen-addr <IP>:<PORT>

# After swarm is created, get the command to add a worker node - this command must be run on the desired node with their own IP address
work_node1: docker swarm join-token worker

# After swarm is created, get the command to add a manager node - this command must be run on the desired node with their own IP address
mgr_node2: docker swarm join-token manager

# View active nodes (only managers can run this command)
docker node ls

# From a manager node, promote a worker node to a manager
mgr_node1: docker node promote <worker-node-ID>
```

_NOTE:_ The advertise and listen address are IP addresses found on the node. To connect to this swarm, other nodes must be able to connect to these set IP addresses


```bash
# create a service with 5 tasks on port 8080 with the given docker image
docker service create -p 8080:8080 --replicas 5 <docker-image>

# check the services
docker service ls

# check the individual containers in the service
docker service ps <service-name>

# scale the service
docker service scale <service-name>=<desired-number>

# update the service with the latest version of the app/image - update 2 containers every 10 seconds
docker service update --image <new-image> --update-parallelism 2 --update-delay 10s
```

_NOTE:_ To access the service, you could hit any one of the nodes in the swarm by using their IP address + the port that you had designated for the swarm. You can even hit a node that is not running one of the containers. Docker swarm will port-forward the request and load balance it to one of the active nodes


## Stacks and Bundles
__Stack:__ An application comprised of multiple _services_
__DAB:__ Distributed Application Bundles that deploy _stacks_



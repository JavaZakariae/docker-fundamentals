# Docker-fundamentals-path

- [Docker-fundamentals-path](#docker-fundamentals-path)
  - [Vauables resources:](#vauables-resources)
    - [Articles](#articles)
  - [What is docker ?](#what-is-docker-)
  - [Why docker ?](#why-docker-)
  - [Dockerfile?](#dockerfile)
  - [Docker commands](#docker-commands)
  - [Docker commands related to images](#docker-commands-related-to-images)
  - [Docker commands related to containers](#docker-commands-related-to-containers)
  - [CMD VS  EntryPoint](#cmd-vs-entrypoint)
  - [Environements variable](#environements-variable)
  - [Docker compose](#docker-compose)
  - [Docker volumes](#docker-volumes)
    - [Type of mounts](#type-of-mounts)
    - [Advantage of volumes](#advantage-of-volumes)
    - [Docker volumes commands](#docker-volumes-commands)
  - [Docker Networks: Concepts](#docker-networks-concepts)
  - [Docker Swarm](#docker-swarm)
    - [Feature](#feature)
    - [What is a Docker Swarm? from sumologic](#what-is-a-docker-swarm-from-sumologic)
    - [Docker swarm commands](#docker-swarm-commands)


## Vauables resources:
### Articles
- Introduction to Containers, VMs and Docker: a good article from medium, https://medium.com/free-code-camp/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b
- https://towardsdatascience.com/learn-enough-docker-to-be-useful-b7ba70caeb4b


## What is docker ?
- Docker is a platform to build, run and share applications within containers. The use of containers to deploy applications is called containerisation. A container isolate the application and its dependencies from the host and from other applications.

## Why docker ?
  - To solve the Matrix from Hell.
  - In a normal way, we need to check compatibilty between libraries and insure that the app will run in differents environements, Docker solve this problem by packaging the app inside a container.
  - Inproving security, ease of developement and testing of softwares... 
  
## Dockerfile?
  - Configuration file to create Docker images. 
  - Contains a sequence of instructions [RUN, ENV, EXPOSE, VOLUME, COPY, ADD...].
  - Advantages: rebuild the image anytime, ease of use... 
  - Docker multi-stage build: using the results of many buils can help to reduce significantly the size of the built image, the [doc](https://docs.docker.com/develop/develop-images/multistage-build/) for more information.

## Docker commands

- To print the docker version: `docker --version`, `docker version`
- To get information about the docker engine: `docker info`, the printed info like running/stopped containers, images, volumes...
- Docker commands structure:
  - old: `docker <command> options`, example: `docker run it imageName`
  - new: `docker <command> <subcommand> options`, example:  `docker container run it imageName`
- To get information about specific docker command: `docker commandName --help`, `docker images --help` will display info about the `docker images` command and how to use it.
- To list docker images: `docker images` or `docker image ls`
- To list docker containers: `docker ps`, this command will list only the running containers. to list all containers, `docker ps -a`, so stopped containers will also be displayed.
- To pull an image from the default repisotory: `docker pull imageName`
- To run a container from an image: `docker run imageName`, if the image exists localy, the docker engine will create/start the container, otherwise, the image will be downloaded from the default repository and a new container will be created and started.
- To run a container from an image in an interactive mode: `docker run -it imageId`
- To run a container from an image in the background mode: `docker run -d imageId`
- To log in to the docker hub using a username and a password: `docker login`, after a succesful login, we can push images to the docker hub repositories.
- To remove an image from local repositories: `docker rmi imageName` or `docker rmi imageId`. If the image is running in a container, `docker rmi -f imageName`
- To push an image to the remote docker hub repository: a tag shoud be given to the specified image, `docker tag imageId username/imagename:versionTag`, now the image could be pushed to the remote repository by `docker push username/imagename:versionTag`
- To start and stop a container: `docker start containerId`, `docker stop containerId`
- To get stats about running containers: `docker stats` or `docker container stats` show memory usage, cpu%, IO of the running containers.
- To check disq usage talken by containers, images, volumes: `docker system df`
- To remove unused data, all stoped containers, dangling images: `docker system prune`, dangling images means image not associated with a container.
- `docker container logs containerName/id`: shows the logs of the specified container.
- `docker cp <containerId>:/file/path/within/container/filetocopy /host/path/target`: copy file from the container to the host system.
docker cp <containerId>:/file/path/within/container /host/path/target


## Docker commands related to images
- `docker history imageName/Id`: list history of an image, by which layers an image has been built. 
- `docker build .`: build an image without a name from the Dockerfile that exists in the same directory. 
- `docker build -f ./somedirectory/customDockerFile`: build a image from the specified Dockerfile.
- `docker build . -t imagename:tag`: build an image and give it a name and a tag.
- `docker rmi $(docker images -q)`: will delete all stored images.
- `docker build --no-cache -t imagename:tag`: build an image without using the cache and give it a name and a tag.


## Docker commands related to containers

- `docker ps`: list running containers. 
- `docker run --name imageName image`: run a container based on the given image, and give it the name imageName.
- `docker start containerName/id`: start a container.
- `docker stop containerName/id`: stop a container.
- `docker pause containerName/id`: pause a container.
- `docker unpause containerName/id`: unpause a container.
- `docker stats containerName/id`: print different statisctics about the container, cpu usage, memory...
- `docker top containerName/id`: print current running processes in the given container.
- `docker attach containerName/id`: connect to the given container, the container should be running, as a result we can write command inside the give container, a shell for example.
- `docker rm containerName/id`: removing a given container.
- `docker kill containerName/id`: killing a given container, `kill` will only kill the running process, the container will still be present.
- `docker exec  containerName/id ls /etc`: run the `ls /etc` command on the running containerName/id container.
- `docker run -p 8080:80 image`: run the container on the port `80` and map it to the port `8080` of the host machine.
- `docker run -e env_variable=1500 image`: run the container and pass the value 1500 as an environement variable.
- `docker inspect containerName/id`: inspect the specified container, this command will show some info like the configuratin, network, volume....
- `docker run -e env_variable=1500 --name mysqldockercontainer mysql`: run the mysql image, name it as `mysqldockercontainer`
- `docker container top containerName/id`: display the running processes on the given container.
- `docker rm $(docker ps -aq)`: will delete all containers.
- `docker export containerId > exportedContainer`: To export a container. 
- `docker exec -it containerid echo "hello"`: To run a given command on a running container. 
- To expose multiple ports: `docker run -p <host-port1:container-port1> -p <host-port2:container-port2>`

## CMD VS  EntryPoint
- Inside a Dockerfile, for example we can write `CMD ["ls","/etc"]` to run the command `ls /etc` whenever the container run. For the same purpose, we can write EntryPoint `["ls", "/etc"]`, the difference between the two is that the `CMD` command will be overriden if we pass another command as an argument when running the container, `docker run image ls /bin` for example will execute `ls /bin` in place of `ls /etc`. On the contrary Entrypoint command and parameters are not ignored when Docker container runs with new command line parameters.
- The Docker engine execute only the last CMD, the same behaviour for the EntryPoint. So we can use EntryPoint for the default command and CMD for the parameters. 
- We can override the entryPoint using the following option: `--entrypount anotherCommand` where `anotherCommand` will be executed in place of the default entrypoint found in the Dockerfile.
- CMD and Entrypoint can be written using two forms: Shell and Exec form, `CMD echo "hello"`, `CMD ["/bin/echo", "hello"]`.
- For more details about the differences and the usecase, check [here](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/), [here](https://stackoverflow.com/questions/21553353/what-is-the-difference-between-cmd-and-entrypoint-in-a-dockerfile) or [here](https://phoenixnap.com/kb/docker-cmd-vs-entrypoint).


## Environements variable
- We can pass environements varible to a running container using the following command: `docker run -e variablename=value imageId`, we can print all existing environements variables in linux using `printenv`
- We can also pass multiple environement variables using a file and running the command: `docker run --env-file variables.txt imageId`
- When using a docker compose file:

      ```
      web:
        environement:
          - variableName=value
      ```
- A good article that explains the differences between environement variables, ARG and environement files: https://vsupalov.com/docker-arg-env-variable-guide/

## Docker compose
- From the docker official documentation: 
  > Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your   application’s services. Then, with a single command, you create and start all the services from your configuration

- Step to use the docker compose tool:
  - Define the app environement with a Dockerfile.
  - Define the services that should be run in a docker-compose.yml file environement with a Dockerfile.
  - Run `docker-compose up`, so that the docker compose too runs the predefined apps.
  - The following example shows 3 images, the first image is a redis image, the second will be built from the dockerfile that exists in the mysql directory, the third is an ubuntu image and it is linked to the two other images. Keep in mind that linking the containers in  this way will be no more used, for more elaborated method, see the networking part of the documentation:
  ```
    redis:
        image: redis
    db:
        build: ./mysql
        ports:
            - 5000:8080
    ubuntu:
        image: ubuntu:latest
        links:
            - redis
            - db
  ```
- With the docker-compose file version 2, we no longer need to link the predefined images, by default docker creates a bridge network that links the containers.

  ```
    version: '2'
    services:
        redis:
            image: redis
        db:
            build: ./mysql
            ports:
                - 5000:8080
        ubuntu:
            image: ubuntu:latest
  ```
- To check the validity of a docker-compose.yml file, we use the following command:`docker-compose config`
- `docker-compose build`: build the images found in the docker-compose file. 
- `docker-compose pull ServiceName`: pull the given image.
- `docker-compose up`: build and run the images found in the docker-compose file. 
- `docker-compose up --scale imageName/id=n`: to scale the imageName/id image n times.
- `docker-compose ps`: list the containers launched with the docker-compose tool, services state.
- `docker-compose start`: start existing containers.
- `docker-compose stop`: stop running containers without removing them.
- `docker-compose rm`: remove stopped service containers.
- `docker-compose down`: stop and removes the containers, volumes and networks created with the docker-compose up command.

## Docker volumes
### Type of mounts

  ![alt text](https://docs.docker.com/storage/images/types-of-mounts-volume.png)

- Volumes are the prefered way to store data generated and used by the docker container, they are stored in a part of the host file system (var/lib/docker/volumes) managed by Docker. non docker process should not modify this part of the system.
- Binds mounts may be stored anywhere in the file system and could be modified anytime by any host system process.
- tmpfs mounts are stored only in the host system memory and are never persisted to the host file system.

### Advantage of volumes
- Decoupling container and storage.
- Share volumes between containers.
- Attach and detach volumes to and from containers.
- When deleting containers, volumes are not deleted.

### Docker volumes commands
- `docker volume ls`: list volumes.
- `docker volume create`: create a volume.
- `docker volume inspect`: inspect detailed information one or more volumes.
- `docker volume rm`: remove one or more volumes..
- `docker run -v volumename:path/inside/thecontainer/to_mount_thevolume  imageId`: run the specified image and mount the specified volume. So anything stored inside the `to_mount_thevolume` will also be available for future use using the volumename.
- `docker run -v host/somedirectory:path/inside/thecontainer/to_mount_thevolume  imageId`: run the specified image and mount the specified directory. So anything stored inside the `to_mount_thevolume` will also be available for future use using the specified directory.


## Docker Networks: Concepts
- By default:
  - Each container is connected to a private virtual network (Bridge).
  - The bridge Network: `172.17.0.0/16`
  - Each virtual network routes throught nat firewall on the host.
  - All containers on a virtal network can talks to each other.
  - Whenever a container is restarted, it can get a new Ip address.
  - `172.17.0.1/16` is associated with the host machine (docker0).
  - `docker run --name containerOne --network newNetwork imageId`: to run the container within the given network, the given network should already be created using for example: `docker network create -d bridge --subnet 172.50.0.0/16 newNetwork`
  - More options to be cheked: `--net : none`, `--net : host`, `--net : container:<containerName>`, `--add-host`, `--dns`
- To show port forwarding info: `docker container port containerId`
- To show IpAddress of a given container : `docker container inspect --format {{.NetworkSettings.IPaddress}} containerId`
- List the existing networks: `docker network ls`
- Inspect a given network: `docker inspect network`, for example the bridge network.
- Remove a given network: `docker network rm myNetwork`, to remove all unused networks `docker network prune`
- Disconnect a container from a given network: `docker network disconnect myNetwork myContainer`
- Install the ping tool inside a running ubuntu container: `apt-get update -y&& apt-get install iputils-ping -y`
- Install the ping tool using a custom Dockerfile, and when loading  the container, run the ping tool:
     ```
      From ubuntu
      RUN apt-get update -y
      RUN apt-get install iputils-ping -y
      CMD ["ping","192.168.1.1"] 
     ```
- the Run command will be executed when building the image, while the CMD command will be executed onluy when running the container.     
- To save the container into an image: `docker commit imageId  user/testimage:version3`
- To push the image to the docker registry: we give the image a tagName, for example `user/imageName`, and we push it with the command `push user/imageName`, the client should already be signed in into the docker registry (using the usernameand the password).

## Docker Swarm
### Feature
- Cluster management
- Decentralized design
- Declarative sevice model
- Scaling
- Service discovery
- Load balancing
### What is a Docker Swarm? [from sumologic](https://www.sumologic.com/glossary/docker-swarm/#:~:text=A%20Docker%20Swarm%20is%20a,join%20together%20in%20a%20cluster.&text=Docker%20swarm%20is%20a%20container,deployed%20across%20multiple%20host%20machines.)
> A Docker Swarm is a group of either physical or virtual machines that are running the Docker application and that have been configured to join together in a cluster. Once a group of machines have been clustered together, you can still run the Docker commands that you're used to, but they will now be carried out by the machines in your cluster. The activities of the cluster are controlled by a swarm manager, and machines that have joined the cluster are referred to as nodes.

> Docker swarm is a container orchestration tool, meaning that it allows the user to manage multiple containers deployed across multiple host machines.

> One of the key benefits associated with the operation of a docker swarm is the high level of availability offered for applications. In a docker swarm, there are typically several worker nodes and at least one manager node that is responsible for handling the worker nodes' resources efficiently and ensuring that the cluster operates efficiently.

### Docker swarm commands
- `doocker swaem init --advertise-addr 192.168.1.2`: to initialise the cluster, [--advertise-addr](https://boxboat.com/2016/08/17/whats-docker-swarm-advertise-addr/).  
- `docker swarm leave`: to leave a cluster.
- `docker swarm join-token manager`: will print the command to join the given swarm cluster as a manager.
- `docker swarm join-token worker`: will print the command to join the given swarm cluster as a worker.
- Example to the above two commands: `docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-1awxwuwd3z9j1z3puu7rcgdbx 172.17.0.2:2377`
- `docker node ls`: if the current node is a manager, it will print the list of the nodes in the swarm.
- `docker node ps`: list of the tasks running on nodes.
- `docker node rm nodename`: remove the given node from the cluster.
- `docker node inspect node1`: inspect the given node.
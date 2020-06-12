# Docker-fundamentals-path

## Vauables resources:
### Articles
- Introduction to Containers, VMs and Docker: a good article from medium, https://medium.com/free-code-camp/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b
- https://towardsdatascience.com/learn-enough-docker-to-be-useful-b7ba70caeb4b


## What is docker ?
- Docker is a platform to build, run and share applications within containers. The use of containers to deploy applications is called containerisation. A container isolate the application and its dependencies from the host and from other applications.

## Why docker ?
  - To solve the Matrix from Hell.
  - In a normal way, we need to check compatibilty between libraries ans insure that the app will run in differents environements, Docker solve this problem by packaging the app inside a container.
  - Inproving security, ease of developement and testing of softwares... 
  
## Image and Containers

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

## Docker commands related to images
- `docker history imageName/Id`: list history of an image, by which layers an image has been built. 
- `docker build .`: build an image without a name from the Dockerfile that exists in the same directory. 
- `docker build -f ./somedirectory/customDockerFile`: build a image from the specified Dockerfile.
- `docker build . -t imagename:tag`: build an image and give it a name and a tag.
- `docker rmi $(docker images -q)`: will delete all stored images.


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



## CMD VS  EntryPoint
- Inside a Dockerfile, for example we can write `CMD ["ls","/etc"]` to run the command `ls /etc` whenever the container run. For the same purpose, we can write EntryPoint `["ls", "/etc"]`, the difference between the two is that the `CMD` command will be overriden if we pass another command as an argument when running the container, `docker run image ls /bin` for example will execute `ls /bin` in place of `ls /etc`.

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
- `docker-compose up --scale imageName/id=n`: to scale the imageName/id image n times.
- `docker-compose ps`: list the containers launched with the docker-compose tool.
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
- Hen deleting containers, volumes are not deleted.

## Docker Netorks: Concepts
- By default:
  - Each container is connected to a private virtual network (Bridge).
  - Each virtual network routes throught nat firewall on the host.
  - All containers on a virtal network can talks to each other.
- To show port forwarding info: `docker container port containerId`
- To show IpAddress of a given container : `docker container inspect --format {{.NetworkSettings.IPaddress}} containerId`
- List the existing networks: `docker network ls`
- Inspect a given network: `docker inspect network`, for example the bridge network.
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

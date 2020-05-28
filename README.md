# docker-fundamentals-path

## vauables resources:
- Introduction to Containers, VMs and Docker: a good article from medium, https://medium.com/free-code-camp/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b

## What is docker ?
- Docker is a platform to build, run and share applications within containers. The use of containers to deploy applications is called containerisation. A container isolate the application and its dependencies from the host and from other applications.

## Docker commands
- To print the docker version: ```docker --version```
- To get information about the docker engine: ```docker info```, the printed info like running/stopped containers, images, volumes...
- To get information about specific docker command: ```docker commandName --help```, ```docker images --help``` will display info about the ```docker images``` command and how to use it.
- To list docker images: ```docker images``` or ```docker image ls```
- To list docker containers: ```docker ps```, this command will list only the running containers. to list all containers, ```docker ps -a```, so stopped containers will also be displayed.
- To pull an image from the default repisotory: ```docker pull imageName```
- To run a container from an image: ```docker run imageName```, if the image exists localy, the docker engine will create/start the container, otherwise, the image will be downloaded from the default repository and a new container will be created and started.

## Image and Containers
## Why docker ?



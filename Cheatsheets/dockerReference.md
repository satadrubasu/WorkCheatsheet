# Docker Reference commands the REDIS example

#SCENARIO 1 - Intro
## SEARCH in the docker repos
**docker search redis**

## RUN docker images
**docker run -d redis**       // run latest image
**docker run -d redis:3.2**   // run version 3.2 of image
**docker run -d --name redisHost -p <host-port>:<container-port> redis:latest ** // container port for redis 6379 by default
**docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis**

### Foreground 
 **docker run -it ubuntu bash**   // access bash shell inside of a container which was just run
 **docker run ubuntu ps**  // launches an Ubuntu container and executes the command ps
 

## STATUS commands
**docker ps**        // list all running containers
**docker inspect <friendly-name| container-id> **   // more details of the running container
**docker logs <friendly-name| container-id> **  // display messages the container has written to std error or stdout
**docker images **   // list os all images on the host

## PERSIST data external to the container as it gets destroyed 
  // Containers are designed to be stateless.Binding of volumes using options
  // option -v <host-dir>:<container-dir>
  // Docker uses $PWD as a placeholder for the current directory.
  
**docker run -d --name redisMapped -v /opt/docker/data/redis:/data redispwd**
**docker run -d --name redisMapped -v "$PWD/data"**

# SCENARIO 2 
How to create a Docker Image for running a static HTML website using Nginx. Docker Images are built based on the contents of a Dockerfile.Note Dockerfile should be at the root folder where other needful files are stored.

## create a Dockerfile and also have some index.html in the same location
Dockerfile -->
  *FROM nginx:alpine
  COPY index.html /usr/share/nginx/html/index.html
  EXPOSE 80
  CMD ["nginx", "-g", "daemon off;"]*

## BUILD Dockerfile 
**docker build -t <build-directory>**
 // The -t parameter allows you to specify a tag, commonly used as a version number

**docker build -t webserver-image:v1 .** // built image will have tag of v1
**docker images**      // list all images on the host
**docker build -t my-nginx-image:latest .**

1. RUN <command> allows you to execute any command as you would at a command prompt, for example installing different application packages or running a build command. The results of the RUN are persisted to the image so it's important not to leave any unnecessary or temporary files on the disk as these will be included in the image.

2. COPY <src> <dest> allows you to copy files from the directory containing the Dockerfile to the container's image. This is extremely useful for source code and assets that you want to be deployed inside your container

3. CMD line in a Dockerfile defines the default command to run when a container is launched. If the command requires arguments then it's recommended to use an array, for example ["cmd", "-a", "arga value", "-b", "argb-value"], which will be combined together and the command cmd -a "arga value" -b argb-value would be run.
  An alternative approach to CMD is ENTRYPOINT. While a CMD can be overridden when the container starts, a ENTRYPOINT defines a command which can have arguments passed to it when the container launches.
In this example, NGINX would be the entrypoint with -g daemon off; the default command.

### Exposing ports in Dockerfile 
EXPOSE <port>   // instruct image to expose the specific port when run in container
EXPOSE 80 443   // expose specific ports
**EXPOSE 8000-8900** // expose range of ports




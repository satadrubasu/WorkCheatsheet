# Docker Reference commands the REDIS example
 Acknowledgement to katacoda for reference of this practice !!

#### SEARCH in the docker repos
  > docker search redis

#### RUN docker images
  ```
  docker run -d redis      // run latest image
  docker run -d redis:3.2   // run version 3.2 of image
  docker run -d --name redisHost -p <host-port>:<container-port> redis:latest
  docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
 ```
#### RUN Foreground 
  ```
  docker run -it ubuntu bash   // access bash shell inside of a container which was just run
  docker run ubuntu ps  // launches an Ubuntu container and executes the command ps
  ```
#### STATUS commands
 > docker ps        // list all running containers
 > docker inspect <friendly-name| container-id>   // more details of the running container
 > docker logs <friendly-name| container-id>   // display messages the container has written to std error or stdout
 > docker images    // list os all images on the host

#### PERSIST data external to the container as it gets destroyed 
  // Containers are designed to be stateless.Binding of volumes using options
  // option -v <host-dir>:<container-dir>
  // Docker uses $PWD as a placeholder for the current directory.
  
  docker run -d --name redisMapped -v /opt/docker/data/redis:/data redispwd
  docker run -d --name redisMapped -v "$PWD/data"

### SCENARIO 
How to create a Docker Image for running a static HTML website using Nginx. Docker Images are built based on the contents of a Dockerfile.Note Dockerfile should be at the root folder where other needful files are stored.

#### CREATE a Dockerfile and also have some index.html in the same location
Dockerfile -->
  *FROM nginx:alpine
  COPY index.html /usr/share/nginx/html/index.html
  EXPOSE 80
  CMD ["nginx", "-g", "daemon off;"]*

#### BUILD Dockerfile 
  docker build -t <build-directory>
      // The -t parameter allows you to specify a tag, commonly used as a version number
  docker build -t webserver-image:v1 . // built image will have tag of v1
  docker images      // list all images on the host
  docker build -t my-nginx-image:latest .

1. __RUN__ <command> allows you to execute any command as you would at a command prompt, for example installing different application packages or running a build command. The results of the RUN are persisted to the image so it's important not to leave any unnecessary or temporary files on the disk as these will be included in the image.

2. __COPY__ <src> <dest> allows you to copy files from the directory containing the Dockerfile to the container's image. This is extremely useful for source code and assets that you want to be deployed inside your container

3. __CMD__ line in a Dockerfile defines the default command to run when a container is launched. If the command requires arguments then it's recommended to use an array, for example ["cmd", "-a", "arga value", "-b", "argb-value"], which will be combined together and the command cmd -a "arga value" -b argb-value would be run.
  An alternative approach to CMD is __ENTRYPOINT__. While a CMD can be overridden when the container starts, a ENTRYPOINT defines a command which can have arguments passed to it when the container launches.
In this example, NGINX would be the entrypoint with -g daemon off; the default command.

#### Exposing ports in Dockerfile 
  EXPOSE <port>   // instruct image to expose the specific port when run in container
  EXPOSE 80 443   // expose specific ports
  EXPOSE 8000-8900 // expose range of ports

### Scenario : how to deploy a Node.js application within a container + CACHE Concepts
General structure for a node js application is one which has a set of files :
 Makefile , app.js , bin/ , package.json , public/ , routes/ , views/
 The next stage is to install the dependencies required to run the application. For Node.js this means running NPM install.

To keep build times to a minimum, Docker caches the results of executing a line in the Dockerfile for use in a future build. If something has changed, then Docker will invalidate the current and all following lines to ensure everything is up-to-date.

With NPM we only want to re-run npm install if something within our package.json file has changed. If nothing has changed then we can use the cache version to speed up deployment. By using COPY package.json <dest> we can cause the RUN npm install command to be invalidated if the package.json file has changed. If the file has not changed, then the cache will not be invalidated, and the cached results of the npm install command will be used
 
#### Dockerfile
  *FROM node:10-alpine
   RUN mkdir -p /src/app
    WORKDIR /src/app
    COPY package.json /src/app/package.json
    RUN npm install
   COPY . /src/app
   EXPOSE 3000
   CMD [ "npm", "start" ]*

   __docker build -t my-nodejs-app .__
   __docker run -d --name runningNodejsAapp -p 3000:3000 my-nodejs-app__
   __curl http://docker:3000__
After we've installed our dependencies, we want to copy over the rest of our application's source code. Splitting the installation of the dependencies and copying out source code enables us to use the cache when required.
   If we copied our code before running npm install then it would run every time as our code would have changed. By copying just package.json we can be sure that the cache is invalidated only when our package contents have changed.
   
 #### ENVIRONMENT VARIABLE
  environment variables can be defined when we launch the container. E.g with Node.js applications, define an environment variable for NODE_ENV when running in production.Using -e option, set the name and value as 
  __-e NODE_ENV=production__
  docker run -d --name my-production-running-app __-e NODE_ENV=production__ -p 3000:3000 my-nodejs-app


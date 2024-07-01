# Docker

Docker is a tool that makes it easier to create, deploy, and run applications by using containers. 

**Containers** are like lightweight, portable virtual machines that include everything needed to run a piece of software, such as the code, runtime, libraries, and settings. This allows developers to ensure that their application works the same way regardless of where it is deployed, whether on a personal computer, a server, or in the cloud.

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F2775ef7f-d916-4397-bbd9-e8ce58eeddb6%2FScreenshot_2024-03-09_at_1.01.25_PM.png?table=block&id=22377015-a7ed-439d-9a51-717dba681af5&cache=v2)

### Why containers?

- **Consistency:** Containers package an application and its dependencies together, ensuring that it runs the same way on any environment, whether it's a developer's laptop, a test server, or a production server. 

- **Isolation:** Each container runs in its own isolated environment, which means multiple containers can run on the same machine without interfering with each other. This isolation helps in preventing conflicts and ensures better security.

The famous line "It works on my machine" highlights a common problem in software development where an application runs correctly on a developer's local machine but fails to run properly in other environments, such as on a teammate's computer, a test server, or in production. This issue often arises due to differences in software dependencies, configurations, or operating systems.

## Inside docker

![image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2Ff09be77f-3157-45a8-98db-f6109c8a0200%2FScreenshot_2024-03-09_at_2.08.40_PM.png?table=block&id=73f531cb-9909-488d-bcf1-00ef5373cb90&cache=v2)

As afull stack developer, we need to be comfortable with the following terminologies -
- Docker Engine
- Docker CLI - Command line interface
- Docker registry


**1. Docker Engine**: Docker Engine is an open-source containerization technology that allows developers to package applications into container

**2. Docker CLI**: The command line interface lets you talk to the docker engine and lets you start/stop/list containers.

`docker run -d -p 27017:27017 mongo`

**3. Docker registry**: The docker registry is how Docker makes money. It is similar to github, but it lets you push images rather than sourcecode.

Docker’s main registry - https://dockerhub.com/

Mongo image on docker registry - https://hub.docker.com/_/mongo


## Image vs Containers

**Docker Image**:
A Docker image is a lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

`**💡A good mental model for an image is Your codebase/repository on github**`
 
**Docker Container**:
A container is a running instance of an image. It encapsulates the application or service and its dependencies, running in an isolated environment.

`**💡A good mental model for a container is when you run node index.js on your machine from some source code you got from github.**`

![image](https://lh6.googleusercontent.com/H8mhf23JNy-zCPrLaNs_H4h6K1xLRHv-P0JS4_Ad86xSo7En4tLT3POuOJPrcBNXG5lWDy2Y6fdNzRrzoB9SSLxrHhwrdk-qO28__D19NzO01OkkyBdr7YzZo2K_46HidAoUpmxeW2FOF42uOtAg3Pnfe_gcWafYs7xYywgdFeRdK3kV-p7LfIY7Z9h9tg)


## Port mapping

When you run a container, you can map a port on the host machine to a port on the container. This allows you to access the application running inside the container through the host's IP address and the mapped port.

**How Port Mapping Works:**

- Container Port: The port on which the application inside the container is running.

- Host Port: The port on the host machine that you want to map to the container port.

Example:
Let's say you have a web server running inside a Docker container on port 80 (the default HTTP port). You want to access this web server from your host machine on port 8080.

`docker run -p 8080:80 my-web-server`

Explanation:
- `-p` is the flag for port mapping.
- `8080` is the port on the host machine.
- `80` is the port inside the container.
- `my-web-server` is the name of the Docker image you're using.


## Docker commands

**1.docker images**: 
Shows you all the images that you have on your machine

**2.docker ps**:
Shows you all the containers you are running on your machine

**3.docker run**:
Lets you start a container

`-p` ⇒ let’s you create a port mapping

`-d` ⇒ Let’s you run it in detatched mode

`docker run -d -p 27017:27017 mongo`

**4.docker build**: 
Lets you build an image. We will see this after we understand how to create your own Dockerfile

**5.docker push**: 
Lets you push your image to a registry

**6. Extra commands**:

`docker rmi IMAGE NAME --force` - delete the image locally

`docker kill IMAGE ID` - stop the container

`docker exec`


## Docker File

If you want to create an image from your own code, that you can push to dockerhub, you need to create a Dockerfile for your application.

A Dockerfile is a text document that contains all the commands a user could call on the command line to create an image.

**A dockerfile has 2 parts:**
- Base image
- Bunch of commands that you run on the base image (to install dependencies like Node.js)



**Common commands**:

1. **`WORKDIR`**: Sets the directory path where subsequent instructions will be executed within the Docker container.

2. **`RUN`**: Executes commands to install software packages, update libraries, or perform any setup tasks inside the Docker image. Each `RUN` command creates a new layer in the Docker image.

3. **`CMD`**: Specifies the default command to run when a container is started. There can only be one `CMD` instruction in a Dockerfile, and if multiple `CMD` instructions are provided, only the last one takes effect.

4. **`EXPOSE`**: Informs Docker that the container will listen on specified network ports at runtime. It does not actually publish the ports; it is more of a documentation tool for developers to know which ports are intended to be exposed.

5. **`ENV`**:(not preferred in Dockerfile) Sets environment variables inside the Docker container. These variables can be accessed by processes running inside the container and can be useful for configuring software.

6. **`COPY`**: Copies files or directories from the Docker host (the machine building the Docker image) into the Docker image itself. This is useful for adding application code, configuration files, or other resources needed by the application running in the container.


## Building Images

After Creating Dockerfile perform these steps:

**0. create .dockerignore** and add required files example: node_modules, npm-debug.log etc

**1.`docker build -t image_name .`**

-t option in the docker build command is used to tag the image with a name

**2.`docker images`** : see your image

**3.`docker run -p 3000:3000 image_name`** : running our image

**4.`docker run -p 3000:3000 -e DATABASE_URL="add url here" image_name`** : -e argument let’s you send in environment variables  

It's advantageous that when building Docker images, we don't need to explicitly specify sensitive information such as environment variables. Instead, we can safely introduce them at runtime when running the image. This approach enhances security by keeping sensitive details separate from the image itself, ensuring better management and protection of application configurations.

## Pushing to dockerhub

**1. Signup to dockerhub**

**2. Create a new repository**

**3. Login to docker cli**: `docker login`

**4. Tag the image**: 

`docker tag local_image_tag docker_hub_username/repository_name:tag`

**4. Push to the repository**: 

`docker push your_username/your_reponame:tagname`

## Pulling and running the image

**1.`docker pull image_name:tag`**

**2.`docker run -p 3000:3000 image_name`** 

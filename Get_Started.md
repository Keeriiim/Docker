- [Installation](#installation)
- [Commands](#commands)


# Installation
Follow these commands

[Red Hat](https://docs.docker.com/engine/install/rhel/)
[Ubuntu]([https://docs.docker.com/engine/install/rhel/](https://docs.docker.com/engine/install/ubuntu/))

# Terminology
DevOps : How fast can you solve / provision a solution for low/high load
- Provisioning: When you host/boot your OS on an environment
- Install -> Boot -> Start application
  1. Bare metal: Installing OS on your OS, take 30-60 min of installation time.
  2. Creating instances: Usinga a cloud provider to host your OS - 15-60 min
  3. Virtulization: Using an VM to launch another OS - 30-60 min configuration time
  4. Containerization: Using dockerfile to create another OS - 5-15 min
![image](https://github.com/Keeriiim/Docker/assets/117115289/5419db67-fead-4c74-a7a6-20b91db04132)

- Docker Hub: Cloud-based registry service that allows you to link to code repositories, build your images, test them, store manually pushed images, and link to Docker Cloud so you can deploy images to your hosts.
              It provides a centralized resource for container image discovery, distribution, and change management.


- Dockerfile: A text document that contains all the commands a user could call on the command line to assemble an image.
  Using docker build users can create an automated build that executes several command-line instructions in succession.
  
- Image: A read-only template used to build containers. Images are used to store and ship applications.
  For example, you might have a Ubuntu image, a Python image, or an image that includes your application and all its dependencies.

- Layer: Each Docker image is made up of a series of layers. Each layer corresponds to an instruction in the image’s Dockerfile. Each layer except the last one is read-only.
         Layers are stacked on top of each other to form a base for a container's filesystem.
  
- Container: Installed OS on docker(runtime instance of a docker image). It runs completely isolated from the host environment by default,
  only accessing host files and networks if configured to do so.



# Engine
Docker Engine: The core of Docker, it is a client-server application.
The CLI is the client part that allows users to send commands to the Docker daemon.
The docker daemon is the server amnd is called **Dockerd**.  It handles all the heavy lifting of managing Docker's various objects like containers, images, networks, and volumes.
It runs as a background process on the host machine.

The communication between the Docker CLI (client) and the Docker daemon (server) occurs through a REST API. When you execute a command like docker run or docker build, the Docker CLI formats this command into an HTTP request and sends it to the Docker daemon. 
The daemon receives this request, performs the necessary operations, and then sends back a response.
Role of HTTP/API: The API here is indeed a set of HTTP-based endpoints that the Docker daemon exposes. These endpoints receive commands, execute the required actions (like starting a new container or building an image), and manage Docker's various functionalities.


A server which is a type of long-running program called a daemon process.
A REST API which specifies interfaces that programs can use to talk to and instruct the Docker daemon.
A command line interface (CLI) client (docker).
# Commands

## Red Hat
```bash

```


```bash
rpm -q docker-ce      # package manager -query docker-ce -> docker-ce-26.1.1-1.el9.x86_64

docker ps             # Shows which OS are running

docker images         # Shows running images
```

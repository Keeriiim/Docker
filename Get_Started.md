- [Installation](#installation)
- [Terminology](#terminology)
- [Engine](#engine)
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
The docker daemon is the server and is called **Dockerd**.  It handles all the heavy lifting of managing Docker's various objects like containers, images, networks, and volumes.
It runs as a background process on the host machine. The communication between the Docker CLI (client) and the Docker daemon (server) occurs through a REST API. When you execute a command like docker run or docker build,
the Docker CLI formats this command into an HTTP request and sends it to the Docker daemon. The daemon receives this request, performs the necessary operations, and then sends back a response.
The API here is a set of HTTP-based endpoints that the Docker daemon exposes. These endpoints receive commands, execute the required actions (like starting a new container or building an image), and manage Docker's various functionalities.


A server which is a type of long-running program called a daemon process.
A REST API which specifies interfaces that programs can use to talk to and instruct the Docker daemon.
A command line interface (CLI) client (docker).
# Commands

## Red Hat
```bash

```


```bash
# Show installed version
rpm -q docker-ce                                  # package manager -query docker-ce -> docker-ce-26.1.1-1.el9.x86_64

# Basic commands
docker pull centos:7                              # Pulls centos 7 from docker registry
docker images                                     # Shows pulled images
docker run -it *ContainerOS*                      # Start & log in to the specified OS
ctrl P and ctrl Q                                 # Exits the container without stopping it

docker ps                                         # Shows which OS are running
docker ps -a                                      # Shows started/closed containers

docker attach *Name*                              # Logs in to the RUNNING container


docker run -it --name *madeUpName* *ContainerOS*  # Chooses a OWN name for a container

```
![image](https://github.com/Keeriiim/Docker/assets/117115289/d651add3-0afd-43b7-b5a7-9dea78f28795)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/6f930185-558a-4515-9fe7-168a9a4ce839)  

![image](https://github.com/Keeriiim/Docker/assets/117115289/11afbb35-e8b0-4957-8808-ae3b0d37fe1c)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/d711ecb5-35c4-4f7d-8d2c-f75228a1a64e)
  




![image](https://github.com/Keeriiim/Docker/assets/117115289/c65943a3-1da1-4c8e-ba06-e8f29a5bda88)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/c8f0ecbc-7eb4-44bf-be5f-d97e52b69f8c)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/abf6a17b-7be0-4d28-903c-7aa48d3a39fc)  



**SELINUX**
Disabling  
![image](https://github.com/Keeriiim/Docker/assets/117115289/ee154cf3-2021-4e65-963a-329d453740c9)  






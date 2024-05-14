- [Intro](#intro)
- [Running Docker](#running-docker)
- [Links](#links)
- [Installation](#installation)
- [Terminology](#terminology)
- [Engine](#engine)
- [Commands](#commands)


# Intro
- Operating Systems have two layers.
1. OS Kernel: Communicates with the hardware (CPU, Memory)
2. Application: Run on the kernel

- Virtualization: Docker & VM machine are virtualization tools.
  1. Docker: Virtualizates the applications layer. The image you pull will be a virtualization of the app layer of the chosen OS.
             It uses the kernel of the running host OS.
  2. VM : Has both the app and kernel layer. Meaning it virtualizates/boots up the complete operating system.
  ![image](https://github.com/Keeriiim/Docker/assets/117115289/1d28b3c2-5034-4502-90c1-2a088d944b70)

- More differences:
  1. Size : Docker images are smaller ( Mbs vs Gbs)
  2. Speed : Docker images are faster to boot because the os kernel is already running.
  3. Compatablity : VM of any OS can run on ANY OS Host. But not possible with docker.

## Links
[CommandCheat](https://spacelift.io/blog/docker-commands-cheat-sheet)
[]()


## Running Docker
First thing is to check if docker can run natively which refers to running Docker containers directly on the host operating system without any additional virtualization or emulation layers. 
For example, on Linux systems, Docker can run natively because it leverages the native containerization features provided by the Linux kernel, such as cgroups and namespaces. This allows Docker containers to share the host's kernel and resources directly. 

***Newer Windows && MAC***
On Windows and macOS systems, where native containerization support for Linux containers is not available, Docker Desktop provides a way to run Docker containers natively through lightweight virtualization technologies like Hyper-V (on Windows) or Hypervisor Framework (on macOS). While this isn't running Docker containers directly on the host OS kernel, it's considered native because it utilizes the host's virtualization capabilities efficiently.

***Older Windows && MAC***
Docker Toolbox was a separate solution provided by Docker before Docker for Windows was introduced. Docker Toolbox was aimed at users of older versions of Windows that lacked native support for Docker, such as Windows 7 and earlier, or Windows versions that didn't have Hyper-V support. Docker Toolbox provided a way to run Docker on these older Windows versions by utilizing Oracle VirtualBox to create a lightweight Linux virtual machine called the "Docker Machine." This VM hosted the Docker engine and provided a Linux-like environment where Docker containers could run, even though the host operating system was Windows. While Docker Toolbox enabled Docker usage on Windows platforms that didn't support Docker natively, it wasn't as seamless or integrated as Docker for Windows, which leveraged Microsoft's Hyper-V technology for better performance and integration with the Windows operating system.


***WSL***
WSL, or Windows Subsystem for Linux, is a feature introduced by Microsoft to enable native Linux compatibility on Windows 10 and later versions. With WSL 2 integration, Docker for Windows can provide faster performance and better resource utilization by running directly inside a lightweight Linux environment compared to earlier solutions like Docker Toolbox or running Docker inside a virtual machine. This integration makes it much more convenient for developers to work with Docker on Windows systems, especially when dealing with Linux-based container

# Installation
Follow these commands

[Red Hat](https://docs.docker.com/engine/install/rhel/)  
[Ubuntu]([https://docs.docker.com/engine/install/rhel/](https://docs.docker.com/engine/install/ubuntu/))  

Windows (10 & earlier):

# Terminology
DevOps : How fast can you solve / provision a solution for low/high load
- Provisioning: When you host/boot your OS on an environment
- Install -> Boot -> Start application
  1. Bare metal: Installing OS on empty hardware, take 30-60 min of installation time.
  2. Creating instances: Usinga a cloud provider to host your OS - 5-20 min
  3. Virtulization: Using an VM to launch another OS - 30-60 min configuration time
  4. Containerization: Using dockerfile to create another OS - 1 min
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

# Commands 
The difference in behavior of running containers are due to the nature of the Docker images being used.
For example will a process run after docker run -d while an OS might not.
## Pull, run , start, attach
```bash
# Show installed version
rpm -q docker-ce                                  # package manager -query docker-ce -> docker-ce-26.1.1-1.el9.x86_64

# Basic commands
docker pull centos:7                              # Pulls centos 7 from docker registry
docker images                                     # Shows pulled images
docker run -it *ImageOS*                          # Start & log in to the specified image OS
docker rename *ContainerOS* *newName*             # Renames the created container
ctrl P and ctrl Q                                 # Exits the container without stopping it

docker ps                                         # Shows which OS are running
docker ps -a                                      # Shows started/closed containers

docker start *ContainerOS*´                       # Starts the stopped containerOS
docker attach *ContainerName*                     # Logs in to the RUNNING container


docker run -it --name *madeUpName* *ContainerOS*  # Chooses a OWN name for a container

```
![image](https://github.com/Keeriiim/Docker/assets/117115289/d651add3-0afd-43b7-b5a7-9dea78f28795)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/6f930185-558a-4515-9fe7-168a9a4ce839)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/11afbb35-e8b0-4957-8808-ae3b0d37fe1c)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/d711ecb5-35c4-4f7d-8d2c-f75228a1a64e)
  




![image](https://github.com/Keeriiim/Docker/assets/117115289/c65943a3-1da1-4c8e-ba06-e8f29a5bda88)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/c8f0ecbc-7eb4-44bf-be5f-d97e52b69f8c)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/abf6a17b-7be0-4d28-903c-7aa48d3a39fc)  


## SELINUX
**SELINUX**
Disabling  
![image](https://github.com/Keeriiim/Docker/assets/117115289/ee154cf3-2021-4e65-963a-329d453740c9)  


## start, stop, prune
```bash
docker container prune              # Removes all stopped containers
docker stop *ContainerName*         # Stops the running OS
docker start *ContainerName*        # Starts the container in the background

```
![image](https://github.com/Keeriiim/Docker/assets/117115289/d27b5faf-5a10-4300-bb52-964f808028df)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/20e9439d-14b9-45d2-bb42-0cd9107a99bd)

## Ports
```bash
docker run -d -p6000:1234           # Runs the container on host port : container port
```

## Entering non OS containers
For debugging / validating your DB, application..etc you can enter the container
```bash
docker exec -it *ContainerName/ID* /bin/bash           # New linux terminal for the container
env                                                    # Prints environmental variables
```









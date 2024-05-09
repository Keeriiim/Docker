




# Project 1
Run server & client application in windows hostmachine from pycharm
Run mongoDB & mongo-express inside containers on docker desktop

```bash
docker pull mongo
docker pull mongo-express
docker network create mongo-network

# Creating MongoDB Server -p host:container
docker run -d -p 27016:27017 -e MONGO_INITDB_ROOT_USERNAME=admin  -e MONGO_INITDB_ROOT_PASSWORD=admin --name mongodb --net mongo-network mongo

docker run -d -e MONGO_INITDB_ROOT_USERNAME=admin  -e MONGO_INITDB_ROOT_PASSWORD=admin --name mongotest mongo
docker network connect mongo-network mongotest
docker run -d -p 8080:8080 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin --net mongo-network --name expresstest -e ME_CONFIG_MONGODB_SERVER=mongotest mongo-express

# Creating MongoDB express -p host:container
docker run -d -p 8080:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express


docker logs mongodb
```

## Lessons learned
Since docker's network is a private bridged network this means that your windows host-machine can access it. In this case there is no need for a new network but it's good practice.
Go to your browser and type http://localhost:8081 to access mongo-express html gui


# Project 2
Run client application in windows hostmachine from pycharm
Run mongoDB & mongo-express inside containers on docker desktop, move the server app to a linux container in the same network as mongo
```bash
docker pull mongo
docker pull mongo-express
docker network create mongo-network

# Creating MongoDB Server
docker run -d -e MONGO_INITDB_ROOT_USERNAME=admin  -e MONGO_INITDB_ROOT_PASSWORD=admin --name mongotest --net mongo-network mongo

# Creating MongoDB express
docker run -d -p 8080:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin --net mongo-network --name expresstest -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express

docker logs expresstest -f # Check to see if the connection is established

# Try to browse localhost:8080 to see if it works
docker run -itd --name server almalinux

```


## Lessons learned
MongoDB: I don't need the -p routing because i wont need any access from my host machine. The db will automatically run it's service on the default port 27017.

If i route my host port to the non default port for the specific container. This will happen and express will not be accessable.
Therefor, ALWAYS make sure to route to the DEFAULT port.
![image](https://github.com/Keeriiim/Docker/assets/117115289/2a237ff6-2c80-4ebb-ace9-b3deeb33b3c3)  

Here are a few reasons why you cannot edit the JSON output of docker inspect directly:

Read-only: The JSON output of docker inspect is read-only. It provides information about the configuration and state of Docker objects but does not allow you to modify them directly. Docker manages the state of containers, images, volumes, networks, etc., 
and modifying this information manually could lead to unexpected behavior or corruption.

Risk of Corruption: Editing the JSON output of docker inspect directly could lead to corruption or misconfiguration of Docker objects. Docker relies on consistent and well-defined configurations to manage containers and other objects. 
Modifying these configurations manually could result in inconsistencies or conflicts with Docker's internal state.

Immutable State: Docker objects, including containers, have an immutable state. Once a container is created, its configuration and state cannot be changed directly through the docker inspect command. 

SOLUTION: ***If you need to make changes to a container's configuration, you would typically do so by recreating the container with the desired configuration.***

## Recreate
```bash

```


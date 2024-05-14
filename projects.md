




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



# Project 3 
Workaround when this happens from the host server that will run docker
![image](https://github.com/Keeriiim/Docker/assets/117115289/432b2d7c-1881-4cc8-9816-f226e3595676)  

```bash
yum install httpd -y                     # Installing httpd
yum install which -y                     # Enables to show the path
httpd path                               # Will give us /usr/sbin/httpd which is the executable meaning same as running systemctl start httpd
/usr/sbin/httpd                          # Starts the httpd service
cat /var/www
killall httpd
```


## Lessons learned
```bash
yum install -y httpd > /dev/null        # Waits for it to install, throwing the output
yum install -y httpd > /dev/null &      # Runs the entire install in the background, throwing the output
```
### Here are 3 containers running in order to understand port forwaring & docker network. Each creation assigns it's port on the hostmachine
![image](https://github.com/Keeriiim/Docker/assets/117115289/a68b0e93-0f9d-4c92-92d6-cb0205bde7cf)  

The assigning will ONLY take place as long as the machines are running!  
![image](https://github.com/Keeriiim/Docker/assets/117115289/f3fae0b1-b23f-4471-9642-7acf57475cd0)  

### Case 1: starting webhost without assigning any port  
This won't do anything for the localhost  
![image](https://github.com/Keeriiim/Docker/assets/117115289/92f797b1-56c1-4d1b-82ea-91468444410b)  
  
Inside the container we see that default port 80 is up and running  
![image](https://github.com/Keeriiim/Docker/assets/117115289/df2ad5d5-4e3c-48a1-b52a-b4aaa043d15b)  
  
We can access the localhost of the container from the inside, but not from outside because no port is specified.
![image](https://github.com/Keeriiim/Docker/assets/117115289/eabed2d0-ce76-4d2d-b9ff-dca717991e93)  

### Case 2: Assigning functioning host:container port 80:80  
The webserver runs by default on port 80, therefor it MUST be set to -p host:80 for it to work. The host can be any FREE port.  
![image](https://github.com/Keeriiim/Docker/assets/117115289/f234b687-f49b-42bf-92dc-4afa53cacc58)   
We are able to access it within the container and from the host machine.  
![image](https://github.com/Keeriiim/Docker/assets/117115289/44e7eba7-823e-4506-b027-25b8f739b6c3)  


### Case 3: Assigning unmatched containerport with installed service  




### Case 3: Assigning multiple host ports for the same container port 80  
![image](https://github.com/Keeriiim/Docker/assets/117115289/c340e2d2-2986-4cfb-bb4c-e2ae376292f1)  

This will work as long as the services are up and running inside the containers
![image](https://github.com/Keeriiim/Docker/assets/117115289/de5bbd50-71af-471f-a95f-cc0730c4748f)




![image](https://github.com/Keeriiim/Docker/assets/117115289/63f4d9c2-1a73-4894-85d8-78d4b610bc20)






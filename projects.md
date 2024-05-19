



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

killall httpd                            # Kills the service
netstat -tlnp                          # Show port, ip & PID info
kill PID                               # Kills the specified PID
```


## Lessons learned
```bash
yum install -y httpd > /dev/null        # Waits for it to install, throwing the output
yum install -y httpd > /dev/null &      # Runs the entire install in the background, throwing the output
```
Killing a process with PID  
![image](https://github.com/Keeriiim/Docker/assets/117115289/5fd8ccb5-e6e1-493e-9b5a-664c6286d320)  

# Project 4
Redirect incoming traffic from different port to webserver

```bash
# Runs the entire install in the background, throwing the output
docker run -it --name reverse -p 9090:500 almalinux    # Starts up a container that listens to traffic on port 500
yum install -y httpd > /dev/null &                     # Listening on default port 80
yum install -y nginx > /dev/null &                     # Listening on default port 80


cd /etc/nginx                                          # Go to nginx config file 
mv nginx.conf nginx.conf.disabled                      # disable installed config file
vim nginx                                              # create new config to listen on port 500 

### Add following to nginx ###
events {
        worker_connections 1024;
}
http {
        server {
                listen 500;
                listen [::]:500;

                location / {
                proxy_pass http://localhost:80/;
                }
        }

}
###############################

vim /root/.bashrc                                      # enable httpd & nginx on startup if container shuts down


curl public_ip_container_host:9090                     # Let's us enter from the specified hostport

### Add following to the file ###
kill /var/run/httpd
rm -f /var/run/nginx.pid

/usr/sbin/httpd
/usr/sbin/nginx
#################################



```
![image](https://github.com/Keeriiim/Docker/assets/117115289/8c888b23-30bb-4caa-808f-9a9320e61f41)

### If using AWS, enable http traffic on port 9090 for the security group of the app
![image](https://github.com/Keeriiim/Docker/assets/117115289/7f61788d-7150-4395-9ec4-b0b0974668c2)  

## Lessons learned
If httpd is down, we will get 502 bad gateway from nginx



# Project 5
Create a network of 3 containers. Two of them will host using httpd and will will be connected with a network alias, the third is a client to ping them. The ping/curl will make use of dockers DNS that will work as a loadbalancer using round robin. This can also be done with nginx & HA-proxy.  
![image](https://github.com/Keeriiim/Docker/assets/117115289/4e2b0adf-a98f-4418-87d4-745a82655f3f)  
```bash
docker network create dnswork                                                  # Creating the network
docker run --name a1 --network dnswork --network-alias webhost almalinux       # Creating container, adding to network and naming alias. 
yum install httpd -y
yum install vim -y
vim .bashrc

### Add for startup
rm -f /var/run/httpd/*         # Garbage collection
/usr/sbin/httpd                # Starting the service
###################
```


Add a python script that will print the contiainers ipv4.
```bash
cd /var/www/html
vim index.py

### Script
#!/bin/bash/env python3
import socket

if __name__ == "__main__":
  print("Content-Type: text/html")  # HTTP header indicating content type
  print()  # Blank line to indicate end of headers
  hostname = socket.gethostname()  # Get the hostname of the local machine
  ip_address = socket.gethostbyname(hostname)  # Get the IP address corresponding to the hostname
  print("Your local IP address is:", ip_address)
###########
```
  
Enable python script from the httpd config file
```bash
vim /etc/httpd/conf/httpd.conf

## Go to <Directory "/var/www/html">, add ExecCGI
Options Indexes FollowSymLinks ExecCGI

## Go to <IfModule dir_module>, add index.py
DirectoryIndex index.html index.py

## Go to <IfModule mime_module>, AddType
AddType application/x-httpd-cgi .py
```

Save this container as a image to duplicate it.
```bash
docker stop a1                    # Better practice to stop it before cloning
docker commit a1 project5         # Cloning current container a1 and naming the image project5

docker run --name a2 --network dnswork --network-alias webhost project5       # Creating container from image and adding to the same network as the first webhost
docker run --name a3Client --network dnswork alpine                           # Creating the client and adding it to the same network. Notice i dont add the alias.
```    
     
curl the network alias
```bash
curl webhost
```
![image](https://github.com/Keeriiim/Docker/assets/117115289/00ac9e97-edc4-44ac-9572-22a2d645d1f2)  




## Lessons learned
Linking only works within the network.I.e using network alias.
If i want to curl from docker host os i can only use ip adresses.  
If i still want a loadbalancer i need to set up a container with portforwarding.

```bash
yum whatprovides tool                 # whatprovides is used to get the package name of the tool you want to use
dnslookup google.com
httpd -M                              # Show all loaded modules
```
 







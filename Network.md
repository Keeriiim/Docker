- [Intro](#intro)
- [Network types](#network-type)
- [Ports](#ports)



# Bridge Network

# Network
Docker creates an isolated network where the containers are running in. If two containers are in the same network they can talk without the need of ports or localhost.
But containers outside need to connect via port.
![image](https://github.com/Keeriiim/Docker/assets/117115289/3f0cb5dd-42b4-4eea-89d1-e7be20c19c86)  

## Creating network
```bash
docker network ls                   # Prints avalible networks
docker network create  *NetworkName*


# Creating MongoDB Server
docker run -d \                           # detached -> in the background
-p 27017:27017 \                          # set port host:container
-e MONGO_INITDB_ROOT_USERNAME=*Name*  \   # user name
-e MONGO_INITDB_ROOT_PASSWORD=*pass* \    # user password
--name mongodb \                          # name of the container
--net mongo-network \                     # add it to the own created network called mongo-network
mongo                                     # start mongodb

# Creating MongoDB express
docker run -d \                              # detached -> in the background
-p 8081:8081 \                               # set port host:container
-e ME_CONFIG_MONGODB_ADMINUSERNAME=*Name* \  # user name
-e ME_CONFIG_MONGODB_ADMINPASSWORD=*Pass*  \ # pass word
--net mongo-network \                        # add it to the own created network called mongo-network 
--name mongo-express \                       #  name of the container
-e ME_CONFIG_MONGODB_SERVER=mongodb \        # Connect to the earlier created mongo server 
mongo-express                                # start mongo-express

# Shortcut
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=admin --name mongodb --net mongo-network mongo                                    

# Shortcut
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express 
```

## Inspecting the network
```bash
docker log *Network Name/ID*  #Shows the logs to see if the creation has been successfull
```

# Ports
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



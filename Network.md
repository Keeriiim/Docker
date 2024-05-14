- [Intro](#intro)
- [Network types](#network-type)



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


- [Intro](#intro)
- [Network types](#network-type)
- [Ports](#ports)
- [Unsupported Service](#unsupported-service)



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
### Here are 3 containers running in order to understand port forwaring & docker network.    
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


### Case 3: Assigning multiple host ports for the same container port 80  
![image](https://github.com/Keeriiim/Docker/assets/117115289/c340e2d2-2986-4cfb-bb4c-e2ae376292f1)  

This will work as long as the services are up and running inside the containers  
![image](https://github.com/Keeriiim/Docker/assets/117115289/de5bbd50-71af-471f-a95f-cc0730c4748f)


### Case 4: Assigning unmatched containerport with installed service  
Without reverse proxy
![image](https://github.com/Keeriiim/Docker/assets/117115289/66224ebd-4cc6-4ea4-aaee-1d2466194711)  

with a proxy - The only difference is i have something inside the container routing 
![image](https://github.com/Keeriiim/Docker/assets/117115289/7c6d42d7-ce03-4a38-8ece-7d1288d76d78)  
![image](https://github.com/Keeriiim/Docker/assets/117115289/3ae7b7a9-1732-4eb0-9ace-6869c2da5b59)  

Inside the container  
![image](https://github.com/Keeriiim/Docker/assets/117115289/503716fe-1807-4ab0-8cd6-2d73acb66426)  







# Unsupported Service
Docker does not support the service command which nowdays is systemctl, you either get this error or D-bus error.  
![image](https://github.com/Keeriiim/Docker/assets/117115289/4556f9e1-414f-4771-a895-51ba959e28e6)  

Therefor we can find the executable for the service using ***which*** and start it from there.
```bash
rpm -q httpd        # Do i have it installed (red hat package manager query)

which httpd         # Show path to executable
/usr/sbin/httpd     # Start httpd service
netstat -tlnp       # Find the PID
kill PID            # End the service process
```

# Systemctl
Another way to check what executable command a service is running  
![image](https://github.com/Keeriiim/Docker/assets/117115289/9ee5f537-5ec0-406f-ad3c-0c8ed0c5e3c9)  

```bash
systemctl status httpd                       # Get the link
vim /usr/lib/systemd/system/httpd.service    # Check executable
```



![image](https://github.com/Keeriiim/Docker/assets/117115289/258e5c34-5c0c-4984-b3f3-bc14b7c42de3)












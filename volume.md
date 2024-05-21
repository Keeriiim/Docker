- [Info](#info)
- [Managing a volume](#managing-a-volume)
- [Filesystem](#filesystem)
- [](#)

# Info
Volumes are stored outside the container's filesystem, making them more durable and easier to manage. They are particularly useful for sharing data between containers and persisting data even if a container is removed.

Storage  
Ephiremeal - Temp inside an VM  
Persistent - Cloud back up  


# Managing a volume

## Creating a volume for future use
```bash
docker volume create v1                      # Creates an empty volume for shared storage
docker run -it --name web1 -v v1 alpine       # Bind/Mount v1 to web1
```


# Filesystem

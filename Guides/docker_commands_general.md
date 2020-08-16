# docker - commands and general

## Docker 

**docker run**
```
docker run username/repository:tag                   # Run image from a registry
docker run 
```



**docker login** 
to be able to push or pull images
```
docker login            # Log in this CLI session using your Docker credentials
```
**pull images**
```
docker pull ibm/...
```

**running ace container**
```
sudo docker run --name acemqserver -p 7600:7600 -p 7800:7800 -p 7843:7843 -p 1414:1414 --env LICENSE=accept --env MQ_QMGR_NAME=QMGR --env ACE_SERVER_NAME=ACESERVER 603a1c53c330
```
**running ace container and mounting inital-config folder**
```
docker run --name aceapp -p 7600:7600 -p 7800:7800 -p 7843:7843 --env LICENSE=accept --env ACE_SERVER_NAME=ACESERVER --mount type=bind,src=/{path to repo}/sample/initial-config,dst=/home/aceuser/initial-config --env ACE_TRUSTSTORE_PASSWORD=truststorepwd --env ACE_KEYSTORE_PASSWORD=keystorepwd aceapp:latest
```

**running containers**
```
docker ps
docker ps -a
```

**starting a existing container**
```
docker container start ...container id...
```

**information about the process**
```
docker top db
```

**access within a running container**
- this is where all commands and access to file system
```
sudo docker exec -it ef4781e5e4a8 /bin/sh
sudo docker exec -it ef4781e5e4a8 runmqsc
```

**checking in a container**
```
docker commit ..container_id...
docker commit ..container_id... test_bar:1.0.0
```

### Basic Commands
**to view even more details about your Docker installation**
```
docker info
```
**list images**
```
docker images
```
**docker images ls**
```
docker images ls
```
**active containers**
```
docker container ls
docker container ls -all
docker container ls -q  //List container IDs
```
**List all running containers**
```
docker container ls
docker container ls -a  //List all containers, even those not running
```
**Stop, kill , remove of container**
```
docker container stop <hash>          //Gracefully stop the specified container
docker container kill <hash>          //Force shutdown of the specified container
docker container rm <hash>            //Remove specified container from this machine
docker container rm $(docker container ls -a -q)        # Remove all containers
```
**Remove images**
```
docker image rm <image id>            //Remove specified image from this machine
docker image rm $(docker image ls -a -q)  //Remove all images from this machine
```

## Information
docker will always use local image first

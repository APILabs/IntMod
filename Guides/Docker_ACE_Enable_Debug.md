# Docker - ACE on Docker : Enable Debug
## Pre-Requisites
1. Docker installation
2. Download/clone git repository : https://github.com/ronald360/integration.git
3. docker pull ibmcom/ace:latest

## Objective
Usage of mounts in docker and then using mounts to inject  configuration into the ace integration server container to enable debug configuration

## Steps
1. Clone or download the below repository :  https://github.com/ot4i/ace-docker.git
2. Pull the public ibmcom/ace image from docker hub : 
     ``` docker pull ibmcom/ace:latest ``` 
3. Navigate to the folder : integration/projects/build 
4. The initial-config folder has the ``` serverconf ``` dir which holds the server.conf.yaml file with the override properties.
            ``` ResourceManagers: ```
                ``` JVM: ```
                   ```vmDebugPort: 7605 ```

5. Once you have updated the files with custom configurations run the below command to start the integration server by using mounts to inject custom configuration into the ace container. Mount the initial config folder into the container on start up by running the below command.

``` docker run --name aceapp -p 7600:7600 -p 7800:7800 -p 7605:7605 --env ACE_SERVER_NAME=ACESERVER --env LICENSE=accept --mount type=bind,src=$PWD/initial-config,dst=/home/aceuser/initial-config ibmcom/ace:latest ```

> Note :  To make any changes after running, first change the config on the local system and then stop and restart the container for the changes to get updated.

6. Once the container is started observe if the debug is started : 
    ``` Listening for transport dt_socket at address: 7605 ```

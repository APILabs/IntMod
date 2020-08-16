# Docker - MQ on Docker : With Persistence
## Pre-Requisites
1. Docker installation
2. Download/clone git repository : https://github.com/ronald360/integration.git
3. docker pull ibmcom/mq:latest

## Objective
Start an MQ container and Usage of docker volumes to  persist mqm data, errors and logs. 

## Steps
1. Pull the public ibmcom/mq image from docker hub : 
     ``` docker pull ibmcom/mq:latest ``` 

2. Create a docker volume : docker volume create qm1devvol

3. Start the ibmcom/mq container with the below command : 
 ``` docker run   --env LICENSE=accept   --env MQ_QMGR_NAME=qm1   --publish 1414:1414   --publish 9443:9443   --detach   --volume qm1devvol:/mnt/mqm ibmcom/mq:latest ```

4. Access the webui :  https://localhost:9443/ibmmq/console

#### Authentication Information 
    Name : DEV.AUTHINFO
    TYPE : IDPWOS
    USERNAME :  admin
    PASSWORD : passw0rd

> Note: Default User Id and Password are used in the image , and channel authentication is set up with the above config by default. If you wish to change the password for the admin user, this can be done using the MQ_ADMIN_PASSWORD environment variable. 
    
5. Connecting the the Queue Manager using MQ Explorer:

#### Connection Properties
    Qmgr Name : qm1
    Host Name or IP : localhost
    Port Number : 1414
    Server Connection channel :  DEV.ADMIN.SVRCONN
    TYPE : IDPWOS
    Enable User identification : True
    USERID :  admin
    Prompt for Password : True
    PASSWORD : passw0rd  

6. Identify the IP address of the container :
    docker ps 
    CONTAINER ID       IMAGE               COMMAND           CREATED      
    bd69e58b0ee0   ibmcom/mq:latest    "runmqdevserver"    22 minutes ago 

7. Inspect the running container and get the IP Address

    ``` docker inspect bd69e58b0ee0 | grep IPAddress ```
    ``` Sample : "IPAddress": "172.17.0.2" ```

>  Note : This IPAddress will be used as the IPAddress in the MQ Endpoint Policy when connecting from the ACE docker container   
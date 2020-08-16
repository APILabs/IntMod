# Docker - ACE on Docker : MQ Client and Remote Default Queue Manager
## Pre-Requisites
1. Docker installation
2. Download/clone git repository : https://github.com/ronald360/integration.git
3. docker pull ibmcom/ace-mqclient:latest
4. Have a running ibmcom/mq:latest image set up using the instructions under document "Docker_MQ_withMount.md". Identify the IP address on the docker container by running ``` docker inspect | grep IPAddress```

## Objective
> Usage of ace-mq client image. 
> Enabling Remote MQ Queue Manager
> Using MQ Endpoint Policy


## Steps
1. Clone or download the below repository :  https://github.com/ronald360/integration.git
2. Pull the public ace-mqclient image from docker hub : 
     ``` docker pull ibmcom/ace-mqclient:latest ``` 

3. Navigate to the folder : integration/projects. This folder has the application projects . The application projects that will be deployed in this LAB:
    - AggregationMQ
    - AggregationMQBackend1
    - AggregationMQBackend2

4. Navigate to the folder : integration/projects/policies. The Policy project that will be deployed :
    - MQPolicy

5. Import the above projects in the ACE workspace and verify the MQ Policy settings :

        <?xml version="1.0" encoding="UTF-8"?>
        <policies>
            <policy policyType="MQEndpoint" policyName="RemoteMQ" policyTemplate="MQEndpoint">
            <connection>CLIENT</connection>
            <destinationQueueManagerName>qm1</destinationQueueManagerName>
            <queueManagerHostname>172.17.0.2</queueManagerHostname>
            <listenerPortNumber>1414</listenerPortNumber>
            <channelName>DEV.ADMIN.SVRCONN</channelName>
            <securityIdentity>mqsecure</securityIdentity>
            <useSSL>false</useSSL>
            <SSLPeerName></SSLPeerName>
            <SSLCipherSpec></SSLCipherSpec>
        </policy>
        </policies>

> Note :  Update the configuration in the MQ Policy based on the configurations for your environment. If you are using the mq container update the "queueManagerHostname". This can be obtained by running the docker inspect command of the mq container, or the hostname of any other external MQ queue manager. 

6. Navigate to the folder : integration/projects/build 
7. The initial-config folder has the ``` serverconf ``` dir which holds the server.conf.yaml file with the override properties.

        remoteDefaultQueueManager: '{MQPolicy}:RemoteMQ' 
        ResourceManagers:
          JVM:
            jvmDebugPort: 7605
        ServerCredentials:
        # Optionally define credentials for use by the Integration Server.
        # Customize the CredentialType section for each type of credential that you want to create credentials for.
        # You must define each CredentialType at most once.
        # Each CredentialName must be unique within the CredentialType.
        # Each CredentialType has a set of allowable properties which are a subset of username, password, passphrase, apiKey, clientId, clientSecret, sshIdentityFile.
        # For full details of allowed CredentialTypes and their properties, refer to the Knowledge Center.
        kafka:
            CloudES:
                username: 'token'
                password: 'ehedQytEW8F1y14c4E1RBwyDIBavvD3x7TPqOCpdcw_F'
        mq:
            mqsecure:
                username: 'admin'
                password: 'passw0rd'

> Note :  ```remoteDefaultQueueManager``` This property in the serverconf yaml file is responsible to configure the remote default queue manager. The MQ Policy project needs to be available in the run folder during start up for the server to recognize the policy project. In later iterations the server should start to recognize the policy project from the overrides folder as well. 


8. The initial-config folder has the ``` bars ``` dir which holds the bar files that will be deployed as the container starts up. The following bars files are available under the build folder
    1. Aggregation.bar :  This Bar file has the Fan In and Fan Out flows required to test the the aggregation concepts. The MQ Input and Output Nodes are configured to use the MQ Client Connection and has the MQ Endpoint Policy Configured.

    2. MQPolicy.bar :  This Bar file has the MQ Endpoint Policy which holds the MQ Client connection configurations. The Security Identity is configured to ``` mqsecure ``` .The mq secure credentials are added in the server conf yaml file under the  ```ServerCredentials``` section. 


9. If the Remote MQ policy is updated. Rebuild the Bar file for the policy project and place the bar file under  folder "integration/projects/build/initial-config/bars

10. Once you have updated the files with custom configurations run the below command to start the integration server by using mounts to inject custom configuration into the ace container. Mount the initial config folder into the container on start up by running the below command. 

*** Navigate the folder "SBSA/integration/projects/build" folder as the mount is using relative folder path($PWD). Else please update the command to use full path where the initial-config folder can be found**


``` docker run --name aceapp -p 7600:7600 -p 7800:7800 -p 7605:7605 --env ACE_SERVER_NAME=ACESERVER --env LICENSE=accept --mount type=bind,src=$PWD/initial-config,dst=/home/aceuser/initial-config ibmcom/ace-mqclient:latest ```

> Note :  To make any changes after running, first change the config on the local system and then stop and restart the container for the changes to get updated.

11. Once the container is started observe if the debug is started and the applications are deployed: 

        Listening for transport dt_socket at address: 7605
        2020-07-28 12:47:45.900500: About to 'Initialize' the deployed resource 'AggregationMQ' of type 'Application'. 
        2020-07-28 12:47:46.213340: About to 'Initialize' the deployed resource 'MQLogger' of type 'Application'. 
        2020-07-28 12:47:46.215368: About to 'Initialize' the deployed resource 'AggregationMQBackend1' of type 'Application'. 
        2020-07-28 12:47:46.216728: About to 'Initialize' the deployed resource 'AggregationMQBackend2' of type 'Application'. 
        ...
        2020-07-28 12:47:55.971208: The HTTP Listener has started listening on port '7600' for 'RestAdmin http' connections

12. Connect the integration server using the below configurations and start debug :
    Host Name : localhost
    Port : 7600

> Note :  Notice the applications and policies deployed under the integration server

13. Once debug is started put a message into the input queue : AGG.IN. 
        Sample Message : 

        <OverallInputMessage xmlns="http://www.ibm.com/Example">
            <Field1>data for backend1</Field1>
            <Field2>data for backend2</Field2>
        </OverallInputMessage>

> Note : You can use the MQ Explorer to put a message into the queue, or you can use the MQ Console "https://localhost:9443/ibmmq/console" 
        username : admin
        password : passw0rd





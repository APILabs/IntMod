# Docker - ACE on Docker : Part 2 - custom configuration

## Pre-Requisites
1. Docker installation
1. Download/clone git repository
1. ACE binary - ace-11.0.0.5.tar.gz 

## Objective
Usage of mounts in docker and then using mounts to inject custom configuration into the ace integration server container. 

## Steps
1. Clone or download the below repository :  https://github.com/ot4i/ace-docker.git
1. Pull the public ibmcom/ace image from docker hub à docker pull ibcom/ace or pre-build the ace image by following the steps of the previous section.
1. Navigate to the folder : ace-docker-masteràsample à initial-config. This folder has sub folders of different configurations. Example : webusers, serverconf. 
1. For each sub folder that is there in the “initial-config” folder, a script corresponding to that directory is run at start-up of the container. Only keep those folders where you want to add custom configuration.

> Note:  Do not keep empty folders. This will cause the scripts to fail at start-up and the integration server will not start.

* serverconf -> ace_config_serverconf.s
* webusers -> ace_config_webusers.sh

1. Once you have updated the files with custom configurations run the below command to start the integration server by using mounts to inject custom configuration into the ace container. Mount the initial config folder into the container on start up by running the below command.

``` docker run --name aceapp -p 7600:7600 -p 7800:7800 -p 7843:7843 --env LICENSE=accept --env ACE_SERVER_NAME=ACESERVER --mount type=bind,src=/{path to repo}/sample/initial-config,dst=/home/aceuser/initial-config --env ACE_TRUSTSTORE_PASSWORD=truststorepwd --env ACE_KEYSTORE_PASSWORD=keystorepwd aceapp:latest ```

1.  To make any changes after running, first change the config on the local system and then stop and restart the container for the changes to get updated.
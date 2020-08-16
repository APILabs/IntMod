# Docker - Ace on Docker : Part 1 - default image

this is when you will build your own container image from GIT

## Docker Environment : default image
### Pre-Requisites
1. Docker installation
1. Docker hub login details
1. - ACE binary - ace-11.0.0.5.tar.gz

### Object
Configure an App Connect Enterprise integration server on the docker environment. Build your own image of app connect enterprise. Connect to the integration server via the toolkit. 

### Steps
1. Clone or download the below repository :  https://github.com/ot4i/ace-docker.git
1. Download a copy of App Connect Enterprise (ie. ace-11.0.0.5.tar.gz) and place it in the deps folder. This folder is located within the git structure just downloaded
1. Folder structure
* Ace-docker-master
 * Deps -->  ace-11.0.0.5.tar.gz
 * --> ubi  --> dockerfile.acemq ( docker file to build ace + mq server)
 *               --> dockerfile.aceonly ( docker file to build ace)
 *               --> dockerfile.mqclient ( docker file to build ace + mq client)
1. Build the docker image with the below commands

Standalone Ace integration server : 
Be in the folder :  ace-docker-master: 
``` docker build -t ace-only --build-arg ACE_INSTALL=IBM_ACE_11.0.0.4_LNX_X8664_INCTKT.tar.gz --file ubi/Dockerfile.aceonly . ```

this command should be run from within ace-docker-master dir
the above command needs to include the correct tar.gz name
 
This will create a docker image with image name “ace-only” with tag “latest”. 
Confirm by running “docker images” to view the newly built image.

Run the docker image to start an ace integration server container.
``` docker run --name aceserver -p 7600:7600 -p 7800:7800 -p 7843:7843 --env LICENSE=accept --env ACE_SERVER_NAME=ACESERVER ace-only:latest ```

Confirm that the integration server container is running – “docker ps”.  If you don’t see any entry, run “docker ps -a” to see if the container started but crashed and didn’t start. If it is running , try connecting to the running integration server using the webui – http://localhost:7600 or using the toolkit.

To view the details of a running container :  Exec into the container and view the existing configuration. 
``` docker exec -it <container_id>  /bin/bash ```
Navigate to path /home/aceuser/aceserver to view the running configurations of the integration server.
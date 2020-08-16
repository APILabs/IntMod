# Docker - ACE on Docker Part 3 - Custom Build with application

## Pre-Requisites:
1. Docker installation
1. Download/clone git repository
1. Base image of app connect enterprise from docker hub or pre-built image.
1. Bar file of application. 
1. Set up the initial-config directory based on the previous steps.
 
 
## Objective 
Use a custom docker file to build an app connect enterprise image with application bar file and configuration to create an immutable copy.
 
## Steps
1. Clone or download the below repository :  https://github.com/ot4i/ace-docker.git
1. Pull the public ibmcom/ace image from docker hub à docker pull ibcom/ace or pre-build the ace image by following the steps of the previous section.
1. Navigate to the folder à ace-docker-masterà sample. 
1. Place the application bar files in the folder “bars_aceonly”
1. View the dockerfile under the sample folder – dockerfile.aceonly
 
1.     Build a custom integration server image with the application bar file by running the below command 
```
docker build -t aceapp --file Dockerfile.aceonly .
```

This command creates a new image with the image name – “aceapp” with the tag “latest”. Confirm the image created by running “docker images”
1. If the initial-config directory config is not updated before, follow the steps mentioned in the previous section.
1. Run the image to create an integration server with the pre-build application and custom configuration by running the below command 
```
docker run --name aceapp -p 7600:7600 -p 7800:7800 -p 7843:7843 --env LICENSE=accept --env ACE_SERVER_NAME=ACESERVER --mount type=bind,src=/{path to repo}/sample/initial-config,dst=/home/aceuser/initial-config --env ACE_TRUSTSTORE_PASSWORD=truststorepwd --env ACE_KEYSTORE_PASSWORD=keystorepwd aceapp:latest
```

 
Confirm that the application is running –
```
docker ps
```

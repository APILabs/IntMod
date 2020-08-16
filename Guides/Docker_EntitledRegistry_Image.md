# Docker - Full docker image from entitled registry or any other registry

### Pre-Requisites
1. Docker installation
2. Docker  login details
3. If you are pulling files from the entitlement registry, then login to IBM cloud container service and copy the entitlement key.

### Steps

1. Run the below commands to set the variables
        export ENTITLED_REGISTRY=cp.icr.io
        export ENTITLED_REGISTRY_USER=cp
        export ENTITLED_REGISTRY_KEY=<<Your entitlement Key>>

2. Once you have set these variables, you can login to the registry:

        docker login "$ENTITLED_REGISTRY" -u "$ENTITLED_REGISTRY_USER" -p "$ENTITLED_REGISTRY_KEY"

3. Once you have logged in successfully, you can pull the image from the entitlement registry.
        docker pull cp.icr.io/cp/appc/ace-server-prod:latest

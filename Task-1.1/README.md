# Task 1.1

## Useful commands
  
##### Deploy first version of an application

```bash
export IMAGE=<image_name or DockerHub repository>
export TAG=<tag or image version>
export NEW_TAG=<new image version>
export APP_NAME=<application name>
```

```bash
# Build docker image
docker build . -t "$IMAGE:$TAG"

# Push image on DockerHub
docker push "$IMAGE:$TAG"
```

```bash
# Deploy application
kubectl create deploy $APP_NAME --image "$IMAGE:$TAG"

# List all pods in ps output format with more information (such as node name)
kubectl get pods -o wide

# Get real-time monitoring of pods changing
kubectl get pods -w

# Forwarding claster port to workstation. (Make it in an another terminal)
kubectl port-forward deploy/$APP_NAME 8080
```

##### Check the result on [https://127.0.0.1:8080](http://127.0.0.1:8080/)

##### Redeploy new version of the application

```bash
# Get information about running deployment
kubectl get deploy $APP_NAME -o wide
```

```bash
export CONTAINER=<copy container name from previous command!>

# Redeploy new version
kubectl set image deploy $APP_NAME $CONTAINER="$IMAGE:$NEW_TAG"
```
##### Check the result on [https://127.0.0.1:8080](http://127.0.0.1:8080/)

# Task 5.2: Monitoring and Blue-Green vs Canary Deployment in GKE

## Blue-Green vs Canary Deployment

Create cluster and get credentials

```bash
gcloud container clusters create "demo-cluster" --zone "us-central1-c" --machine-type "e2-medium" --num-nodes 2
gcloud container clusters get-credentials "demo-cluster" --zone "us-central1-c" --project "united-electron-419120"
```

See all info about all clusters

```bash
kubectl get all -A
```

Create image in Artifact Registry

```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/${DEVSHELL_PROJECT_ID}/demo-repo/demo:v1.0.0
```

Prepare an environment and deploy the image

```bash
kubectl create namespace "demo"
kubectl config set-context --current --namespace "demo"
kubectl create deployment "demo" --image "us-central1-docker.pkg.dev/${DEVSHELL_PROJECT_ID}/demo-repo/demo:v1.0.0"
kubectl get deployment
```

Create service

```bash
kubectl expose deployment "demo" --type=LoadBalancer --port=80 --target-port=8080
kubectl get services
```

Get the IP-adress of the cluster (Load Balancer)

```bash
export EXTERNAL_IP=$(kubectl get services -o jsonpath="{..ingress[0].ip}")
echo $EXTERNAL_IP
```

Check application availability

```bash
curl $EXTERNAL_IP
```
In another terminal run online monitoring

```bash
while true; do curl $EXTERNAL_IP; sleep 0.75; done
```

Edit Dockerfile and build image version 2.0.0

```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/${DEVSHELL_PROJECT_ID}/demo-repo/demo:v2.0.0
```

Make Blue-Green Deployment (Zero DownTime Update)

```bash
# Zero DownTime Update
kubectl set image deploy "demo" demo="us-central1-docker.pkg.dev/${DEVSHELL_PROJECT_ID}/demo-repo/demo:v2.0.0"
kubectl get deployments.apps -o wide
```

```bash
kubectl rollout history deploy "demo"
```

After checking updating rollback version of the application to 1.0.0

```bash
kubectl rollout undo deploy "demo" --to-revision 1
```

Annotate cose of changes (optional)

```bash
# Deployment annotation
kubectl annotate deploy "demo" kubernetes.io/change-cause='update to version 2.0.0'
kubectl rollout history deploy "demo"
```

Create deployment version for Canary deploy

```bash
kubectl create deploy "demo-2" --image "us-central1-docker.pkg.dev/${DEVSHELL_PROJECT_ID}/demo-repo/demo:v2.0.0"
kubectl get pod --show-labels
kubectl get services -o wide
```

Marck all runing pods as "run=demo"

```bash
kubectl label pod --all run=demo
```

Edit service manifest to change the service of our application

```bash
kubectl edit svc demo
```

Change section "selector"

```
> "selector":
>   app: demo  ->  run: demo
```

Create scaling number of pods 3:1 (75% version 1.0.0 and 25% version 2.0.0)

```bash
# Canary Deployment
kubectl scale deploy demo --replicas 3
kubectl label pod --all run=demo
kubectl get po -w
kubectl get svc -o wide
```

For 100% transit to version 2.0.0 up-scale this replicas and down-scale number of pods version 1.0.0

```bash
kubectl scale deploy demo-2 --replicas 3
kubectl scale deploy demo --replicas 0
```

## Monitoring with Uptime Robot

To set up monitoring using [Uptime Robot](https://uptimerobot.com):

1. Log in to your Uptime Robot account or create a new account if you don't have one already.
0. Once logged in, navigate to the dashboard or main menu.
0. Look for the option to add a new monitor and click on it.
0. Select the "Keyword" monitor type from the available options.
0. Provide a friendly name or label for the monitor to easily identify it.
0. Enter the specific keyword or phrase that you want to monitor in the provided field.
0. Choose the appropriate settings for the monitoring interval and timeout duration.
0. Specify any additional advanced settings or alert configurations as needed.
0. Save the monitor settings to activate it.
0. Uptime Robot will now start monitoring the specified keyword on the designated website or page.

That's it! By setting up a Keyword monitor in Uptime Robot, you can receive alerts or notifications whenever the specified keyword is not found or present on the monitored webpage. This can help you ensure the availability and proper functioning of critical content or elements on your website or online service.

Result status page **[https://stats.uptimerobot.com/HnFXgHCWpD](https://stats.uptimerobot.com/HnFXgHCWpD)**

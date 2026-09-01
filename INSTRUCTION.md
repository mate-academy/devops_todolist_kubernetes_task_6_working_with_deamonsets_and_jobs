# INSTRUCTION.md

## Prerequisites

- [kind](https://kind.sigs.k8s.io/) installed
- [kubectl](https://kubernetes.io/docs/tasks/tools/) installed
- [podman](https://podman.io/) with rootless driver (for kind's container runtime)

## Deploy to Kind Cluster

### 1. Create the Kind cluster (podman rootless driver)

```bash
# Create a kind cluster using podman (rootless)
KIND_EXPERIMENTAL_PROVIDER=podman kind create cluster --name todoapp

# Verify the cluster is running
kubectl cluster-info
```

### 2. Deploy the todoapp

```bash
# Create the todoapp namespace
kubectl apply -f .infrastructure/namespace.yml

# Deploy the todoapp deployment
kubectl apply -f .infrastructure/deployment.yml

# Deploy the ClusterIP service
kubectl apply -f .infrastructure/clusterIp.yml

# Wait for the todoapp to be ready
kubectl wait --namespace todoapp --for=condition=ready pod --selector=app=todoapp --timeout=120s
```

### 3. Create the mateapp namespace and the todoapp service

```bash
kubectl create namespace mateapp

# Expose the todoapp ClusterIP as todoapp.mateapp.svc.cluster.local
# (skip this if a `todoapp` service already exists in mateapp)
kubectl -n mateapp create service externalname todoapp \
  --type=ExternalName \
  --external-name=todoapp-service.todoapp.svc.cluster.local
```

### 4. Deploy the DaemonSet

```bash
kubectl apply -f .infrastructure/daemonset.yml
```

### 5. Deploy the CronJob

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

## Validation

### Validate DaemonSet

Check the DaemonSet is running:

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -l app=todoapp-healthcheck
```

View DaemonSet logs (every 5 seconds it curls the todoapp health endpoint):

```bash
# Get the daemonset pod name
DS_POD=$(kubectl get pods -n mateapp -l app=todoapp-healthcheck -o jsonpath='{.items[0].metadata.name}')

# Follow logs
kubectl logs -n mateapp $DS_POD -f
```

Expected output: Health check responses from the todoapp, printed every 5 seconds.

### Validate CronJob

Check the CronJob is configured:

```bash
kubectl get cronjob -n mateapp
```

View CronJob run history:

```bash
kubectl get jobs -n mateapp --show-labels
```

View logs from the latest CronJob run:

```bash
# Get the latest job pod name
CJ_POD=$(kubectl get jobs -n mateapp -o jsonpath='{.items[0].metadata.name}')
kubectl get pods -n mateapp -l batch.kubernetes.io/job-name=$CJ_POD -o jsonpath='{.items[0].metadata.name}'

# Follow logs
kubectl logs -n mateapp <pod-name> -f
```

Expected output: Health check response from `/api/health`, printed every 4 minutes.

### Validate the todoapp service

```bash
# Check the todoapp is running
kubectl get pods -n todoapp -l app=todoapp

# Test the ClusterIP service via the mateapp FQDN
kubectl run -it --rm debug --image=busyboxplus:curl --restart=Never -n mateapp -- curl -s http://todoapp.mateapp.svc.cluster.local/api/health
```

> **Note:** The `busyboxplus:curl` image is ARM64-only. For AMD64 clusters, replace it with `alpine:latest` which has equivalent `sh` and `curl`.

### Full status overview

```bash
# All resources in mateapp
kubectl get all -n mateapp

# All resources in todoapp
kubectl get all -n todoapp
```

## Cleanup

```bash
# Delete the kind cluster
kind delete cluster --name todoapp
```

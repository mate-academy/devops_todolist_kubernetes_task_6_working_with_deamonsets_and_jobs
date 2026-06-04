# Deployment Instructions

## Prerequisites

Ensure the ToDo app and its ClusterIP service are running in the `todoapp` namespace:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

Create the `mateapp` namespace if it does not exist:

```bash
kubectl create namespace mateapp
```

## Deploy DaemonSet and CronJob

Apply the manifests:

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

Verify that resources were created:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

## Validate the Solution

### DaemonSet

Check that a pod is running on each worker node:

```bash
kubectl get pods -n mateapp -l app=todoapp-curl-daemonset -o wide
```

View logs from a DaemonSet pod (replace `<pod-name>` with an actual pod name):

```bash
kubectl logs -n mateapp <pod-name>
```

The logs should show repeated HTTP responses from the ToDo app service, confirming that `curl` runs every 5 seconds.

### CronJob

Check the CronJob status:

```bash
kubectl get cronjob -n mateapp
```

Wait at least 4 minutes for the first scheduled run, then list the created Jobs:

```bash
kubectl get jobs -n mateapp
```

View logs from a CronJob pod (replace `<job-pod-name>` with an actual pod name):

```bash
kubectl logs -n mateapp <job-pod-name>
```

The logs should contain a successful response from the `/api/health` endpoint.

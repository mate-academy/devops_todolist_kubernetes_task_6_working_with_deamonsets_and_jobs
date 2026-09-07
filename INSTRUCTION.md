# ToDo App Deployment Instructions

This document provides instructions on how to deploy the DaemonSet and CronJob manifests to the Kubernetes cluster and how to validate their successful execution.

## 1. Deployment

Both manifests are configured to be deployed into the `mateapp` namespace. To apply the configurations to your cluster, run the following commands from the directory where your `.yml` files are located:

```bash
# Apply the DaemonSet manifest
kubectl apply -f daemonset.yml

# Apply the CronJob manifest
kubectl apply -f cronjob.yml
```

*(Note: If the `mateapp` namespace does not exist yet, create it first using `kubectl create namespace mateapp`)*

## 2. Validation

After deploying, you can validate the solution by checking the status of the resources and inspecting their logs.

### Step 2.1: Verify Resources are Running

Check if the DaemonSet and CronJob resources are created successfully:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
kubectl get pods -n mateapp
```

### Step 2.2: Check DaemonSet Logs

The DaemonSet executes a health check via `curl` every 5 seconds. To view these logs, use the name of your DaemonSet (e.g., `todo`):

```bash
kubectl logs daemonset/todo -n mateapp
```

### Step 2.3: Check CronJob Logs

The CronJob runs every 4 minutes. First, verify that a Job has been triggered:

```bash
kubectl get jobs -n mateapp
```

Once you see a completed Job, find the corresponding Pod name in the pods list (`kubectl get pods -n mateapp`). Then, check its logs to confirm the `/api/health` endpoint was called successfully:

```bash
kubectl logs <pod-name> -n mateapp
```
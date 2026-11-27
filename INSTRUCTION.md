




# INSTRUCTION.md
# DaemonSet and CronJob Deployment Guide

## 1. Overview

This document describes how to deploy and validate the Kubernetes DaemonSet and CronJob resources in the `mateapp` namespace.

The solution consists of two workloads:

| Resource | Purpose | Execution interval |
|---|---|---|
| DaemonSet (`mate-server`) | Sends HTTP requests to the `todoapp` ClusterIP Service | Every 5 seconds |
| CronJob (`cronjob`) | Calls the `/api/health` endpoint of the `todoapp` Service | Every 4 minutes |

Both workloads use the `busyboxplus:curl` container image.

## 2. Prerequisites

Before deploying the resources, make sure:

- A Kubernetes cluster is running.
- `kubectl` is configured to communicate with the cluster.
- The `mateapp` namespace exists.
- The `todoapp` Service is deployed in the `mateapp` namespace.
- The `todoapp` application exposes the `/api/health` endpoint.

Verify the cluster connection:

```bash
kubectl cluster-info
kubectl get nodes
```

Verify the namespace and Service:

```bash
kubectl get namespace mateapp
kubectl get service todoapp -n mateapp
```

If the namespace does not exist, create it:

```bash
kubectl create namespace mateapp
```

## 3. Deploy the DaemonSet

The DaemonSet continuously executes a curl command against the `todoapp` Service every five seconds.

### 3.1. Deploy

From the directory containing `daemonset.yml`, execute:

```bash
kubectl apply -f daemonset.yml
```

### 3.2. Verify deployment

Check the DaemonSet status:

```bash
kubectl get daemonset mate-server -n mateapp
```

Check the Pods created by the DaemonSet:

```bash
kubectl get pods -n mateapp -l app=mate-server -o wide
```

The DaemonSet should maintain one Pod on each eligible node.

The `DESIRED`, `CURRENT`, and `READY` counts should normally match.

### 3.3. Validate DaemonSet logs

Display the logs of all DaemonSet Pods:

```bash
kubectl logs -n mateapp -l app=mate-server --all-containers=true --prefix=true -f
```

The logs should show the responses returned by the `todoapp` Service.

New requests should appear approximately every five seconds.

Press `Ctrl+C` to stop following the logs.

To inspect an individual Pod:

```bash
kubectl get pods -n mateapp -l app=mate-server
```

Then:

```bash
kubectl logs -n mateapp <pod-name>
```

## 4. Deploy the CronJob

The CronJob periodically executes a curl command against the `/api/health` endpoint of the `todoapp` Service.

Its schedule is configured as:

```yaml
schedule: "*/4 * * * *"
```

This means the CronJob executes every four minutes.

### 4.1. Deploy

From the directory containing `cronjob.yml`, execute:

```bash
kubectl apply -f cronjob.yml
```

### 4.2. Verify deployment

Check the CronJob:

```bash
kubectl get cronjob cronjob -n mateapp
```

Check the schedule and execution information:

```bash
kubectl describe cronjob cronjob -n mateapp
```

### 4.3. Verify Job creation

Kubernetes creates a new Job every time the CronJob schedule is triggered.

List the Jobs:

```bash
kubectl get jobs -n mateapp
```

List the Pods created by the Jobs:

```bash
kubectl get pods -n mateapp -l job-name
```

### 4.4. Validate CronJob logs

First, identify the Job created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

Display its logs:

```bash
kubectl logs -n mateapp job/<job-name>
```

The logs should show the response returned by the `/api/health` endpoint.

A successful HTTP response should result in a successful Job execution.

The `curl -f` option ensures that HTTP error responses (400 and above) cause curl to return a non-zero exit status.

### 4.5. Trigger a manual execution (optional)

Instead of waiting four minutes for the scheduled execution, create a Job manually from the CronJob:

```bash
kubectl create job cronjob-manual --from=cronjob/cronjob -n mateapp
```

Wait for the Job to complete:

```bash
kubectl wait --for=condition=complete job/cronjob-manual -n mateapp --timeout=120s
```

Check its status:

```bash
kubectl get job cronjob-manual -n mateapp
```

Display its logs:

```bash
kubectl logs -n mateapp job/cronjob-manual
```

This allows the health-check functionality to be validated immediately.

## 5. Troubleshooting

If the DaemonSet or CronJob does not work as expected, use the following commands.

### Check resource status

```bash
kubectl get all -n mateapp
```

### Inspect DaemonSet

```bash
kubectl describe daemonset mate-server -n mateapp
```

### Inspect CronJob

```bash
kubectl describe cronjob cronjob -n mateapp
```
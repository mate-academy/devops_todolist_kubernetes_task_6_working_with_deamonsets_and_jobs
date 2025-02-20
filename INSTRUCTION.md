# Deployment Instructions

## Prerequisites
1. A Kubernetes cluster is running and accessible.
2. You have `kubectl` installed and configured to interact with the cluster.

---

## Deployment Steps

1. Create the `mateapp` namespace:
```bash
   kubectl create namespace mateapp
```
2. Apply the daemonset.yml file:
``` bash
kubectl apply -f .infrastructure/daemonset.yml
```
3. Apply the cronjob.yml file:
```bash
kubectl apply -f .infrastructure/cronjob.yml
```
# Validation Steps
## DaemonSet

1. Check the pods created by the DaemonSet:
```bash
kubectl get pods -n mateapp -l app=todoapp-daemon
```
2. Check the logs of any DaemonSet pod:
```bash
kubectl logs -n mateapp <daemonset-pod-name>
```
Example output should show repeated curl requests to the todoapp service.

## CronJob
1. Check the status of CronJobs:
```bash
kubectl get cronjob -n mateapp
```
2. Check the jobs triggered by the CronJob:
```bash
kubectl get jobs -n mateapp
```
3. Check logs of a specific job pod:
```bash
kubectl logs -n mateapp <cronjob-pod-name>
```
Example output should confirm the /api/health endpoint call.

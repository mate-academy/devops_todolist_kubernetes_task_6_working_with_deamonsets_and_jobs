# Deployment Instructions for DaemonSet and CronJob

## 1. Create Namespace
```
kubectl create namespace mateapp
```
## 2. Apply Manifests

```
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```
## 3. Validate DaemonSet
Check pods:
```
kubectl get pods -n mateapp -l app=busybox-curl
```
View logs for a pod:
```
kubectl logs <pod-name> -n mateapp
```
You should see curl outputs every 5 seconds hitting the todoapp service.

## 4. Validate CronJob
Check CronJobs:


```
kubectl get cronjob -n mateapp
```
Check recent Jobs:

```
kubectl get jobs -n mateapp
```
View logs of a job pod:

```
kubectl logs <job-pod-name> -n mateapp
```
```
kubectl describe cronjob todoapp-healthcheck -n mateapp
```
You should see curl output hitting /api/health endpoint.

Notes
DaemonSet runs one pod per node, continuously curling the ClusterIP service.

CronJob runs every 4 minutes, keeps 10 successful and 5 failed job records, with concurrencyPolicy: Allow.

Resources (CPU/memory) are defined in both manifests.
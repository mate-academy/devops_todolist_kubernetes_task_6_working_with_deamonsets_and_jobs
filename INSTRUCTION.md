# ToDo App Kubernetes DaemonSet & CronJob

## 1. How to deploy to the cluster
Apply the DaemonSet and CronJob manifests to the `mateapp` namespace (ensure the `todoapp-clusterip` service from previous tasks is running):
```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## 2. Configuration Rationale (Why these settings?)
Based on Kubernetes best practices, the following parameters were strictly configured:
* **Resource Limits:** Crucial for DaemonSets. A memory leak in a DaemonSet could bring down entire nodes. Hard limits (`50m CPU`, `32Mi Memory`) are set to prevent this.
* **concurrencyPolicy: Allow:** Configured for the CronJob to allow parallel execution if a previous job takes longer than the 4-minute schedule interval.
* **History Limits:** `successfulJobsHistoryLimit: 10` and `failedJobsHistoryLimit: 5` prevent the cluster (and etcd) from being cluttered with hundreds of stale Completed pods.
* **restartPolicy: OnFailure:** Set in the CronJob template because Jobs are designed to run to completion, unlike Deployments which run continuously.

## 3. How to validate the solution (Logs)

### Validate DaemonSet
The DaemonSet executes a curl command every 5 seconds. To view its real-time logs:
```bash
# 1. Fetch logs directly from the DaemonSet resource
kubectl logs daemonset/todoapp-daemonset -n mateapp

# OR verify a specific pod:
# 2. Get the DaemonSet pod name
kubectl get pods -n mateapp -l app=todo-checker

# 3. Follow the logs of that specific pod (-f for continuous stream)
kubectl logs -f <pod-name> -n mateapp
```
*(Press Ctrl+C to stop following the logs).*

### Validate CronJob
The CronJob executes every 4 minutes. It may take up to 4 minutes after deployment to see the first run.
```bash
# 1. Check if the CronJob has been scheduled successfully
kubectl get cronjob todoapp-cronjob -n mateapp

# 2. List the jobs spawned by the CronJob (history of runs)
kubectl get jobs -n mateapp

# 3. Find the pod created by the completed Job (Status should be 'Completed')
kubectl get pods -n mateapp | grep todoapp-cronjob

# 4. View the logs of the completed job pod to see the /api/health endpoint response
# (Replace <cronjob-pod-name> with the actual pod name from the previous step)
kubectl logs <cronjob-pod-name> -n mateapp
```
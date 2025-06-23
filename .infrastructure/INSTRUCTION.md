# Instructions for DaemonSet & CronJob Deployment

## 1. Deploy Namespace

kubectl apply -f .infrastructure/namespace.yml

2. Deploy DaemonSet

kubectl apply -f .infrastructure/daemonset.yml
Check the logs from any pod (example for one node):


kubectl get pods -n mateapp
kubectl logs <daemonset-pod-name> -n mateapp

3. Deploy CronJob

kubectl apply -f .infrastructure/cronjob.yml
List CronJob runs:


kubectl get jobs -n mateapp
View logs from the latest CronJob pod:


kubectl get pods -n mateapp
kubectl logs <cronjob-pod-name> -n mateapp

4. Configuration Explanation

DaemonSet
Image: ikulyk404/busyboxplus:curl

Purpose: Continuously curl the todoapp service every 5 seconds from each node.

Resources: Light requests/limits for lightweight operation.

CronJob
Image: ikulyk404/busyboxplus:curl

Schedule: Every 4 minutes.

Target Endpoint: /api/health on the service.

Concurrency: Allow to permit overlapping runs if previous didn’t finish.

5. Verification

Check logs for DaemonSet pods to confirm periodic curl runs.

Check CronJob logs to confirm health checks run every 4 minutes.
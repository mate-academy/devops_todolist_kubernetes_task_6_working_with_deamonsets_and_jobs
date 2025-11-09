# Deployment Instructions for DaemonSet and CronJob

## 1. Deploy DaemonSet

1. Apply the DaemonSet manifest:
   ```bash
   kubectl apply -f daemonset.yml


Verify the DaemonSet pods:

kubectl get pods -o wide


2. Deploy CronJob

Apply the CronJob manifest:

kubectl apply -f cronjob.yml


Verify that the CronJob was created successfully:

kubectl get cronjobs -o wide


Example output:

NAME            SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE   CONTAINERS   IMAGES             SELECTOR
hello-cronjob   */4 * * * *   False     0        <none>          29s   hello        busyboxplus:curl    <none>

3. Validation Instructions
Validate DaemonSet

List all pods managed by the DaemonSet:

kubectl get pods -o wide


Check logs from one of the DaemonSet pods:

kubectl logs <daemonset-pod-name>

Validate CronJob

List all CronJobs:

kubectl get cronjobs


Identify the pod created by the CronJob and check logs:

kubectl get pods | grep hello-cronjob
kubectl logs <cronjob-pod-name>
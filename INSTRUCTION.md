1. Preparation

Make sure the mateapp namespace exists. If not, create it:

kubectl create namespace mateapp

2. Deploy DaemonSet

Apply the DaemonSet manifest:

kubectl apply -f .infrastructure/daemonset.yml


Verify that all Pods are created:

kubectl -n mateapp get pods -l app=todoapp-curl


Check the logs of a Pod to validate the curl command execution:

kubectl -n mateapp logs <pod-name>


You should see output similar to:

2025-11-15T15:00:05 curl -> http://todoapp:80


This confirms that the DaemonSet executes the curl command every 5 seconds.

3. Deploy CronJob

Apply the CronJob manifest:

kubectl apply -f .infrastructure/cronJob.yml


Verify that the CronJob is created:

kubectl -n mateapp get cronjob


Check logs of a CronJob Pod:

kubectl -n mateapp logs <job-pod-name>


You should see output similar to:

2025-11-15T15:04:00 curling /api/health

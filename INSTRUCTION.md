## Deploying the DaemonSet

1. Apply the DaemonSet manifest:
   ```
   kubectl apply -f .infrastructure/daemonset.yml
   ```

2. Verify the DaemonSet is running:
   ```
   kubectl get daemonset -n mateapp
   ```

## Deploying the CronJob

1. Apply the CronJob manifest:
   ```
   kubectl apply -f .infrastructure/cronjob.yml
   ```

2. Verify the CronJob is created:
   ```
   kubectl get cronjob -n mateapp
   ```

## Validation Instructions

### DaemonSet Logs

To check that the DaemonSet is executing the `curl` command every 5 seconds:

1. List the pods created by the DaemonSet:
   ```
   kubectl get pods -n mateapp -l app=my-app
   ```

2. View logs from one of the pods:
   ```
   kubectl logs <daemonset-pod-name> -n mateapp
   ```

You should see repeated output from the `curl` command to the todoapp service.

### CronJob Logs

To check that the CronJob is calling the `/api/health` endpoint every 4 minutes:

1. List the jobs created by the CronJob:
   ```
   kubectl get jobs -n mateapp
   ```

2. List pods created by the jobs:
   ```
   kubectl get pods -n mateapp -l job-name=<job-name>
   ```

3. View logs from a CronJob pod:
   ```
   kubectl logs <cronjob-pod-name> -n mateapp
   ```

You should see the output from the `curl http://todoapp-service:8080/api/health` command.
# ToDo Application Instructions

## Instructions for deploying daemonset.yml and cronjob.yml files

Use these commands:

```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## Instructions for checking the correctness of the task execution and commands for viewing DaemonSet and CronJob logs

### DaemonSet Check

First check that the DaemonSet is created:
```bash
kubectl get daemonset -n mateapp
```
Then look at the Pod he created:
```bash
kubectl get pods -n mateapp -l app=todoapp-checker
```
Pod should have a Running status

Next, look at the logs:
```bash
kubectl logs -n mateapp -l app=todoapp-checker
```

A response from todoapp-service should appear in the logs approximately every 5 seconds.


### CronJob Check

Verify that the CronJob has been created:
```bash
kubectl get cronjob -n mateapp
```

The SCHEDULE column should contain:
```bash
*/4 * * * *
```

Check the Job created by him:
```bash
kubectl get jobs -n mateapp
```

Check the Pod created by the CronJob:
```bash
kubectl get pods -n mateapp
```

To view logs for a specific Pod:
```bash
kubectl logs <cronjob-pod-name> -n mateapp
```

The logs should contain the endpoint response.
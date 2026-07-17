# Instructions

## Prerequisites

The DaemonSet and CronJob send `curl` requests to the todoapp ClusterIP service, so the app and its service must be running first.

> Note: the todoapp Deployment and ClusterIP service run in the `todoapp` namespace, while the DaemonSet and CronJob run in the `mateapp` namespace.

Deploy the todoapp service and application:

```bash
kubectl apply -f clusterip.yml
kubectl apply -f deployment.yml
```

## Deploy

Apply the DaemonSet and CronJob manifests:

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## Validation

### DaemonSet

Find the DaemonSet pod name:

```bash
kubectl get pods -n mateapp
```

Check its logs — you should see the HTML response from the todoapp service (a new request every 5 seconds):

```bash
kubectl logs <daemonset-pod-name> -n mateapp
```

If you see `connection refused`, something is misconfigured (most likely a port mismatch between the service and the app).

### CronJob

The CronJob runs every 4 minutes. You can either wait for a scheduled run:

```bash
kubectl get jobs -n mateapp
kubectl logs job/<job-name> -n mateapp
```

Or trigger a run manually without waiting for the schedule:

```bash
kubectl create job --from=cronjob/cron-health-check manual-test -n mateapp
kubectl logs job/manual-test -n mateapp
```

You should see the HTML response from the `/api/health` endpoint.
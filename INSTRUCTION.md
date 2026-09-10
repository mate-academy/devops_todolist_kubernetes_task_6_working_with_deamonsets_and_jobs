# DaemonSet and CronJob Deployment

## Prerequisites

* Kubernetes cluster
* Existing todoapp deployment
* Existing ClusterIP service for todoapp
* Namespace `mateapp`

## Deploy

Apply DaemonSet:

```bash
kubectl apply -f daemonset.yml
```

Apply CronJob:

```bash
kubectl apply -f cronjob.yml
```

Verify resources:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

## Validation

### DaemonSet

Check DaemonSet pods:

```bash
kubectl get pods -n mateapp
```

View logs:

```bash
kubectl logs -n mateapp <daemonset-pod-name>
```

The logs should show curl requests executed every 5 seconds.

### CronJob

Check CronJob:

```bash
kubectl get cronjob -n mateapp
```

Check Jobs:

```bash
kubectl get jobs -n mateapp
```

View Job logs:

```bash
kubectl logs -n mateapp <job-pod-name>
```

A new Job should be created every 4 minutes and call:

```text
/api/health
```

through the ClusterIP service.

## Resource Requests and Limits

Requests:

* CPU: 50m
* Memory: 64Mi

Limits:

* CPU: 100m
* Memory: 128Mi

These values are sufficient because the containers only execute lightweight curl requests.

## CronJob Configuration

* Schedule: every 4 minutes (`*/4 * * * *`)
* successfulJobsHistoryLimit: 10
* failedJobsHistoryLimit: 5
* concurrencyPolicy: Allow

This configuration satisfies the task requirements while keeping enough job history for troubleshooting.

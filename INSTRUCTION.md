# Kubernetes Deployment Instructions

## Deployment

Run the following commands from the repository root:

```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## Validation

### Check the DaemonSet

```bash
kubectl get daemonset todoapp-daemonset -n mateapp
kubectl get pods -n mateapp
```

Check logs of one DaemonSet pod:

```bash
kubectl logs <daemonset-pod-name> -n mateapp
```

The logs should show requests sent to the `todoapp-service` ClusterIP Service every 5 seconds.

### Check the CronJob

```bash
kubectl get cronjob health-check-cronjob -n mateapp
kubectl get jobs -n mateapp
```

Check logs of a completed CronJob Job:

```bash
kubectl logs job/<job-name> -n mateapp
```

The CronJob runs every 4 minutes and sends a request to:

```text
http://todoapp-service:80/api/health
```
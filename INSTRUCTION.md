# Instructions

## Deploy

1. Ensure namespace `mateapp` exists (create if needed):
   ```bash
   kubectl create namespace mateapp
   ```

2. Apply manifests:
   ```bash
   kubectl apply -f .infrastructure/daemonset.yml
   kubectl apply -f .infrastructure/cronjob.yml
   ```

## Validate

**Resources**
```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

**DaemonSet logs** (one pod per node; replace `<pod-name>` with a pod from `kubectl get pods -n mateapp`):
```bash
kubectl logs -n mateapp <pod-name>
```
You should see periodic curl output (every 5 seconds).

**CronJob logs** (runs every 4 minutes; list jobs then pick a completed job's pod):
```bash
kubectl get jobs -n mateapp
kubectl logs -n mateapp job/<job-name>
```
Or get the pod name from the job and run:
```bash
kubectl logs -n mateapp <cronjob-pod-name>
```

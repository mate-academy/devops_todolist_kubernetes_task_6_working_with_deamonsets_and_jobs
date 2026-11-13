# Deployment and validation instructions

Both manifests live in the `mateapp` namespace and call the todoapp through its ClusterIP service (`todoapp-service`), so the todoapp Deployment and Service must already be running there.

## Deploy

```bash
# 1. Create the namespace (skip if it already exists)
kubectl create namespace mateapp

# 2. Make sure the todoapp service exists
kubectl get svc todoapp-service -n mateapp

# 3. Apply the manifests
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## Validate

### DaemonSet

```bash
# One pod per node should be Running
kubectl get daemonset todoapp-daemonset -n mateapp
kubectl get pods -n mateapp -l app=todoapp-daemonset

# Logs: an HTTP status code (e.g. 200) should appear every 5 seconds
kubectl logs -n mateapp -l app=todoapp-daemonset --tail=20 -f
```

### CronJob

```bash
# The CronJob should exist with schedule */4 * * * *
kubectl get cronjob todoapp-cronjob -n mateapp

# Jobs appear every 4 minutes (history: 10 successful, 5 failed)
kubectl get jobs -n mateapp

# Logs of the latest run: the /api/health response
kubectl logs -n mateapp job/$(kubectl get jobs -n mateapp \
  --sort-by=.metadata.creationTimestamp \
  -o jsonpath='{.items[-1].metadata.name}')

# Optional: trigger a run immediately instead of waiting 4 minutes
kubectl create job --from=cronjob/todoapp-cronjob manual-test -n mateapp
kubectl logs -n mateapp job/manual-test
```

## Clean up

```bash
kubectl delete -f .infrastructure/daemonset.yml -f .infrastructure/cronjob.yml
```

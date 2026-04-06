## Kubernetes task 6 — DaemonSets and CronJobs

### Apply manifests

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## Validate DaemonSet (logs)
DaemonSet makes a request every 5 seconds to the ClusterIP service:

```bash
kubectl -n mateapp logs -l app=todoapp-1 --tail=50
```

### Validate CronJob (jobs + logs)
CronJob runs every 4 minutes and calls `/api/health` via the ClusterIP service.

Wait a few minutes, then:

```bash
kubectl -n mateapp get jobs --sort-by=.metadata.creationTimestamp
kubectl logs -l job-name -n mateapp
```

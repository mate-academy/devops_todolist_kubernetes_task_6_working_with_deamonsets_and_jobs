## Kubernetes task 6 — DaemonSets and CronJobs

### Apply manifests

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

Check status:

```bash
kubectl -n mateapp get deploy,pods,svc,ds,cronjob,job
kubectl -n mateapp get pods -l app=todoapp-curl -o wide
```

### Validate DaemonSet (logs)
DaemonSet makes a request every 5 seconds to the ClusterIP service:

```bash
kubectl -n mateapp logs -l app=todoapp-curl --tail=50
```

### Validate CronJob (jobs + logs)
CronJob runs every 4 minutes and calls `/api/health` via the ClusterIP service.

Wait a few minutes, then:

```bash
kubectl -n mateapp get jobs --sort-by=.metadata.creationTimestamp
kubectl -n mateapp get pods -l job-name --show-labels
```

Show logs for the latest job (replace `<JOB_NAME>` with the newest job from the `get jobs` output):

```bash
kubectl -n mateapp logs job/<JOB_NAME> --tail=50
```

If you want to trigger a run immediately (optional):

```bash
kubectl -n mateapp create job --from=cronjob/todoapp-health todoapp-health-manual
kubectl -n mateapp logs job/todoapp-health-manual
```


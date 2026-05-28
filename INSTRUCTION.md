# Deploy DaemonSet

Apply daemonset manifest:

```bash id="m6x8az"
kubectl apply -f .infrastructure/daemonset.yml
```

Check daemonset:

```bash id="h5u2pq"
kubectl get daemonset -n mateapp
```

Check daemonset pods:

```bash id="t1r7ke"
kubectl get pods -n mateapp
```

View daemonset logs:

```bash id="y8c4fw"
kubectl logs -n mateapp -l app=todoapp-daemon
```

---

# Deploy CronJob

Apply cronjob manifest:

```bash id="p3v9nm"
kubectl apply -f .infrastructure/cronjob.yml
```

Check cronjob:

```bash id="n7w5ds"
kubectl get cronjob -n mateapp
```

Check jobs:

```bash id="q2x6jc"
kubectl get jobs -n mateapp
```

Check pods created by cronjob:

```bash id="f4m8ha"
kubectl get pods -n mateapp
```

View cronjob logs:

```bash id="s9u7lb"
kubectl logs -n mateapp job/<job-name>
```

---

# Validation

## Validate DaemonSet

DaemonSet should:

* Run one pod per node
* Execute curl requests every 5 seconds
* Show successful responses in logs

Example:

```bash id="a5j3xe"
kubectl logs -n mateapp -l app=todoapp-daemon
```

---

# Validate CronJob

CronJob should:

* Run every 4 minutes
* Create Job resources
* Keep 10 successful jobs
* Keep 5 failed jobs

Check jobs:

```bash id="r6k2pn"
kubectl get jobs -n mateapp
```

Check logs:

```bash id="v4n9ys"
kubectl logs -n mateapp job/<job-name>
```

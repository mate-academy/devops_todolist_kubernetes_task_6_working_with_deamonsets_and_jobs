# TodoApp DaemonSet & CronJob Instructions

## 1. Deploy to cluster

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

Verify resources are created:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

---

## 2. Validate DaemonSet

Check that DaemonSet pods are running:

```bash
kubectl get pods -n mateapp -l app=todoapp-daemonset
```

View logs to confirm curl is running every 5 seconds:

```bash
kubectl logs -l app=todoapp-daemonset -n mateapp
```

You should see repeated responses from the `/api/health` endpoint every 5 seconds.

---

## 3. Validate CronJob

Check CronJob status:

```bash
kubectl get cronjob -n mateapp
```

List jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

View logs of the most recent CronJob run:

```bash
kubectl logs -l job-name=<job-name> -n mateapp
```

To get the job name run:

```bash
kubectl get jobs -n mateapp
```

You should see a successful curl response from `/api/health` in the logs.

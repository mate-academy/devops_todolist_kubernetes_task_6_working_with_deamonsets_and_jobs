# Task 6: DaemonSet and CronJob

## Prerequisites

- A running Kubernetes cluster and `kubectl` configured.
- The ToDo app must already be deployed in namespace **`mateapp`** with ClusterIP service **`todoapp-service`** (see `.infrastructure/deployment.yml` and `.infrastructure/clusterIp.yml` from previous tasks).

## Deploy the app stack (if not applied yet)

From the repository root:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl wait --for=condition=Available deployment/todoapp -n mateapp --timeout=180s
```

Optional (not required for DaemonSet/CronJob validation): HPA, NodePort.

## Deploy DaemonSet and CronJob

```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

Check status:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
kubectl get pods -n mateapp -l app=todoapp-curl
```

DaemonSet pods should be **Running** on each node (one pod per node). The first CronJob run appears within **4 minutes** (`*/4 * * * *`).

If the image does not pull, use the same image as in the course materials, for example `ikulyk404/busyboxplus:curl`, in both manifests.

## Validate DaemonSet (logs)

Every **5 seconds** the DaemonSet container calls the todoapp ClusterIP service.

List DaemonSet pods:

```bash
kubectl get pods -n mateapp -l app=todoapp-curl -o wide
```

Stream logs from one pod (you should see repeated `curl` output / HTTP body from `/api/health`):

```bash
POD=$(kubectl get pods -n mateapp -l app=todoapp-curl -o jsonpath='{.items[0].metadata.name}')
kubectl logs -n mateapp "$POD" --tail=20
kubectl logs -n mateapp "$POD" -f
```

Follow for ~15 seconds and confirm new log lines appear about every 5 seconds.

## Validate CronJob (logs and history)

List jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
kubectl get cronjob todoapp-cronjob-healthcheck -n mateapp
```

After a scheduled run, inspect the latest job name and its logs:

```bash
JOB=$(kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1].metadata.name}')
kubectl logs -n mateapp "job/${JOB}"
```

You should see a successful response from `http://todoapp-service/api/health`.

Verify history limits (after several runs):

```bash
kubectl describe cronjob todoapp-cronjob-healthcheck -n mateapp
```

Expect up to **10** successful and **5** failed finished jobs kept in history (`successfulJobsHistoryLimit: 10`, `failedJobsHistoryLimit: 5`, `concurrencyPolicy: Allow`).

## Quick health check

```bash
kubectl get all -n mateapp
kubectl logs -n mateapp -l app=todoapp-curl --tail=5
```

## Clean up (optional)

```bash
kubectl delete -f .infrastructure/daemonset.yml
kubectl delete -f .infrastructure/cronjob.yml
```

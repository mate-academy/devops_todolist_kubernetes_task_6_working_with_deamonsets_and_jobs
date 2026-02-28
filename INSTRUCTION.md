# Deployment Instructions

## Prerequisites

- A running Kubernetes cluster (e.g. [minikube](https://minikube.sigs.k8s.io/), [kind](https://kind.sigs.k8s.io/), or any cloud provider)
- `kubectl` installed and configured to point at your cluster
- The `todoapp` namespace and its ClusterIP service (`todoapp-service`) already deployed

---

## 1. Ensure the `todoapp` namespace and service exist

The DaemonSet and CronJob communicate with the todoapp via its ClusterIP service.
Apply the existing base manifests first if you haven't already:

```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f clusterIp.yml
```

Verify the service is up:

```bash
kubectl get svc -n todoapp
# Expected: todoapp-service   ClusterIP   <ip>   <none>   80/TCP
```

---

## 2. Create the `mateapp` namespace

Both new manifests belong to the `mateapp` namespace. Create it before applying them:

```bash
kubectl create namespace mateapp
```

---

## 3. Deploy the DaemonSet

The DaemonSet schedules a `busyboxplus:curl` pod on **every node** in the cluster.
Every 5 seconds it sends a `curl` request to the todoapp's root endpoint.

```bash
kubectl apply -f daemonset.yml
```

Verify it is running:

```bash
kubectl get daemonset -n mateapp
# Expected: todoapp-curl-daemonset   <N>   <N>   <N>   <N>   <N>
```

---

## 4. Deploy the CronJob

The CronJob runs every 4 minutes and calls the `/api/health` endpoint of the todoapp.

```bash
kubectl apply -f cronjob.yml
```

Verify it was created:

```bash
kubectl get cronjob -n mateapp
# Expected: todoapp-health-cronjob   */4 * * * *   False   0   <last-schedule>   <age>
```

---

## 5. Validation

### Validate the DaemonSet

Check that a DaemonSet pod is running on each node:

```bash
kubectl get pods -n mateapp -l app=todoapp-curl-daemonset -o wide
```

Stream live logs from a DaemonSet pod to confirm curl requests are firing every 5 seconds:

```bash
# Get the pod name first
kubectl get pods -n mateapp -l app=todoapp-curl-daemonset

# Then tail its logs
kubectl logs -n mateapp -l app=todoapp-curl-daemonset --follow
```

Expected output (repeated every 5 seconds):
```
<!DOCTYPE html>
<html>
...
```
or an HTTP response from the todoapp root page.

---

### Validate the CronJob

Check the CronJob schedule and last run time:

```bash
kubectl get cronjob -n mateapp todoapp-health-cronjob
```

List Jobs created by the CronJob (up to 10 successful + 5 failed kept in history):

```bash
kubectl get jobs -n mateapp
```

View logs from the most recent CronJob run:

```bash
# Get the pod created by the latest job
kubectl get pods -n mateapp -l app=todoapp-health-cronjob

# Fetch its logs
kubectl logs -n mateapp -l app=todoapp-health-cronjob
```

Expected output:
```json
{"status": "ok"}
```
or a `200 OK` JSON health response from `/api/health`.

To manually trigger the CronJob immediately (for quick testing):

```bash
kubectl create job --from=cronjob/todoapp-health-cronjob manual-health-test -n mateapp
kubectl logs -n mateapp -l app=todoapp-health-cronjob --follow
```

---

## Summary of applied manifests

| File            | Kind       | Namespace | Purpose                                      |
|-----------------|------------|-----------|----------------------------------------------|
| `daemonset.yml` | DaemonSet  | mateapp   | Curl todoapp root `/` every 5 seconds        |
| `cronjob.yml`   | CronJob    | mateapp   | Curl todoapp `/api/health` every 4 minutes   |
# Deployment Instructions for DaemonSet, CronJob and ToDo App

## 1. Prerequisites

- Kubernetes cluster is running
- `kubectl` is configured
- You are in the .infrastructure directory containing the following files:
  - `namespace-mateapp.yml`
  - `daemonset.yml`
  - `cronjob.yml`
  - `todoapp-deployment.yml`
  - `todoapp-service.yml`

---

## 2. Create Namespace

First, create the `mateapp` namespace:

```bash
kubectl apply -f namespace-mateapp.yml
```

You can confirm it was created:

```bash
kubectl get namespaces
```

---

## 3. Deploy ToDo App

Apply the ToDo app deployment:

```bash
kubectl apply -f todoapp-deployment.yml
```

Then expose it with the service:

```bash
kubectl apply -f todoapp-service.yml
```

Check that the pod is running:

```bash
kubectl get pods -n mateapp -l app=todoapp
```
```
NAME                      READY   STATUS    RESTARTS   AGE
todoapp-9667f954d-tqjhs   1/1     Running   0          16s
```

```bash
kubectl logs todoapp-9667f954d-tqjhs -n mateapp
```
```
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).
May 21, 2025 - 23:36:15
Django version 4.1.10, using settings 'todolist.settings'
Starting development server at http://0.0.0.0:8000/
Quit the server with CONTROL-C.
[21/May/2025 23:36:18] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:23] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:28] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:33] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:38] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:43] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:48] "GET /api/readiness HTTP/1.1" 200 19
[21/May/2025 23:36:53] "GET /api/readiness HTTP/1.1" 200 19
```
---

## 4. Deploy DaemonSet

Apply the DaemonSet manifest:

```bash
kubectl apply -f daemonset.yml
```

Confirm the DaemonSet and its pods are running:

```bash
kubectl get daemonset -n mateapp
```

```
NAME          DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
curl-daemon   1         1         1       1            1           <none>          7s
```

---

## 5. Deploy CronJob

Apply the CronJob manifest:

```bash
kubectl apply -f cronjob.yml
```

Check the CronJob is scheduled:

```bash
kubectl get cronjob -n mateapp
```

```
NAME        SCHEDULE      TIMEZONE   SUSPEND   ACTIVE   LAST SCHEDULE   AGE
curl-cron   */4 * * * *   <none>     False     0        <none>          5s
```

> **Note:** `concurrencyPolicy: Allow` means the CronJob is allowed to run multiple jobs in parallel if a previous one is still running.

---

## 6. Validate DaemonSet

List the DaemonSet pods:

```bash
kubectl get pods -n mateapp -l app=curl-daemon
```

```
NAME                READY   STATUS    RESTARTS   AGE
curl-daemon-ckw6r   1/1     Running   0          48s
```

Check logs of any pod (replace `<pod-name>`):

```bash
kubectl logs curl-daemon-ckw6r -n mateapp
```

You should see repeated `curl` requests every 5 seconds.

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  38556      0 --:--:-- --:--:-- --:--:-- 47000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  29565      0 --:--:-- --:--:-- --:--:-- 35250
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  24995      0 --:--:-- --:--:-- --:--:-- 28200
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  52513      0 --:--:-- --:--:-- --:--:-- 70500
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  38273      0 --:--:-- --:--:-- --:--:-- 47000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  81034      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  31893      0 --:--:-- --:--:-- --:--:-- 35250
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  71537      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  32676      0 --:--:-- --:--:-- --:--:-- 47000
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  77007      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  29715      0 --:--:-- --:--:-- --:--:-- 35250
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  81739      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  50682      0 --:--:-- --:--:-- --:--:-- 70500
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  71464      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  56786      0 --:--:-- --:--:-- --:--:-- 70500
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  65398      0 --:--:-- --:--:-- --:--:--  137k
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  31778      0 --:--:-- --:--:-- --:--:-- 35250
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   141  100   141    0     0  58409      0 --:--:-- --:--:-- --:--:-- 70500
```

---

## 7. Validate CronJob

After a few minutes, list the jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```
```
NAME                 STATUS     COMPLETIONS   DURATION   AGE
curl-cron-29131180   Complete   1/1           3s         4m46s
curl-cron-29131184   Complete   1/1           3s         46s
```
Check logs of a recent job (replace `<job-name>`):

```bash
kubectl logs job/curl-cron-29131180  -n mateapp
```

You should see the response from the `/api/liveness` endpoint.

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    19  100    19    0     0  11830      0 --:--:-- --:--:-- --:--:-- 19000
{"status": "alive"}%          
```
---

## Notes

- Both DaemonSet and CronJob use the `ikulyk404/busyboxplus:curl` image.
- Requests are sent to the `todoapp-service` ClusterIP.
- Make sure this service and the `todoapp` application are already deployed in the `mateapp` namespace.

---

## 🧹 Cleanup Instructions

### ❌ Delete all resources in `mateapp` namespace:
```bash
kubectl delete all --all -n mateapp
```

This will remove all: Pods, Deployments, Services, HPAs, etc.

### ❌ Delete the namespace itself:
```bash
kubectl delete namespace mateapp
```

If `kubectl delete namespace mateapp` hangs and `mateapp` remains in `Terminating` state, use the following command to forcefully remove finalizers:

```bash
kubectl get namespace mateapp -o json \
| jq 'del(.spec.finalizers)' \
| kubectl replace --raw "/api/v1/namespaces/mateapp/finalize" -f -
```

This will remove the finalizers and allow the namespace to be deleted properly.

---

### ✅ Confirm everything is gone:
```bash
kubectl get all -A | grep mateapp
```

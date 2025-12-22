# DaemonSet and CronJob Instructions

## Prerequisites

Ensure that the `todoapp` is running and exposed via a ClusterIP service named `todoapp-internal` in the `mateapp` namespace. Without the service, the DNS resolution for `todoapp-internal` will fail.

## 1. How to deploy

1.  **Apply the DaemonSet:**
    ```bash
    kubectl apply -f daemonset.yml
    ```
    This will launch a pod on every available node in your cluster immediately.

2.  **Apply the CronJob:**
    ```bash
    kubectl apply -f cronjob.yml
    ```
    This will register a scheduled task. The first pod will spawn at the next 4-minute mark (e.g., 12:00, 12:04, 12:08).

## 2. Verification

* **Check DaemonSet:**
    ```bash
    kubectl get ds -n mateapp
    # Check logs of one of the pods to see the curls happening every 5s
    kubectl logs -l app=curl-daemon -n mateapp --tail=10 -f
    ```

* **Check CronJob:**
    ```bash
    kubectl get cronjob -n mateapp
    # Wait for 4 minutes, then check if Jobs/Pods were created
    kubectl get jobs -n mateapp
    kubectl get pods -n mateapp | grep health-checker
    ```

---

## 3. Configuration Explanation

### DaemonSet (`daemonset.yml`)

* **Logic:**
    Since DaemonSet pods are designed to be long-running services, we cannot simply execute a single `curl` command (the pod would complete, restart, and enter a CrashLoop). Instead, we used a shell script with a `while true` loop and `sleep 5` to keep the process alive and perform the request periodically.
* **Target:** `http://todoapp-internal.mateapp.svc.cluster.local` (Internal DNS of the service).
* **Resources:** Minimal requests/limits are set since `curl` is very lightweight.

### CronJob (`cronjob.yml`)

* **Schedule (`*/4 * * * *`):** The job runs every 4th minute of every hour.
* **ConcurrencyPolicy (`Allow`):**
    This allows a new job to start even if the previous one is still running (e.g., if the network is slow and the previous curl hangs).
* **History Limits:**
    * `successfulJobsHistoryLimit: 10`: We keep metadata and logs for the last 10 successful checks for debugging or audit purposes.
    * `failedJobsHistoryLimit: 5`: We keep the last 5 failed attempts to analyze why the health check failed.
* **RestartPolicy (`OnFailure`):** If the `curl` command fails (returns non-zero exit code), Kubernetes will try to restart the pod to succeed. If it succeeds, the pod completes.
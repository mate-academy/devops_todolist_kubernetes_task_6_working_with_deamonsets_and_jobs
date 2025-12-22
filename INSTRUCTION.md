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

---

## 2. Verification

### Checking DaemonSet
1.  Verify that DaemonSet pods are running:
    ```bash
    kubectl get ds -n mateapp
    ```
2.  Check the logs to see the loop executing every 5 seconds:
    ```bash
    kubectl logs -l app=curl-daemon -n mateapp --tail=10 -f
    ```

### Checking CronJob
1.  Wait for at least 4 minutes for the first job to trigger.
2.  List the pods created by the CronJob:
    ```bash
    kubectl get pods -n mateapp | grep health-checker
    ```
    *You should see pods with names like `health-checker-28345920-xxxxx` and status `Completed`.*

3.  **Check the logs of a completed job:**
    Copy the name of one of the completed pods from the previous step and run:
    ```bash
    kubectl logs <pod-name> -n mateapp
    ```
    *Example:* `kubectl logs health-checker-28345920-abcde -n mateapp`
    *Expected Output:* You should see the HTML/JSON response from the `curl` command or an empty output if the server returned 200 OK silently (add `-v` to curl in manifest if you need verbose output).

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
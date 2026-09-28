# Kubernetes DaemonSet and CronJob — Deployment & Validation Guide

This document explains how to deploy and validate the DaemonSet and CronJob required by this task.

## Task requirements covered

- `daemonset.yml`
  - runs `ikulyk404/busyboxplus:curl`;
  - belongs to the `mateapp` namespace;
  - has CPU and memory requests/limits;
  - calls the ToDo app through its ClusterIP Service every 5 seconds.

- `cronjob.yml`
  - runs `ikulyk404/busyboxplus:curl`;
  - belongs to the `mateapp` namespace;
  - has CPU and memory requests/limits;
  - runs every 4 minutes;
  - calls `/api/health` through the ToDo app ClusterIP Service;
  - keeps 10 successful Jobs in history;
  - keeps 5 failed Jobs in history;
  - uses `concurrencyPolicy: Allow`.

The existing ToDo application and its ClusterIP Service are expected to run in the `todoapp` namespace.

Because the DaemonSet and CronJob are in another namespace (`mateapp`), they access the Service by its full Kubernetes DNS name:

```text
todoapp-service.todoapp.svc.cluster.local
```

---

## 1. Verify the Kubernetes cluster

```bash
kubectl cluster-info
kubectl get nodes
```

The node should be in `Ready` state.

---

## 2. Verify the ToDo application and ClusterIP Service

```bash
kubectl get pods -n todoapp
kubectl get svc -n todoapp
kubectl get endpoints todoapp-service -n todoapp
```

The expected ClusterIP Service name is:

```text
todoapp-service
```

The endpoints should contain at least one application Pod IP and port `8080`.

---

## 3. Create the `mateapp` namespace

```bash
kubectl create namespace mateapp --dry-run=client -o yaml | kubectl apply -f -
```

Verify:

```bash
kubectl get namespace mateapp
```

---

## 4. Validate manifests before deployment

```bash
kubectl apply --dry-run=client -f .infrastructure/daemonset.yml
kubectl apply --dry-run=client -f .infrastructure/cronjob.yml
```

Both commands should complete without validation errors.

---

## 5. Deploy the DaemonSet

```bash
kubectl apply -f .infrastructure/daemonset.yml
```

Check it:

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -o wide
```

A DaemonSet creates one Pod on each eligible node.  
With a single-node Docker Desktop cluster, normally one DaemonSet Pod should run.

---

## 6. Validate the DaemonSet

Find the DaemonSet Pod:

```bash
kubectl get pods -n mateapp -l app=todoapp-daemon
```

Follow logs:

```bash
kubectl logs -n mateapp -l app=todoapp-daemon -f
```

The logs should show repeated requests approximately every 5 seconds to:

```text
http://todoapp-service.todoapp.svc.cluster.local/
```

Stop log streaming with:

```text
Ctrl+C
```

Inspect the DaemonSet:

```bash
kubectl describe daemonset todoapp-daemon -n mateapp
```

---

## 7. Deploy the CronJob

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

Check it:

```bash
kubectl get cronjob -n mateapp
```

Expected schedule:

```text
*/4 * * * *
```

This means the CronJob runs every 4 minutes.

---

## 8. Validate the CronJob configuration

```bash
kubectl describe cronjob todoapp-health-check -n mateapp
```

Verify:

```text
Concurrency Policy: Allow
Successful Job History Limit: 10
Failed Job History Limit: 5
```

The command should call:

```text
http://todoapp-service.todoapp.svc.cluster.local/api/health
```

---

## 9. Validate CronJob execution and logs

Wait for a scheduled run and list Jobs:

```bash
kubectl get jobs -n mateapp
```

List Pods created by Jobs:

```bash
kubectl get pods -n mateapp
```

Show Jobs ordered by creation time:

```bash
kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp
```

Read logs from a Job:

```bash
kubectl logs -n mateapp job/<JOB_NAME>
```

A successful run should show a response from the `/api/health` endpoint.

---

## 10. Test the CronJob immediately

Create a one-time Job from the CronJob:

```bash
kubectl create job \
  --from=cronjob/todoapp-health-check \
  todoapp-health-check-manual \
  -n mateapp
```

Check it:

```bash
kubectl get jobs -n mateapp
```

Read logs:

```bash
kubectl logs -n mateapp job/todoapp-health-check-manual
```

Delete the manual test Job:

```bash
kubectl delete job todoapp-health-check-manual -n mateapp
```

---

## 11. Verify resource requests and limits

Inspect the DaemonSet:

```bash
kubectl get daemonset todoapp-daemon -n mateapp -o yaml
```

Inspect the CronJob:

```bash
kubectl get cronjob todoapp-health-check -n mateapp -o yaml
```

Both containers should contain CPU and memory `requests` and `limits`.

Example:

```yaml
resources:
  requests:
    cpu: "10m"
    memory: "16Mi"
  limits:
    cpu: "50m"
    memory: "32Mi"
```

These values are suitable for lightweight curl-based health checks.

---

## 12. Useful troubleshooting commands

```bash
kubectl get all -n mateapp
kubectl describe daemonset todoapp-daemon -n mateapp
kubectl describe cronjob todoapp-health-check -n mateapp
```

Test Service DNS manually from a temporary curl Pod:

```bash
kubectl run curl-test \
  --rm -it \
  --restart=Never \
  -n mateapp \
  --image=ikulyk404/busyboxplus:curl \
  -- sh
```

Inside:

```bash
curl http://todoapp-service.todoapp.svc.cluster.local/
curl http://todoapp-service.todoapp.svc.cluster.local/api/health
```

Exit:

```bash
exit
```

---

## 13. Remove the task resources

```bash
kubectl delete -f .infrastructure/cronjob.yml
kubectl delete -f .infrastructure/daemonset.yml
```

If `mateapp` is used only for this task:

```bash
kubectl delete namespace mateapp
```

Do not delete the `todoapp` namespace if the existing application is still needed.

---

## 14. Pull Request

Check changed files:

```bash
git status
```

Add only the task files:

```bash
git add .infrastructure/daemonset.yml
git add .infrastructure/cronjob.yml
git add INSTRUCTION.md
```

Commit:

```bash
git commit -m "Add Kubernetes DaemonSet and CronJob"
```

Push:

```bash
git push
```

If the branch has no upstream:

```bash
git push -u origin <YOUR_BRANCH_NAME>
```

Suggested PR title:

```text
Add Kubernetes DaemonSet and CronJob
```

Suggested PR description:

```markdown
## Changes

- Added DaemonSet using `busyboxplus:curl`
- Added CPU and memory requests/limits
- Added a curl request to the ToDo ClusterIP Service every 5 seconds
- Added CronJob that runs every 4 minutes
- Added `/api/health` validation through ClusterIP DNS
- Configured `concurrencyPolicy: Allow`
- Configured 10 successful and 5 failed Job history limits
- Added deployment and validation instructions
```

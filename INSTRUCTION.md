# TodoApp Kubernetes Health Checks

This document describes how to deploy and validate the DaemonSet and CronJob used to monitor the TodoApp health endpoint.

## Prerequisites

Before deploying the manifests, make sure that:

- A Kubernetes cluster is running.
- The `todoapp` namespace exists.
- The TodoApp Deployment is running in the `todoapp` namespace.
- The `todoapp-service` ClusterIP Service exists in the `todoapp` namespace.
- The `todoapp-service` exposes port `80` and forwards traffic to the TodoApp container on port `8080`.
- The `mateapp` namespace exists.

Check the existing TodoApp resources:

```bash
kubectl get deployment -n todoapp
kubectl get service -n todoapp
kubectl get pods -n todoapp
```

If the `mateapp` namespace does not exist, create it:

```bash
kubectl create namespace mateapp
```

---

## Deploy the DaemonSet

Apply the DaemonSet manifest:

```bash
kubectl apply -f daemonset.yml
```

Verify that the DaemonSet was created:

```bash
kubectl get daemonset -n mateapp
```

Check the DaemonSet pods:

```bash
kubectl get pods -n mateapp -l app=todoapp-daemonset
```

The DaemonSet creates one pod on each available Kubernetes node.

The container periodically sends an HTTP request to the TodoApp health endpoint every 5 seconds:

```text
http://todoapp-service.todoapp.svc.cluster.local:80/api/health
```

The request is sent through the TodoApp ClusterIP Service in the `todoapp` namespace.

---

## Deploy the CronJob

Apply the CronJob manifest:

```bash
kubectl apply -f cronjob.yml
```

Verify that the CronJob was created:

```bash
kubectl get cronjob -n mateapp
```

The CronJob runs every 4 minutes and sends a request to:

```text
/api/health
```

through the TodoApp ClusterIP Service.

Check the Jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

Check the pods created by the Jobs:

```bash
kubectl get pods -n mateapp
```

---

## Validate the DaemonSet

First, get the DaemonSet pod name:

```bash
kubectl get pods -n mateapp -l app=todoapp-daemonset
```

Then check its logs:

```bash
kubectl logs -n mateapp <daemonset-pod-name>
```

The logs should contain health-check results similar to:

```text
Mon Nov 3 14:00:00 UTC 2026 - Checking TodoApp...
HTTP Status: 200
Mon Nov 3 14:00:05 UTC 2026 - Checking TodoApp...
HTTP Status: 200
Mon Nov 3 14:00:10 UTC 2026 - Checking TodoApp...
HTTP Status: 200
```

To monitor the logs continuously:

```bash
kubectl logs -f -n mateapp <daemonset-pod-name>
```

A successful request should return HTTP status `200`.

---

## Validate the CronJob

Check the CronJob status:

```bash
kubectl get cronjob -n mateapp
```

Check the Jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

After a Job has been created, find its pod:

```bash
kubectl get pods -n mateapp
```

Then check the pod logs:

```bash
kubectl logs -n mateapp <cronjob-pod-name>
```

Expected output:

```text
Mon Nov 3 14:04:00 UTC 2026 - Checking TodoApp health...
HTTP Status: 200
```

Because the schedule is:

```text
*/4 * * * *
```

a new Job should be created every 4 minutes.

---

## Verify Job History

The CronJob is configured to retain:

- up to 10 successful Jobs;
- up to 5 failed Jobs.

Check the current Jobs:

```bash
kubectl get jobs -n mateapp
```

The relevant CronJob configuration is:

```yaml
successfulJobsHistoryLimit: 10
failedJobsHistoryLimit: 5
concurrencyPolicy: Allow
```

The `Allow` concurrency policy permits a new Job to start even if a previous Job is still running.

---

## Verify the TodoApp Service

Check the Service:

```bash
kubectl get service todoapp-service -n todoapp
```

Inspect its configuration:

```bash
kubectl describe service todoapp-service -n todoapp
```

The Service should be a `ClusterIP` and expose port `80` with a target port of `8080`.

The communication path is:

```text
DaemonSet/CronJob
      |
      | HTTP :80
      v
todoapp-service
(namespace: todoapp)
      |
      | targetPort: 8080
      v
TodoApp Pod
      |
      | /api/health
      v
HTTP 200
```

---

## Troubleshooting

### DaemonSet pods are not running

Check the DaemonSet:

```bash
kubectl describe daemonset todoapp-daemonset -n mateapp
```

Check the pod:

```bash
kubectl describe pod <daemonset-pod-name> -n mateapp
```

Check pod events:

```bash
kubectl get events -n mateapp --sort-by=.lastTimestamp
```

### CronJob is not creating Jobs

Check the CronJob:

```bash
kubectl describe cronjob todoapp-health-check -n mateapp
```

Check existing Jobs:

```bash
kubectl get jobs -n mateapp
```

Check namespace events:

```bash
kubectl get events -n mateapp --sort-by=.lastTimestamp
```

### TodoApp health check fails

Verify that the TodoApp is running:

```bash
kubectl get pods -n todoapp
```

Verify the Service:

```bash
kubectl get service todoapp-service -n todoapp
```

Verify that the Service has endpoints:

```bash
kubectl get endpoints todoapp-service -n todoapp
```

The Service should have at least one endpoint pointing to a TodoApp pod on port `8080`.

---

## Cleanup

To remove the DaemonSet:

```bash
kubectl delete -f daemonset.yml
```

To remove the CronJob:

```bash
kubectl delete -f cronjob.yml
```

Verify that the resources have been removed:

```bash
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
```
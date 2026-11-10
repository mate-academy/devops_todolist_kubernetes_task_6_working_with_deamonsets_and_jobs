# Instructions

This repository contains two manifests:

- `daemonset.yml` runs a Pod on every node that sends a `curl` request to the todoapp service every 5 seconds.
- `cronjob.yml` calls the `/api/health` endpoint of the todoapp service every 4 minutes.

Both resources belong to the `mateapp` namespace.

## Prerequisites

- A running Kubernetes cluster and configured `kubectl`.
- The `mateapp` namespace.
- The todoapp deployed with a ClusterIP service named `todoapp-service` on port 80 in the `mateapp` namespace.

Create the namespace if it does not exist yet:

```bash
kubectl create namespace mateapp
```

## Deploy

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## Validate

### DaemonSet

Check that the DaemonSet and its Pods are running (one Pod per node):

```bash
kubectl get daemonset -n mateapp
kubectl get pods -n mateapp -l app=busyboxapp
```

Check the logs. A new `200` line should appear every 5 seconds:

```bash
kubectl logs -n mateapp -l app=busyboxapp --tail=20
```

### CronJob

Check that the CronJob was created:

```bash
kubectl get cronjob -n mateapp
```

The first Job appears within 4 minutes. List the Jobs and Pods:

```bash
kubectl get jobs -n mateapp
kubectl get pods -n mateapp
```

Check the logs of a CronJob Pod (use the real Pod name from the list above):

```bash
kubectl logs -n mateapp <cronjob-pod-name>
```

To avoid waiting 4 minutes, trigger a Job manually:

```bash
kubectl create job --from=cronjob/todoapp-cronjob manual-test -n mateapp
kubectl logs -n mateapp job/manual-test
```

The logs should contain the response from `/api/health`.

## Cleanup

```bash
kubectl delete -f daemonset.yml
kubectl delete -f cronjob.yml
```
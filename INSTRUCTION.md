# ToDo App — DaemonSet & CronJob Instructions

## Prerequisites

- A running Kubernetes cluster with `mateapp` namespace and the ToDo app
  (Deployment + ClusterIP service) already deployed
- `kubectl` installed and configured

## 1. Deploy the manifests

```bash
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## 2. Validate the DaemonSet

Check that a DaemonSet pod is running on each node:

```bash
kubectl get pods -n mateapp -l app=mate-daemon
```

View its logs to confirm it's curling the app every 5 seconds:

```bash
kubectl logs -n mateapp -l app=mate-daemon
```

## 3. Validate the CronJob

Check the CronJob and its job history:

```bash
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
```

Get the logs of the latest job run:

```bash
kubectl logs -n mateapp job/<job-name>
```

(Replace `<job-name>` with the name from `kubectl get jobs -n mateapp`.)

## Cleaning up

```bash
kubectl delete -f .infrastructure/cronjob.yml
kubectl delete -f .infrastructure/daemonset.yml
```
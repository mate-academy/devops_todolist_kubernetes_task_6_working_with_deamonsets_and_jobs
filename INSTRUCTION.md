# Instructions

## Prerequisites

Make sure that Kubernetes is running and `kubectl` is configured.

Check the cluster:

```
kubectl get nodes
```

Make sure the `mateapp` namespace exists:

```
kubectl get namespace mateapp
```

If the namespace does not exist:

```bash
kubectl create namespace mateapp
``

## Deploy DaemonSet

Deploy the DaemonSet:

```
kubectl apply -f .infrastructure/daemonset.yml
```

Check the DaemonSet:

```
kubectl get daemonset -n mateapp
```

Check the DaemonSet pods:

```
kubectl get pods -n mateapp -l app=todo-daemonset
```

## Deploy CronJob

Deploy the CronJob:

```
kubectl apply -f .infrastructure/cronjob.yml
```

Check the CronJob:

```
kubectl get cronjob -n mateapp
```

Check the Jobs created by the CronJob:

```
kubectl get jobs -n mateapp
```

## Validate the DaemonSet

The DaemonSet should execute a `curl` request to the `todoapp` ClusterIP Service every 5 seconds.

Check the DaemonSet logs:

```
kubectl logs -n mateapp -l app=todo-daemonset
```

To follow the logs continuously:

```
kubectl logs -n mateapp -l app=todo-daemonset -f
```

The logs should contain repeated `curl` requests approximately every 5 seconds.

## Validate the CronJob

The CronJob runs every 4 minutes and calls the `/api/health` endpoint through the `todoapp` ClusterIP Service.

Check the CronJob:

```
kubectl get cronjob -n mateapp
```

Check the Jobs:

```
kubectl get jobs -n mateapp
```

Get the CronJob pod:

```
kubectl get pods -n mateapp
```

Check the logs of the CronJob pod:

```
kubectl logs -n mateapp <cronjob-pod-name>
```

The logs should show a request to:

```
http://todoapp/api/health
```

## Validate the configuration

Check the DaemonSet configuration:

```
kubectl describe daemonset todo-daemonset -n mateapp
```

Check the CronJob configuration:

```
kubectl describe cronjob todo-cronjob -n mateapp
```

The CronJob must have:

* schedule: `*/4 * * * *`
* `concurrencyPolicy: Allow`
* `successfulJobsHistoryLimit: 10`
* `failedJobsHistoryLimit: 5`

Both resources must be deployed in the `mateapp` namespace.

## Validate all resources

```
kubectl get all -n mateapp
```

The DaemonSet and CronJob logs must be checked to confirm that both workloads are running as expected.

## Cleanup

Remove the DaemonSet:

```
kubectl delete -f .infrastructure/daemonset.yml
```

Remove the CronJob:

```
kubectl delete -f .infrastructure/cronjob.yml
```

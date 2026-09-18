# Kubernetes Deployment Instructions

## Prerequisites

Make sure that:

* Kubernetes cluster is running.
* `kubectl` is installed and configured.
* The `mateapp` namespace exists.
* The `todoapp-service` Service is deployed in the `mateapp` namespace.

Check the namespace:

```bash
kubectl get namespace mateapp
```

Check the TodoApp Service:

```bash
kubectl get service -n mateapp
```

The DaemonSet and CronJob use the following ClusterIP Service:

```text
todoapp-service.mateapp.svc.cluster.local
```

---

## Deploy DaemonSet

Apply the DaemonSet manifest:

```bash
kubectl apply -f daemonset.yml
```

Check the DaemonSet:

```bash
kubectl get daemonset -n mateapp
```

Check the Pods created by the DaemonSet:

```bash
kubectl get pods -n mateapp -l app=busybox_label
```

The DaemonSet container executes a `curl` request to the TodoApp Service every 5 seconds.

### Validate DaemonSet logs

First, get the Pod name:

```bash
kubectl get pods -n mateapp -l app=busybox_label
```

Then check its logs:

```bash
kubectl logs -n mateapp <pod-name>
```

The logs should contain successful responses from the TodoApp Service.

To continuously monitor the logs:

```bash
kubectl logs -f -n mateapp <pod-name>
```

---

## Deploy CronJob

Apply the CronJob manifest:

```bash
kubectl apply -f cronjob.yml
```

Check the CronJob:

```bash
kubectl get cronjob -n mateapp
```

The CronJob runs every 4 minutes and sends a request to:

```text
/api/health
```

through the TodoApp ClusterIP Service.

---

## Validate CronJob

Check the Jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

After a scheduled run has completed, check the Pods:

```bash
kubectl get pods -n mateapp
```

Check the logs of a CronJob Pod:

```bash
kubectl logs -n mateapp <cronjob-pod-name>
```

The logs should contain the response from the `/api/health` endpoint.

You can also check the most recent Job and its Pods:

```bash
kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp
```

```bash
kubectl get pods -n mateapp --sort-by=.metadata.creationTimestamp
```

---

## Validate CronJob configuration

Check the CronJob configuration:

```bash
kubectl get cronjob crobjobname -n mateapp -o yaml
```

Verify that:

* `schedule` is `*/4 * * * *`
* `concurrencyPolicy` is `Allow`
* `successfulJobsHistoryLimit` is `10`
* `failedJobsHistoryLimit` is `5`
* the container image is `busyboxplus:curl`
* resource requests and limits are defined

---

## Validate DaemonSet configuration

Check the DaemonSet configuration:

```bash
kubectl get daemonset deamonsetname -n mateapp -o yaml
```

Verify that:

* the DaemonSet belongs to the `mateapp` namespace
* the container image is `busyboxplus:curl`
* resource requests and limits are defined
* the container executes `curl` every 5 seconds
* the request is sent to the TodoApp ClusterIP Service

---

## Check all resources

To see all relevant resources in the namespace:

```bash
kubectl get all -n mateapp
```

For a more detailed view:

```bash
kubectl get daemonset,cronjob,jobs,pods -n mateapp
```

---

## Cleanup

To remove the DaemonSet:

```bash
kubectl delete -f daemonset.yml
```

To remove the CronJob and its associated Jobs:

```bash
kubectl delete -f cronjob.yml
```

# Deployment Instructions

## Prerequisites

- A running Kubernetes cluster
- `kubectl` configured to point at the target cluster

## 1. Create the namespace

```
kubectl apply -f .infrastructure/namespace.yml
```

## 2. Deploy the ToDo app and its service

The DaemonSet and CronJob rely on the `todoapp-service` ClusterIP service being reachable, so deploy the app first:

```
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
```

## 3. Deploy the DaemonSet

```
kubectl apply -f .infrastructure/daemonset.yml
```

## 4. Deploy the CronJob

```
kubectl apply -f .infrastructure/cronjob.yml
```

## Validation

### Check that resources were created

```
kubectl get all -n mateapp
```

You should see the `todoapp` Deployment, `todoapp-service` Service, `todoapp` DaemonSet (one pod per node), and the `hello-cronjob` CronJob.

### Validate the DaemonSet

List the DaemonSet pods:

```
kubectl get pods -n mateapp -l app=mate-server
```

Check the logs of a DaemonSet pod — it should show a successful `curl` response every 5 seconds:

```
kubectl logs -n mateapp <daemonset-pod-name>
```

### Validate the CronJob

List the Jobs and Pods created by the CronJob:

```
kubectl get jobs -n mateapp
kubectl get pods -n mateapp -l job-name
```

Check the logs of a Job pod — it should show the response of the `curl` call to `/api/health`:

```
kubectl logs -n mateapp <cronjob-pod-name>
```

Wait a few scheduling cycles (schedule is `*/4 * * * *`) and confirm:

```
kubectl get cronjob -n mateapp hello-cronjob
```

- `successfulJobsHistoryLimit` keeps the last 10 successful Jobs
- `failedJobsHistoryLimit` keeps the last 5 failed Jobs

# ToDo App Kubernetes Resources Deployment and Validation Guide

This README provides instructions for deploying and validating Kubernetes resources for the ToDo app. These resources include a DaemonSet and a CronJob that interact with the ToDo app service in a different namespace.

## Deployment Instructions

### Deploying the DaemonSet

The DaemonSet ensures a curl command is executed periodically to the ToDo app's service.

1. **Deploy the DaemonSet:**

```bash
   kubectl apply -f daemonset.yml
```

### Deploying the CronJob

The CronJob periodically calls the /api/health endpoint of the ToDo app to check its health.

```bash
kubectl apply -f cronjob.yml
```

## Validation Instructions

### Validating the DaemonSet

To confirm the DaemonSet is running as expected:

Check the DaemonSet's status:

```bash
kubectl -n mateapp get daemonset
```

### Validating the CronJob

To ensure the CronJob is functioning correctly:

Check the CronJob's status:

```bash
kubectl -n mateapp get cronjob
```

## Checking Logs

To validate the actions performed by the DaemonSet and CronJob:

1. Find the Pod Names:

For the DaemonSet:

```bash
kubectl -n mateapp get pods -l app=todoapp-curl
```

For the CronJob:

```bash
kubectl -n mateapp get pods -l job-name=todoapp-health-check
```

2. View the Logs:

```bash
kubectl -n mateapp logs <pod-name>
```

Replace <pod-name> with the actual name of the pod you wish to inspect.

By following these instructions, you can deploy and validate the DaemonSet and CronJob for the ToDo app in your Kubernetes environment.

# Instructions for deploying DaemonSet and CronJob

## 1. Deploy DaemonSet

Apply the DaemonSet manifest to your Kubernetes cluster:

```bash
kubectl apply -f .infrastructure/daemonset.yml
```

## 2. Deploy CronJob

Apply the CronJob manifest to your Kubernetes cluster:

```bash
kubectl apply -f .infrastructure/cronjob.yml
```

## 3. Validate the Solution

### Check DaemonSet Logs

List the pods created by the DaemonSet:

```bash
kubectl get pods -n mateapp -l app=request-todoapp
```

Check logs for one of the DaemonSet pods:

```bash
kubectl logs <daemonset-pod-name> -n mateapp
```

### Check CronJob logs

List the jobs created by the CronJob:

```bash
kubectl get jobs -n mateapp
```

List the pods created by the CronJob:

```bash
kubectl get pods -n mateapp -l job-name=request-todoapp-health
```

Check logs for one of the CronJob pods:

```bash
kubectl logs <cronjob-pod-name> -n mateapp
```

Replace `<daemonset-pod-name>` and `<cronjob-pod-name>` with the actual pod names from the output of the previous commands.

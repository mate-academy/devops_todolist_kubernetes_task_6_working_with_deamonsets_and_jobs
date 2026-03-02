# Deployment and validation guide

## 1. Prerequisites

- Kubernetes cluster is running
- `kubectl` is configured to work with your cluster
- Namespace `mateapp` exists
- A ClusterIP service named `todoapp-service` is available in namespace `mateapp`

If namespace does not exist, create it:

```bash
kubectl create namespace mateapp
```

## 2. Deploy manifests

Run from the project root:

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## 3. Validate DaemonSet

Check DaemonSet status:

```bash
kubectl get daemonset -n mateapp
kubectl describe daemonset todoapp-curl-daemon -n mateapp
```

Check pods created by DaemonSet:

```bash
kubectl get pods -n mateapp -l app=todoapp-curl-daemon
```

Check DaemonSet logs (continuous curl every 5 seconds):

```bash
kubectl logs -n mateapp -l app=todoapp-curl-daemon --tail=50
```

## 4. Validate CronJob

Check CronJob status:

```bash
kubectl get cronjob todoapp-healthcheck -n mateapp
kubectl describe cronjob todoapp-healthcheck -n mateapp
```

Wait at least 4 minutes, then check jobs:

```bash
kubectl get jobs -n mateapp
```

Check logs of a CronJob pod:

```bash
JOB_NAME=$(kubectl get jobs -n mateapp --sort-by=.metadata.creationTimestamp -o jsonpath='{.items[-1:].metadata.name}')
kubectl logs -n mateapp job/$JOB_NAME
```

## 5. Optional quick manual test of CronJob

Create a one-time job from CronJob template:

```bash
kubectl create job --from=cronjob/todoapp-healthcheck manual-healthcheck -n mateapp
kubectl logs -n mateapp job/manual-healthcheck
```

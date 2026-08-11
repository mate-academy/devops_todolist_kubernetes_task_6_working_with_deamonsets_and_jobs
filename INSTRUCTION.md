# Kubernetes Deployment Instructions

## Deploy DaemonSet

Apply the DaemonSet manifest:

```bash
kubectl apply -f daemonset.yml
```

Check the DaemonSet:
```bash
kubectl get daemonset -n mateapp
```

Check its pods:
```bash
kubectl get pods -n mateapp -l app=todoapp-daemonset
```

Check DaemonSet logs:
```bash
kubectl logs -n mateapp -l app=todoapp-daemonset
```

## Deploy CronJob

Apply the CronJob manifest:
```bash
kubectl apply -f cronjob.yml
```

Check the CronJob:
```bash
kubectl get cronjob -n mateapp
```

Check created Jobs:
```bash
kubectl get jobs -n mateapp
```

Check Pods created by the CronJob:
```bash
kubectl get pods -n mateapp
```

check logs of a CronJob pod:
```bash
kubectl logs -n mateapp <pod-name>
```
The logs should contain the result of the curl request to: http://todoapp/api/health

## Validation

Verify that the DaemonSet is running:
```bash
kubectl get daemonset -n mateapp
```

Verify that DaemonSet pods are running:
```bash
kubectl get pods -n mateapp -l app=todoapp-daemonset
```

Verify DaemonSet logs:
```bash
kubectl logs -n mateapp -l app=todoapp-daemonset
```

Verify the CronJob configuration:
```bash
kubectl get cronjob todoapp -n mateapp
```

Verify that Jobs are being created:
```bash
kubectl get jobs -n mateapp
```

Verify CronJob logs:
```bash
kubectl get pods -n mateapp
kubectl logs -n mateapp <cronjob-pod-name>
```
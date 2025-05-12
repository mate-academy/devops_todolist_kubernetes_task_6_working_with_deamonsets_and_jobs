# Deployment Instructions


## Deployment Steps

1. Create the mateapp namespace if it doesn't exist:
```bash
kubectl create namespace mateapp
```

2. Deploy the DaemonSet:
```bash
kubectl apply -f .infrastructure/daemonset.yml
```

3. Deploy the CronJob:
```bash
kubectl apply -f .infrastructure/cronjob.yml
```

## Validation Steps

1. Verify DaemonSet deployment:
```bash
kubectl get daemonset -n mateapp
```

2. Check DaemonSet pods:
```bash
kubectl get pods -n mateapp -l app=todoapp-monitor
```

3. View DaemonSet pod logs:
```bash
kubectl logs -n mateapp -l app=todoapp-monitor
```

4. Verify CronJob deployment:
```bash
kubectl get cronjob -n mateapp
```

5. Check CronJob jobs:
```bash
kubectl get jobs -n mateapp
```

6. View CronJob pod logs:
```bash
kubectl logs -n mateapp -l job-name=todoapp-health-check-<job-id>
```
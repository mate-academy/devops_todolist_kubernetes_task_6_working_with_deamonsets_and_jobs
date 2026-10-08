# Instructions for Deploying and Testing DaemonSet and CronJob

## 1. How to Apply Manifests
```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## 2. How to Validate the Solution
```bash
# Validate DaemonSet logs
kubectl logs -l name=todoapp-pinger -n mateapp --tail=20

# Validate CronJob execution
kubectl get jobs,pods -n mateapp
```

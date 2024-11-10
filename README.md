# Instructions for Deploying DaemonSet & CronJob

## Deploy to Kubernetes Cluster

```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## Verify the Deployment

```bash
kubectl get daemonset -n todoapp
kubectl get cronjob -n todoapp
```

## Retrieve Logs for DaemonSet and CronJob

```bash 
kubectl get pods -n todoapp
kubectl logs <name_of_pod> -n todoapp
```

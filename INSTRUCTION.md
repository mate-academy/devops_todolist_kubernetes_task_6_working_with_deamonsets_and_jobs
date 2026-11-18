## Deploy the DaemonSet and CronJob

Run from the project folder:

```bash
kubectl apply -f .infrastructure/namespace.yml
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml
kubectl apply -f .infrastructure/hpa.yml
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

Check that both resources were created:

```bash
kubectl get daemonset,cronjob -n mateapp
```

## Validate the solution

### DaemonSet

1. List the pods and find the one whose name starts with `todoapp-bbx`:

   ```bash
   kubectl get pods -n mateapp
   ```

2. Read its logs (replace `<todoapp-bbx-name>` with the full pod name):

   ```bash
   kubectl logs <todoapp-bbx-name> -n mateapp --tail=10
   ```

   > Expected output: HTML tags.

### CronJob

1. List the pods and find the one whose name starts with `health-cronjob-` (status `Completed`):

   ```bash
   kubectl get pods -n mateapp
   ```

2. Read its logs (replace `<health-cronjob-pod-name>` with the full pod name):

   ```bash
   kubectl logs <health-cronjob-pod-name> -n mateapp
   ```

   > Expected output: `Health OK`
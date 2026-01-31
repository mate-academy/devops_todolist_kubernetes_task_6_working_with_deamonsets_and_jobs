# How to Deploy and Validate Todo App with DaemonSet and CronJob

## 1. Deploy Kubernetes Resources

From the repository root, execute the following commands:

```bash
# Create namespace
kubectl apply -f .infrastructure/namespace.yml

# Create ClusterIP service and NodePort (for manual checks in browser)
kubectl apply -f .infrastructure/clusterIp.yml
kubectl apply -f .infrastructure/nodeport.yml

# Create Deployment with the Todo app and HPA
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/hpa.yml

# Deploy DaemonSet and CronJob
kubectl apply -f .infrastructure/daemonset.yml
kubectl apply -f .infrastructure/cronjob.yml
```

## 2. Verify Resources

Check that everything is running in the `mateapp` namespace:

```bash
kubectl get pods -n mateapp
kubectl get deploy -n mateapp
kubectl get svc -n mateapp
kubectl get hpa -n mateapp
kubectl get daemonset -n mateapp
kubectl get cronjob -n mateapp
```

### DaemonSet Verification

List DaemonSet pods:

```bash
kubectl get pods -n mateapp -l app=todoapp-daemon
```

Watch logs from all DaemonSet pods:

```bash
kubectl logs -n mateapp -l app=todoapp-daemon -f
```

You should see repeated messages confirming that the DaemonSet is calling the Todo app through the ClusterIP service.

### CronJob Verification

Check CronJob and created Jobs:

```bash
kubectl get cronjob -n mateapp
kubectl get jobs -n mateapp
kubectl get pods -n mateapp -l job=todoapp-healthcheck
```

View logs for a specific job (replace `<job-name>` with the actual job name):

```bash
kubectl logs job/<job-name> -n mateapp
```

## 3. Get Node External IP

To access the application externally, retrieve the node's external IP:

```bash
kubectl get nodes -o wide
```
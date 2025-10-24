## Apply Manifests

```bash
# Create the namespace
kubectl apply -f .infrastructure/namespace.yml

# Create the ClusterIP service
kubectl apply -f .infrastructure/clusterIp.yml

# Create the Deployment for todoapp application
kubectl apply -f .infrastructure/deployment.yml

# Create the DaemonSet controller
kubectl apply -f .infrastructure/daemonset.yml

# Create the CronJob controller
kubectl apply -f .infrastructure/cronjob.yml
```

After applying, you can check the status of the pods in the mateapp namespace:

```bash
kubectl get pods -n mateapp -o wide
```

## Check the DaemonSet controller

The DaemonSet continuously prints logs showing the curl command output.

To view the logs of one of the DaemonSet pods:

```bash
# List all pods managed by the DaemonSet
kubectl get pods -n mateapp -l app=busyboxplus

# Example output:
# NAME                      READY   STATUS    RESTARTS   AGE
# busyboxplus-abc12         1/1     Running   0          2m

# View logs from one pod
kubectl logs busyboxplus-abc12 -n mateapp

```
You should see repeating log lines like:

```text
--------------------------------------------------
Pinging todoapp service...
Health OK
--------------------------------------------------
```

or

```text
--------------------------------------------------
Pinging todoapp service...
Service unavailable
--------------------------------------------------
```

if the service is down.

## Check the CronJob controller

The CronJob runs every 4 minutes and creates short-lived Job pods.

To verify execution:

```bash
# List CronJobs
kubectl get cronjobs -n mateapp

# List Jobs created by the CronJob
kubectl get jobs -n mateapp

# List pods created by jobs
kubectl get pods -n mateapp
```

To check logs from the latest job pod:

```bash
# Replace <pod-name> with the actual name from the list
kubectl logs <pod-name> -n mateapp
```

You should see output like:

```text
Health OK
```

## Cleanup

```bash
kubectl delete -f .infrastructure/daemonset.yml
kubectl delete -f .infrastructure/cronjob.yml
kubectl delete -f .infrastructure/deployment.yml
kubectl delete -f .infrastructure/clusterIp.yml
kubectl delete -f .infrastructure/namespace.yml
```
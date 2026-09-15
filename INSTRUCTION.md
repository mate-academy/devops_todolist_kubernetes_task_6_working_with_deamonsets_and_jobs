### How to deploy the app to k8s

```bash
kubectl apply -f .infrastructure/namespace.yml 
kubectl apply -f .infrastructure/deployment.yml
kubectl apply -f .infrastructure/clusterIp.yml 
kubectl apply -f .infrastructure/daemonset.yml 
kubectl apply -f .infrastructure/cronjob.yml 
```

### Validate CronJob Execution & Logs

#### Check CronJob pod name:

Check CronJob status:

```bash
kubectl get cronjob -n mateapp
```
Retrieve the latest Job pod created by the CronJob:

```bash
CRON_POD=$(kubectl get pods -n mateapp -l job-name --sort-by='.metadata.creationTimestamp' | tail -n 1 | awk '{print $1}')
echo "Latest CronJob Pod: $CRON_POD"
```
Inspect CronJob execution logs:

```bash
kubectl logs $CRON_POD -n mateapp
```
Expected Output: Successful HTTP response headers/status (e.g., HTTP/1.1 200 OK or response body {"status":"ok"}) indicating completed curl requests.

### Validate DaemonSet Execution & Logs



Verify DaemonSet status:

```bash
kubectl get daemonset -n mateapp
```
Check Daemonset pods name:
```bash
kubectl get pods -n mateapp
```

Check logs:
```bash
kubectl logs <pod-name> -n mateapp
```
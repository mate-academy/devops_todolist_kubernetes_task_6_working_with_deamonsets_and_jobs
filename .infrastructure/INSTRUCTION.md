# Deployment and Validation Guide

## Deployment
To deploy the manifests to your Kubernetes cluster, execute the following commands from your terminal:
```
# Deploy the CronJob
kubectl apply -f cronjob.yml

# Deploy the DaemonSet
kubectl apply -f daemonset.yml
```
## Validation
### 1. Verify Resources
Check if the CronJob and DaemonSet are successfully created and running:

```
# Check CronJob status
kubectl get cronjob -n mateapp

# Check DaemonSet status (should show pods for each node)
kubectl get ds -n mateapp

# List all pods in the namespace
kubectl get pods -n mateapp
```
### 2. Validate CronJob Execution and Logs
Since the CronJob runs every 4 minutes, you can wait for a scheduled run or trigger one manually for testing:
```
# Manual trigger (optional)
kubectl create job --from=cronjob/health-status-cronjob manual-test-check -n mateapp

# View logs of the last CronJob pod
# Replace <pod-name> with the actual pod name from 'kubectl get pods'
kubectl logs <cronjob-pod-name> -n mateapp
```
### 3. Validate DaemonSet Logs
To verify that the monitoring tool is calling the service every 5 seconds, check the logs of any DaemonSet pod:

```
# Get logs from a DaemonSet pod using the label selector
kubectl logs -l app=todoapp-monitoring -n mateapp --tail=20
```
### 4. Check Service Connectivity
To ensure the `curl` commands are reaching the `todoapp` service, inspect the log output for the "Success" message or the HTTP response defined in the script.
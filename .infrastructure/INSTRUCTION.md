## Deployment
To deploy the manifests to your Kubernetes cluster, execute the following commands from your terminal:
```bash
# Create the namespace (if not already present)
kubectl apply -f - <<EOF
apiVersion: v1
kind: Namespace
metadata:
  name: mateapp
EOF

# Deploy the CronJob
kubectl apply -f .infrastructure/cronjob.yml

# Deploy the DaemonSet
kubectl apply -f .infrastructure/daemonset.yml
```

-----

## Validation

### 1. Verify Resources

Check if the CronJob and DaemonSet are successfully created and running:

```bash
# Check CronJob status
kubectl get cronjob -n mateapp

# Check DaemonSet status (should show pods for each node)
kubectl get ds -n mateapp

# List all pods in the namespace
kubectl get pods -n mateapp
```

### 2. Validate CronJob Execution and Logs

Since the CronJob runs every 4 minutes, you can monitor its execution using these steps:

1.  **Find the Job name** (created by the CronJob):
    ```bash
    kubectl get jobs -n mateapp
    ```
2.  **Find the Pod name** associated with that Job:
    ```bash
    # Replace <job-name> with the name from the previous step
    kubectl get pods -n mateapp --selector=job-name=<job-name>
    ```
3.  **Inspect the logs**:
    ```bash
    # Replace <pod-name> with the actual pod name
    kubectl logs <pod-name> -n mateapp
    ```

### 3. Validate DaemonSet Logs

To verify that the monitoring tool is calling the service every 5 seconds, check the logs of the DaemonSet pods:

```bash
# Get logs from DaemonSet pods using the label selector
kubectl logs -l app=todoapp-monitoring -n mateapp --tail=20
```

### 4\. Pull Request for Validation

The formal submission of this infrastructure change is tracked via the following Pull Request:

**PR link:** [https://github.com/](https://github.com/mate-academy/devops_todolist_kubernetes_task_6_working_with_deamonsets_and_jobs/pull/137)

# Deployment and Validation Instructions

This guide provides the necessary steps to deploy and verify the connectivity monitoring tools (DaemonSet and CronJob) within the Kubernetes cluster.

---

## 🚀 Deployment

To deploy the resources, apply the manifest files located in the `.infrastructure` directory:

1. **Deploy the DaemonSet:**
```bash
   kubectl apply -f .infrastructure/daemonset.yml
```
2. **Deploy the CronJob:**   
```bash
   kubectl apply -f .infrastructure/cronjob.yml
```


## 🚀 Validation

1. **Validate DaemonSet:**
The DaemonSet ensures a monitoring pod runs on every node.

Check Pod Status:
Verify that the pods are in the Running state:
   
```Bash
    kubectl get pods -l app=busybox-daemon -n mateapp
```
Check Connectivity Logs:
To see the continuous health checks (sent every 5 seconds), view the logs:
```Bash
    kubectl logs -l app=busybox-daemon -n mateapp --tail=20
```

2. **Validate CronJob:**

The CronJob triggers a health check every 4 minutes.
Check CronJob Status:
```Bash
    kubectl get cronjob call-api-cronjob -n mateapp
```
Check Job History:
Verify that successful jobs are kept in history (up to 10):
```Bash
    kubectl get jobs -n mateapp
```
Check CronJob Logs:
Find the name of a completed pod created by the CronJob:

```Bash
    kubectl get pods -n mateapp | grep call-api-cronjob
```
View the logs for that specific pod:

```Bash
    kubectl logs <pod-name> -n mateapp
```
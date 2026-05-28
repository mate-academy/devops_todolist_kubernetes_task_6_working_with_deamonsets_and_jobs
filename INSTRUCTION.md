# Instructions for deploying DaemonSet and CronJob in Kubernetes
This guide contains information on how to deploy and test the "DaemonSet" and "CronJob" components in namespace "mateapp".
---
## Using manifests

To create and start resources in the cluster, run the following commands from the root folder:
```bash
kubectl apply -f daemonset.yml
kubectl apply -f cronjob.yml
```

## 2. Validation and status check
To ensure that all components are working properly , use the following steps to check:

1. Check the status of the flow
Verify that the pods for DaemonSet and CronJob have been created successfully and are in the correct status(Running or Completed for completed CronJobs):

```bash
kubectl get pods -n mateapp
```

2. Checking logs
To monitor the execution of curl requests and check whether tasks have completed successfully, view the logs of the corresponding pods:
```bash
# View DaemonSet pod logs.(Replace <pod-name> with your pods name)
kubectl logs <pod-name> -n mateapp

# View logs of the completed CronJob (replace <cronjob-pod-name> with the name ofthe pod)
kubectl logs <cronjob-pod-name> -n mateapp

```

# Deployment Instructions

### Follow these steps to deploy the resources to the Kubernetes cluster.

#### 1. Deploy the DaemonSet
`kubectl apply -f .infrastructure/daemonset.yml`
#### 2. Deploy the CronJob
`kubectl apply -f .infrastructure/cronjob.yml`

### ✅ Solution Validation
After deployment, verify that the resources are running correctly by checking the logs.

#### 1. Validate DaemonSet
Check the status of the DaemonSet and its pods:

- `kubectl get ds`
- `kubectl get pods`

#### View DaemonSet logs:

`kubectl logs <DAEMONSET_POD_NAME>`

#### 2. Validate CronJob
Verify that the CronJob is scheduled and check the logs of the executed jobs:

`kubectl get cronjob`

To see the results of the health check, find the pod created by the last job execution:

`kubectl get pods`

#### View CronJob logs: Replace [POD_NAME] with the name of the pod found in the previous step:

`kubectl logs [POD_NAME]`